## Un cas typique en embarqué

"Install Debian and add those files..."

```sh
.
├── bin
│   ├── pcs
│   ├── pim.db
│   └── readme
├── Install.sh
├── gpio
│   └── 99-gpio.rules
├── lib
│   ├── libmodbus.la
│   ├── libmodbus.so
│   ├── libmodbus.so.5
│   ├── libmodbus.so.5.1.0
│   └── readme
└── etc
    ├── sudoers
    └── readme
```

Note:
 * Mise en situation: un client nous envoie un .zip et nous dit "voici mon
   système Linux"
 * Le .zip contient plusieurs README, un semblant de rootfs, des fichiers
   binaires, des sources, des fichiers de config, une clé SSH, ...
 * 10 développeurs vont créer 10 systèmes Linux différents.
 * Bien sur comme c'est de l'embarqué, libmodbus va être quelque part la-dedans.

---
## Points négatifs

 * Procédure manuelle prône à l'erreur
 * Binaires de provenance inconnue
   - Version du code source?
   - Y a-t-il des patchs à appliquer?
   - Une configuration particulière à activer?
   - Avec quel compilateur?
   - Des options de compilation importantes?

Note:
Lire la slide

---
## Avec Yocto
```sh
SUMMARY = "A Modbus library"
DESCRIPTION = "libmodbus is a C library designed to provide a fast and robust \
implementation of the Modbus protocol. It runs on Linux, Mac OS X, FreeBSD, \
QNX and Windows."
HOMEPAGE = "http://www.libmodbus.org/"
SECTION = "libs"
LICENSE = "LGPLv2.1+"
LIC_FILES_CHKSUM = "file://COPYING.LESSER;md5=4fbd65380cdd255951079008b364516c"
SRC_URI = "http://libmodbus.org/site_media/build/${BP}.tar.gz"

inherit autotools pkgconfig

SRC_URI[md5sum] = "c80f88b6ca19cabc4ceffc195ca07771"
SRC_URI[sha256sum] = "046d63f10f755e2160dc56ef681e5f5ad3862a57c1955fd82e0ce036b69471b6"
```
Note:
 * Une recette pour la libmodbus d'il-y-a deux slides.
 * Comme libmodbus se compile et s'installe simplement (`./configure && make &&
   make install`), la recette est elle aussi simple (~10 lignes).
 * Les versions sont fixes (`${BP}.tar.gz`) et on a un checksum des sources.
 * S'il y a une quelconque précaution à prendre pour pallier à un problème de
   compilation croisé, un bug ou une vulnérabilité, celle-ci est présente sous
   forme de code exécutable et non comme une procédure manuelle dans un README.
