## Requis pour l'embarqué
* Configuration unique
* Mise à jour robuste et atomique

La mise à jour par téléchargement de paquets ne répond à aucun de ces critères.

Note:
 * Configuration unique: systèmes embarqués accomplissent une tâche unique bien
   précise. La personnalisation d'une image par l'installation de paquets n'est
   pas nécessaire, voire nuisible.
 * atomique: systèmes embarqués sont généralement plus prompt aux ruptures
   d'alimentation et de réseau et doivent récupérer des fautes de façon
   autonome.
 * La mise à jour doit aussi être sécuritaire mais ce sont vraiment les 2 points
   mentionnés sur la slide qui justifie ce qui suit.
---
### Mise à jour symétrique

![](yocto/img/double_copy_layout.png)

source: [SWUpdate](https://sbabic.github.io/swupdate/overview.html)

Note:
 * Aussi connu sous le nom de mise à jour A/B
 * Configuration unique: l'ensemble du rootfs est mis à jour.
 * Atomique: On pivote sur la seconde banque à la toute fin en modifiant une
   variable du bootloader à la toute fin.
 * Ceci est une des alternatives viables, mentionnons également la mise à jour
   asymétrique (main/recovery) ou encore la mise à jour atomique basé sur des
   fichiers (libOSTree)
---
## Avec Yocto
* SWUpdate (https://github.com/sbabic/swupdate)
* Robust Auto-Update Controller (RAUC) (https://github.com/rauc/rauc)
* Mender.io (https://mender.io/)

Note:
 * Yocto contient des recettes pour plusieurs agents de mise à jour.
 * Yocto génère les artefact de mise à jour tel que requis par l'agent de mise à
   jour.
 * Yocto génère aussi des paquets (`.ipk` ou `.rpm`) mais ceux-ci sont
   principalements destinés à l'assemblage de l'image finale.
