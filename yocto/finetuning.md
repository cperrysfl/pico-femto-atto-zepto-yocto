## Réglages fins

Note:
Quelques exemples de réglages fins que Yocto va faire à partir d'une
configuration haut-niveau.
---
## Machine tune

```verbatim
-mtune=name
	This option specifies the name of the target ARM processor
	for which GCC should tune the performance of the code.  For
	some ARM implementations better performance can be obtained
	by using this option.  Permissible names are: arm7tdmi,
	arm7tdmi-s, arm710t, ...
```
Note:
 * Possible d'optimiser la compilation pour la machine, ce qui n'est pas
   possible pour une distribution binaire car on ne connait pas la machine
   exacte à priori.
 * Impacte la compilation de tous les packages du système.
 * Généralement en incluant la layer BSP du fabricant d'un board et en
   spécifiant MACHINE="acme_machine_123", ceci se fait automatiquement.
---
## PACKAGECONFIG

Exemple: configuration de libSDL2

```sh
PACKAGECONFIG[alsa]       = "-DSDL_ALSA=ON,-DSDL_ALSA=OFF,alsa-lib,"
PACKAGECONFIG[arm-neon]   = "-DSDL_ARMNEON=ON,-DSDL_ARMNEON=OFF"
PACKAGECONFIG[directfb]   = "-DSDL_DIRECTFB=ON,-DSDL_DIRECTFB=OFF,directfb,directfb"
PACKAGECONFIG[gles2]      = "-DSDL_OPENGLES=ON,-DSDL_OPENGLES=OFF,virtual/libgles2"
PACKAGECONFIG[jack]       = "-DSDL_JACK=ON,-DSDL_JACK=OFF,jack"
PACKAGECONFIG[kmsdrm]     = "-DSDL_KMSDRM=ON,-DSDL_KMSDRM=OFF,libdrm virtual/libgbm"
PACKAGECONFIG[opengl]     = "-DSDL_OPENGL=ON,-DSDL_OPENGL=OFF,virtual/egl"
PACKAGECONFIG[pulseaudio] = "-DSDL_PULSEAUDIO=ON,-DSDL_PULSEAUDIO=OFF,pulseaudio"
PACKAGECONFIG[wayland]    = "-DSDL_WAYLAND=ON,-DSDL_WAYLAND=OFF,wayland-native wayland wayland-protocols libxkbcommon"
PACKAGECONFIG[x11]        = "-DSDL_X11=ON,-DSDL_X11=OFF,virtual/libx11 libxext libxrandr libxrender"
```
Note:
 * Plusieurs packages peuvent être configuré avant d'être compilé. C'est le temps
   d'activer ou désactiver certaines fonctions.
 * Avec la compilation native, les configurations sont auto-détectées et
   sélectionnées en fonction de ce qui est présent sur le système.
 * Avec Yocto, qui compile pour un système différent de la machine native, les
   options de configuration doivent être explicites.
 * Yocto expose les options de configuration dans `PACKAGECONFIG`. Certains
   `PACKAGECONFIG` vont s'activer automatiquement via `MACHINE_FEATURES` et
   `DISTRO_FEATURES` qui sont globales à la machine et à la distro.

---
## Système de fichier racine en lecture seule

```
IMAGE_FEATURES += "read-only-rootfs"
```

 * Garantit que tous les scripts post-installations s'exécutent avec succès et
   ne sont pas déférés au premier boot.

```
IMAGE_FSTYPES += "squashfs"
```

 * Ajoute la génération d'une image `squashfs` du système de fichier racine.

Note: Souvent utilisé en embarqué pour minimiser l'usure de la mémoire flash.
