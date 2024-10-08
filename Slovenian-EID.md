# Slovenian EID (eOI)

Slovenian EID (eOI) is a card manufactured by NXP. They also provide the software solution IDprotect adopted for SI-TRUST. There are versions for Windows and Mac accessible from <https://www.si-trust.gov.si/sl/eoi/programska-oprema-idprotect-client/>.

Support for Linux was planned at the beginning, but with time it was removed.

## The card

The eOI has contact and contactless support over NFC. NFC requires CAN to enable access. On the card there are multiple apps for different levels of security. First app (`E828BD080F014E585031`) has one token for low security use that does not require PIN to access.
Second app (`E828BD080F014E585030`) has two high security tokens. One token for identification and one token for signing. Both need PIN to access.
All keys on the card are 384 bit EC. All certificates have SI-TRUST as issuer. CA and ROOT certs are on the card. ROOT cert is self signed.

## OpenSC support

OpenSC includes EOI driver that supports the card from version 0.24.0. It is available for Windows, Mac and Unix. If your distribution on Linux does not have prebuilt package for OpenSC 0.24.0 or newer, it can be built from source, see [Compiling-and-Installing-on-Unix-flavors](Compiling-and-Installing-on-Unix-flavors). The driver will be built only if [OpenPACE](https://frankmorgner.github.io/openpace/) is present with development extended libraries for it.

With no configuration all apps and tokens are available to the user with PKCS#11 interface. To use authentication cert with a browser, you need configure PKCS#15 framework to enable just the app (`E828BD080F014E585030`) containing authentication token (Podpis in prijava (Norm PIN)).

With OpenSC apps can be configured separately. To access high security tokens in browser the opensc.conf should include the following in app default section:

```C
framework pkcs15 {
        # Slovenian eID - low level (pinless, "Prijava brez PIN-a")
        application E828BD080F014E585031 {
                model = "ChipDocLite";
                disable = true;
                user_pin = "Norm PIN";
        }

        # Slovenian eID - high level (QES, "Podpis in prijava")
        application E828BD080F014E585030 {
                model = "ChipDocLite";
                # disable = true;
                user_pin = "Norm PIN";
                sign_pin = "Sig PIN";
        }
}
```

To use low security tokens use:

```C
framework pkcs15 {
        # Slovenian eID - low level (pinless, "Prijava brez PIN-a")
        application E828BD080F014E585031 {
                model = "ChipDocLite";
                #disable = true;
                user_pin = "Norm PIN";
        }

        # Slovenian eID - high level (QES, "Podpis in prijava")
        application E828BD080F014E585030 {
                model = "ChipDocLite";
                disable = true;
                user_pin = "Norm PIN";
                sign_pin = "Sig PIN";
        }
}
```
