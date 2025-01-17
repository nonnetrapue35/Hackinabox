
# Hackinabox

## Installation de MacOS avec UnRaid

Ce guide est destiné à l'utilisateur qui souhaite exécuter macOS à l'intérieur d'une VM sur UnRAID dans le but de contourner le besoin de multiples correctifs sur le matériel et les cartes mères basés sur AMD, tout en améliorant les objectifs de fournir l'expérience la plus native possible.

## Remerciements

 - [Lime Technology](https://twitter.com/limetechnology)
 - [UnRAID](https://unraid.net/)
 - [OpenCore Bootloader](https://github.com/acidanthera/OpenCorePkg)
 - [Dortania Team](https://dortania.github.io/)
 - [Pavo-IM](https://github.com/Pavo-IM/Hackinbox)

  
## Auteurs du Guide

- [@Pavo-IM](https://www.github.com/Pavo-IM)
- [@osx86-ijb](https://www.github.com/osx86-ijb)
- [@MattsCreative](https://twitter.com/MattsCreative1)

## Index

- [1) Pour commencer](https://github.com/nonnetrapue35/Hackinabox#1-d%C3%A9marrage-)
- [2) Création de l'USB UnRAID](https://github.com/nonnetrapue35/Hackinabox#2-pr%C3%A9paration-de-lusb-unraid-)
- [3) Configuration de votre dispositif hôte](https://github.com/nonnetrapue35/Hackinabox#3-configuration-de-votre-h%C3%B4te-unraid-server-os-)
- [4) Configuration de votre VM macOS](https://github.com/nonnetrapue35/Hackinabox#4-configuration-de-votre-vm-macos)
- [5) Création de la clé USB de récupération sous Linux](https://github.com/nonnetrapue35/Hackinabox#5-cr%C3%A9ation-de-la-cl%C3%A9-usb-de-r%C3%A9cup%C3%A9ration-sous-linux)
- [6) Création de l'EFI](https://github.com/nonnetrapue35/Hackinabox#6-cr%C3%A9ation-de-lefi) 
- [7) Installation de macOS](https://github.com/nonnetrapue35/Hackinabox#7-installation-de-macos)
- [8) Finalisation post-installation](https://github.com/nonnetrapue35/Hackinabox#8-finalisation-de-linstallation)
- [9) Exemples de configuration du SSDT - Avant et après](https://github.com/nonnetrapue35/Hackinabox#9-ssdt-setup-examples---before--after)

## FAQ

#### 1) Si je n'ai pas d'installation macOS existante à utiliser pour créer un programme d'installation hors ligne de macOS, mais que je suis déjà démarré dans unRAID, que puis-je faire pour y parvenir ?
[Use Macinabox from SpaceinvaderOne](https://github.com/SpaceinvaderOne/Macinabox)

#### 2) Si ma VM se fige et que je ne peux pas la redémarrer correctement à partir du backend unRAID et que je suis confronté à l'option de redémarrage brutal de mon ordinateur, que puis-je/doit-on faire ?
**ATTENTION:** 

*Le redémarrage forcé de l'ordinateur ou la réinitialisation de l'ordinateur et le fait de ne pas choisir d'arrêter l'ordinateur en utilisant l'option prévue à cet effet dans le backend de l'unRAID peuvent entraîner la corruption des données et la nécessité éventuelle de refaire la clé USB de l'unRAID. À tout prix, il faut toujours s'assurer d'utiliser le bouton SHUTDOWN dans le backend de l'unRAID pour arrêter votre ordinateur, au lieu de le redémarrer. Il serait également judicieux de s'assurer d'avoir un clone 1/1 de votre installation unRAID, juste au cas où le besoin s'en ferait sentir*.

  
## Fonctionnalités

- Il n'est pas nécessaire d'utiliser les correctifs d'AMD.
- Performance/utilisation complète du GPU supporté.
- Ethernet fonctionne OOB via la mise en place d'un dispositif de mise en réseau VirtIO, pas besoin de passer par des contrôleurs Ethernet physiques.
- Mise à jour facile
- Compatible avec macOS 12 Monterey

  
# Procédures d'installation


## 1) Démarrage :

- 1.1) [Télécharger Unraid Server OS USB Creator pour Mac/Windows](https://unraid.net/download)
- 1.2) **Lancer l'application Unraid Server OS USB Creator**.


## 2) Préparation de l'USB Unraid :

- 2.1) **Sous "sélectionner la version", sélectionnez "Stable", puis Unraid 6.9.2 ou une version plus récente selon le moment où vous lisez ceci** ![UnraidUSBSelectVersion_01](https://i.ibb.co/mX8cB7j/Unraid-USB-Creator-01.png)
- 2.2) **Cliquez sur "Customize" et cochez la case "Allow UEFI Boot "** ![UnraidUSBSelectVersion_02](https://i.ibb.co/tBkb1XG/Unraid-USB-Creator-02.png)
- 2.3) **Sélectionnez le lecteur USB que vous voulez créer comme votre USB Unraid, puis cliquez sur le bouton "Write" sous Write Image** ![UnraidUSBSelectVersion_03](https://i.ibb.co/CHb1j1m/Unraid-USB-Creator-03.png)
- 2.4) **Attendez que USB Creator finisse de créer la USB Unraid, puis redémarrez la machine et essayez de démarrer à partir de la USB Unraid nouvellement créée**.


## 3) Configuration de votre hôte Unraid Server OS :

- 3.1) **Booter à partir de votre Unraid Server OS USB**
- 3.2) **Sélectionnez la première option pour démarrer l'Unraid**.
- 3.3) **Entrez le nom d'utilisateur et le mot de passe = root**
- 3.4) A partir d'un autre appareil connecté au même réseau que votre OS Unraid Server démarré, allez sur http://tower.local dans le navigateur de votre choix.
- 3.5) **Sélectionnez le disque sur lequel vous souhaitez installer/stocker les fichiers du système d'exploitation du serveur Unraid dans le menu déroulant "Disk 1"** ![3.5](https://i.ibb.co/pychKXD/01.png)
- 3.6) **Aller en bas de la page après avoir sélectionné votre disque dans le menu déroulant Disk 1, et cliquer sur le bouton "START "** ![3.6](https://i.ibb.co/NndYd5w/02.png)
- 3.7) **(Il vous demandera ensuite à peu près au même moment de confirmer que vous voulez formater le disque, assurez-vous de l'accepter !)**.
- 3.8) **Après qu'il ait terminé, vous allez cliquer sur le texte cliquable qui dit "Flash "** ! [3.8](https://i.ibb.co/wBd6p1m/03.png)
- 3.9) **Dans la page qui se charge ensuite, faites défiler vers le bas jusqu'à la zone de texte verte intitulée "Unraid OS "** ![3.9](https://i.ibb.co/fQjQLQN/04.png)
- 3.10) **À l'intérieur de la zone de texte verte Unraid OS, vous verrez** `apend initrd=/bzroot` **Nous allons changer cela en** `append pcie_acs_override=downstream,multifunction video=efifb:off initrd=/bzroot` ! [3.10](https://i.ibb.co/JzJhxv8/05.png)
- 3.11) **Défilez jusqu'au bas de la page et cliquez sur le bouton "APPLY "** ! [3.11](https://i.ibb.co/wwBTKKD/06.png)
- 3.12) **Cliquez sur le bouton appelé "MAIN" en haut, et lorsque cette page se charge, faites défiler la page jusqu'en bas, et cliquez sur le bouton REBOOT** ! [3.12](https://i.ibb.co/TPP03Vw/07.png)
- 3.13) **Une fois que vous êtes redémarré dans votre Unraid Server OS à partir de votre clé USB Unraid Server OS, cliquez sur "TOOLS", puis "System Devices "**.  
![3.13](https://i.ibb.co/wcBNDqk/08.png)
- 3.14) **Une fois que le système de périphériques est chargé, vous devez vous assurer d'isoler votre USB Unraid sur son propre contrôleur USB, loin de tous les autres périphériques. Cela devrait pouvoir être fait sans avoir à redémarrer, mais si ce n'est pas possible de rafraîchir la liste des périphériques pour vous, alors vous pouvez éteindre et redémarrer entre chaque changement de port, en testant pour voir quel port permettra à l'USB Unraid d'être isolé de lui-même. Si nécessaire/possible, utilisez un port inférieur arrière, et un hub USB avec l'USB 
Unraid branché dessus si le branchement nu ne détecte pas l'USB lors du processus et de la tentative de redémarrage de l'Unraid USB)** ![3.14](https://i.ibb.co/2tqmPBB/09.png)
- 3.15) **Une fois que cela a été fait, assurez-vous de cocher les cases pour votre GPU, l'audio du GPU, tout chipset audio embarqué éventuellement supporté (si nécessaire), ainsi que votre contrôleur réseau (WiFi), et votre contrôleur NVME (ou si vous avez deux groupes SATA, liez le SSD/HDD que vous voulez), et appuyez sur le bouton "BIND SELECTED TO VFIO AT BOOT" en bas**.
- 3.16) **Une fois que c'est fait, vous devez retourner à l'onglet "MAIN" et aller au bas de la page et appuyer sur le bouton "REBOOT".
- 3.17) **Une fois que vous avez redémarré dans votre Unraid Server OS à partir de votre USB Unraid , allez en bas de l'onglet "MAIN" et assurez-vous d'appuyer sur le bouton "START" pour démarrer votre tableau.**
- 3.18) **Après cela, nous allons aller dans "SETTINGS", et ensuite sélectionner "Disk Settings "** ![3.18](https://i.ibb.co/Yj5GDG7/10.png)
- 3.19) **Une fois que nous sommes dans la page "Disk Settings", nous allons changer "Enable auto start" à "Yes", puis cliquer sur le bouton "APPLY "** ![3.19](https://i.ibb.co/9cW1Ltr/11.png)


## 4) Configuration de votre VM macOS

# ! ! **DISCLAIMER : VOTRE NOMBRE DE LIGNES POURRAIT NE PAS ÊTRE LE MÊME QUE CELUI INDIQUÉ DANS LE GUIDE EN RAISON DE DIFFÉRENCES POTENTIELLES DANS LA CONFIGURATION MATÉRIELLE, PRENEZ-EN NOTE ET SOYEZ ATTENTIF AUX DIFFÉRENCES** ! !!

 4.1) **Nous allons maintenant nous rendre dans l'onglet "VMS" du backend et cliquer sur le bouton "ADD VM "** ! 
 
 ![4.1](https://i.ibb.co/hLhL1jR/12.png)

- 4.2) **Lorsque la page "Add VM" se charge, nous allons sélectionner FreeBSD** !
 
