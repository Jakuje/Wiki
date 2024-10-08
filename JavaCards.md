# Java Cards

OpenSC (including initialization) works with JavaCards if you have a supported applet on the card.

JavaCards can come in different flavors:

* Empty (preferred)
* Pre-loaded with an applet in EEPROM
* With an applet in ROM
* With a pre-loaded applet in a finalized state (applets can't be deleted or added).

Some cards, for example older IBM JCOP  or older Cyberflex, come pre-loaded with a filesystem (PKCS#15) applet, which are of no interest in the broader context of JavaCards.

The *core* of OpenSC is a bunch of card drivers, both read-only drivers as well as PKCS#15 initialization drivers. It is important to realize, that all of the initialization drivers target a single card (usually proprietary) which is closely tied to the card vendor.  Open source is all about providing options and being tied to a card vendor (even if the card comes with good documentation) has the same advantages and disadvantages as some popular binary-only display drivers provided by the vendor: it is not possible to change the way the card behaves. Therefore it is desirable to have open source code both inside the card and on the host computer and use JavaCards.

The main difference between "native" cards and JavaCards is the requirement to install the proper application to the card before continuing with OpenSC, which has historically been a somewhat complicated procedure and what this page tried to demystify.

List of applets OpenSC supports (can be read-only and locked cards):

* [IsoApplet](https://github.com/philipWendland/IsoApplet)
  * General PKCS#15 filesystem and PKI operations.
  * Requires Java Card 3.0.4 or newer for v1, Java Card 2.2.1 or newer for legacy branch, forks with 2.2.1 support exist.
  * `pkcs15-init -E` to erase then `pkcs15-init -C` to create a filesystem, PIN, and PUK. Then `pkcs15-tool` for any further operations.
* [GidsApplet](https://github.com/vletoux/GidsApplet)
  * Also compatible with Windows built-in drivers, for most features.
  * Requires Java Card 2.2.1 or newer.
  * `gids-tool --initialize` to initialize, set 48-char hex string as admin key, and set a user pin. Then `gids-tool`, `pkcs15-init`, and `pkcs15-tool` for any further operations.
* [SmartPGP](https://github.com/github-af/SmartPGP)
  * OpenPGP card 3.4 implementation. Java Card 3.0.4 or newer.
  * Serial number is embedded in the applet ID: see [how to set the serial number at install time](https://github.com/github-af/SmartPGP/issues/52) using `gp`.
  * Use CLI tool in its source repo to set key size, then `openpgp-tool` or `gpg --card-edit` to set up.
* [PivApplet](https://github.com/arekinath/PivApplet)
  * PIV (NIST SP 800-73-4) compatible JavaCard applet.
  * JavaCard 2.2.2 or newer.
* MuscleApplet (deprecated)

## Supported cards

Things to consider when buying JavaCards, please have a look at [JavaCard Buyer's Guide](https://github.com/martinpaljak/GlobalPlatformPro/tree/master/docs/JavaCardBuyersGuide)

## Loading the applet

After you have fetched a suitable applet for your card (pay attention to JavaCard version and card peculiarities like Cyberflex cards), you'll need to load the software to the card.  Here's how to do it.

## Required software

A GlobalPlatform compliant software is needed for loading the applet to the card. Card vendors also provide tools for loading applets but also open source alternatives exist.

* GlobalPlatformPro - <https://github.com/martinpaljak/GlobalPlatformPro> - requires Java 1.8+

## Interesting JavaCard applets

The following Open Source applets are fully supported for use and initialization with OpenSC:

* <https://github.com/vletoux/GidsApplet>
* <https://github.com/philipWendland/IsoApplet>
* <https://github.com/Yubico/ykneo-openpgp>
* <https://github.com/arekinath/PivApplet>

Open source applets possibly usable (with some work) with OpenSC:

* [CoolKey Applet (MuscleApplet fork)](https://github.com/dogtagpki/coolkey/)
* [JavaCardSign PKCS#15 applet](http://sourceforge.net/projects/javacardsign/)
* [OpenPGP](http://sourceforge.net/projects/jopenpgpcard/) applet:, related [wiki page](OpenPGP-card) and a somewhat matching [`javax.smartcardio` GUI](http://sourceforge.net/projects/javaopenpgpcard/)

Other interesting applets:

* [MRTD (biometric passport) applet](http://sourceforge.net/projects/jmrtd/), from JMRTD
* [ISO18013 driving license applet](http://sourceforge.net/projects/isodl/)
* [Web server in Java Card](http://www.citi.umich.edu/techreports/reports/citi-tr-99-3.pdf)

List of Java Card open source applets can be found at <https://github.com/crocs-muni/javacard-curated-list>.

## Resources

* [Writing a Java Card Applet](https://www.oracle.com/java/technologies/java-card/writing-javacard-applet.html)
* [A curated list of applets](https://github.com/EnigmaBridge/javacard-curated-list)