![4.2](https://i.ibb.co/NCYytQv/13.png)

- 4.3) **Une fois chargée, cliquez sur le bouton "EDIT" pour charger la page d'édition et modifier les paramètres de la VM. Lorsque la page suivante se charge, allez à la section "Logical CPUs", et sélectionnez toutes les autres combinaisons de CPU threads à l'exception de CPU 0 / X (où X est la variable pour votre noyau spécifique / nombre de threads). Après cela, assurez-vous de définir votre mémoire maximale. Gardez à l'esprit que Unraid a besoin de 4 Go de RAM, donc tout ce qui est inférieur de 4 Go à votre RAM maximale installée devrait être suffisant si vous n'avez pas besoin d'allouer plus ailleurs. ** !
 
![4.3](https://i.ibb.co/0Qdsk5J/14-5.png)

- 4.4) **Puis, faites défiler la page et sélectionnez "3.0 qemu XHCI" dans le menu déroulant pour le contrôleur USB. Il s'agit d'une préférence personnelle concernant la sélection du contrôleur USB, et cela n'a pas vraiment d'importance puisque macOS ne supporte aucun d'entre eux. Assurez-vous ensuite de sélectionner votre GPU dans le menu déroulant de la carte graphique. Sélectionnez ensuite votre chipset audio approprié dans le menu déroulant de la sélection nommée Carte son.** ! 

![4.4](https://i.ibb.co/ChvQdtR/14-6.png)

- 4.5) **Assurez-vous que Network Bridge est défini sur "br0" dans le menu déroulant et que Network Model est défini sur "virtio-net" dans la sélection déroulante appropriée. Ensuite, cochez les deux (ou plus ?) périphériques de la section "Other PCI Devices" que vous voulez faire passer. Décochez "Start VM after creation", puis cliquez sur le bouton "CREATE" PS : Ceci ne doit être fait que si l'utilisateur ne passe pas déjà, ou n'est pas capable de passer par son contrôleur Ethernet** !

![4.5](https://i.ibb.co/wdP7V24/14-7.png)

- 4.6) **Cliquez sur l'icône FreeBSD, et choisissez "Edit". Dans le coin supérieur droit lorsque la page d'édition se charge, vous verrez un curseur avec les mots "FORM VIEW" à côté de lui.

![4.6.1](https://i.ibb.co/4jFdyLP/14-8.png)  

**Cliquez dessus pour le changer en "XML VIEWER".

![4.6.2](https://i.ibb.co/t826xfx/14-9.png)

- 4.7) **Après le chargement de la vue XML, nous allons aller à la ligne 37 et la supprimer (la ligne entière) en fonction de ce que nous avons sélectionné dans l'image ici. Ceci est fait parce que la topologie de QEMU n'est pas lue correctement par macOS et nécessiterait le patch du noyau de topologie à partir des patchs du noyau AMD afin de fonctionner autrement**.  

![4.7](https://i.ibb.co/Qbq5j1R/15.png)

- 4.8) **Puis, nous allons aller à la ligne 40 et sélectionner le texte "utc",** !

![4.8.1](https://i.ibb.co/xYFgx8D/17.png)  

**et le changer en "localtime "**.  

![4.8.2](https://i.ibb.co/5ry53yH/18.png)

- 4.9) **Défilez vers le bas jusqu'à ce qui devrait être la ligne 63 et copiez multifonction = 'on',** !

![4.9.1](https://i.ibb.co/GTCJncP/19.png) 

**et collez-la à la fin de la ligne 118 comme indiqué dans l'image correspondante, en vous assurant de changer le dernier zéro de function='0x0' à function='0x1' après avoir fait le travail de collage précédent, puisqu'il doit être changé aussi** !

![4.9.2](https://i.ibb.co/MZDrV16/20.png)

- 4.10) **Sur la ligne en dessous de 120 il est dit bus='0x03' bien à 127 changez bus='0x04' en 0x03 pour correspondre au gpu qui le définit comme un seul périphérique pour que l'audio fonctionne.** !

![4.10](https://i.ibb.co/MZDrV16/20.png) 

**Les étapes numéro 9 et numéro 10 sont faites pour que votre GPU et le périphérique audio du GPU soient vus par macOS comme un seul périphérique. L'argument multifonction indique à l'hyperviseur d'autoriser plusieurs périphériques à fonctionner sur le même bus. Seul le périphérique audio GPU doit correspondre au bus et à l'emplacement du périphérique GPU. Le périphérique audio du GPU doit utiliser la fonction 0x1**.
- 4.11) **Allez en bas de la ligne 144, cliquez sur la fin de </devices> et appuyez sur Entrée pour créer une nouvelle ligne (145) et collez ceci :** !

![4.11](https://i.ibb.co/P5YDmZy/22.png) 

```
     <qemu:commandline>  
     <qemu:arg value='-device'/>  
     <qemu:arg value='isa-applesmc,osk=ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc'/>  
     <qemu:arg value='-smbios'/>  
     <qemu:arg value='type=2'/>  
     <qemu:arg value='-cpu'/>  
     <qemu:arg value='host,vendor=GenuineIntel,+invtsc,kvm=on'/>  
     </qemu:commandline>  
```

- 4.12) **Après avoir collé le texte requis dans la nouvelle ligne 145, veuillez cliquer sur le bouton "UPDATE". Maintenant, après avoir appuyé sur le bouton de mise à jour, nous pouvons nous arrêter et redémarrer dans la distribution Linux Live de notre choix et continuer à créer l'installateur macOS, si nous n'en avons pas déjà créé un auparavant **.

## 5) Création de la clé USB de récupération sous Linux

- 5.1) [Rendez-vous sur la page du Guide d'installation d'OpenCore pour la création d'une clé USB d'installation de macOS via Linux pour continuer à créer votre clé USB, et une fois terminé, revenez ici pour continuer.](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html)


## 6) Création de l'EFI

- 6.1) [Donc, après avoir créé votre installation USB de macOS, veuillez appliquer l'OpenCore EFI d'ici à la partition ESP/EFI de votre installation USB de macOS, en utilisant les hiérarchies de dossiers appropriées.](https://cdn.discordapp.com/attachments/469592019384270858/855930439343931452/EFI.zip)


## 7) Installation de macOS

- 7.1) **Créez votre installateur macOS basé sur USB comme vous le feriez normalement. Je suggère d'utiliser la méthode ``createinstallmedia`` sur macOS.**
- 7.2) **Appliquez l'EFI qui peut être obtenu à partir de ce dépôt et dans l'étape précédente à la partition ESP de l'installateur USB créé.**
- 7.3) **Démarrez à partir de l'installeur créé à l'intérieur d'une instance VM d'OpenCore.**
- 7.4) **Installez votre version de macOS, puis démarrez-la une fois que tout est terminé ! Assurez-vous de copier l'EFI sur le disque d'installation afin de pouvoir démarrer sans utiliser votre USB d'installation de macOS !**.


## 8) Finalisation de l'installation

- 8.1) **Nous allons vouloir configurer notre matériel en entrant les informations et les valeurs correctes dans les emplacements corrects en utilisant IORegistryExplorer pour obtenir les emplacements et les noms d'adresses appropriés, et MaciASL pour éditer les fichiers SSDT et placer les informations correspondantes à notre matériel. N'oubliez pas de tester vos modifications de manière non destructive afin de ne pas bousiller votre EFI, et d'avoir une sauvegarde EFI fonctionnelle à partir de laquelle démarrer !**.
- 8.2) **Montez votre partiton EFI/ESP en utilisant le moyen/logiciel de votre choix.**
- 8.3) **Obtenez et ouvrez IORegistryExplorer, de préférence la plus récente si possible, bien que toute version d'au moins 2.x devrait suffire.**
- 8.4) **Obtenez et ouvrez MaciASL, de préférence la version du repo GitHub d'Acidanthera.**
- 8.5) **Chargez chacun des SSDT, en travaillant sur chacun d'entre eux un par un afin de ne pas alourdir le processus.
- 8.6) **Dans chaque SSDT chargé, cherchez l'adresse et le nom du dispositif correspondant, et copiez les deux ensembles d'informations dans le SSDT correspondant sur lequel vous travaillez.**
- 8.7) 
- 8.8) 
- 8.9) 
- 8.10) 


## 9) SSDT Setup Examples - Before & After

- 9.1a) **SSDT-ARPT.aml Avant d'éditer:** ![9.1a](https://i.ibb.co/1bVw8XP/SSDT-ARPT-aml.png)  
- 9.1b) **SSDT-ARPT.aml Après édition:** ![9.1b](https://i.ibb.co/3m211Ft/SSDT-ARPT-aml.png)
- 9.2a) **SSDT-GFX.aml Avant d'éditer:** ![9.2a](https://i.ibb.co/sJw0fpp/SSDT-GFX-aml.png)
- 9.2b) **SSDT-GFX.aml Après édition:** ![9.2b](https://i.ibb.co/XD6Wn46/SSDT-GFX-aml.png)
- 9.3a) **SSDT-NVME.aml Avant d'éditer:** ![9.3a](https://i.ibb.co/sWBjgNB/SSDT-NVME-aml.png)
- 9.3b) **SSDT-NVME.aml Après édition:** ![9.3b](https://i.ibb.co/zXC19jX/SSDT-NVME-aml.png)
- 9.4a) **SSDT-PLUG.aml Avant d'éditer:** ![9.4a](https://i.ibb.co/9Y8XyCT/SSDT-PLUG-aml.png)
- 9.4b) **SSDT-PLUG.aml Après édition:** ![9.4b](https://i.ibb.co/TBTmc5s/SSDT-PLUG-aml.png)
- 9.5a) **SSDT-XGE.aml Avant d'éditer:** ![!9.5a](https://i.ibb.co/j5ssrxd/SSDT-XGE-aml.png)
- 9.5b) **SSDT-XGE.aml Après édition:** ![9.5b](https://i.ibb.co/mqyPbc0/SSDT-XGE-aml.png)
- 9.6a) **SSDT-XHC.aml Avant d'éditer:** ![9.6a](https://i.ibb.co/cT5RmmF/SSDT-XHC-aml.png)


# Support

Pour obtenir de l'aide, veuillez rejoindre le serveur Discord d'AMD-OSX et y poser votre question, ou rejoindre et poser votre question sur les forums d'AMD-OSX.com ! Vous pouvez également chercher de l'aide concernant les questions relatives à Unraid sur les forums Unraid ! Merci, amusez-vous bien, et salutations !
