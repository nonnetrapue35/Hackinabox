
# Hackinabox (en cours de traduction)

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

## Annexe

- [1) Pour commencer](https://github.com/osx86-ijb/Hackinabox#1-getting-started)
- [2) Création de l'USB UnRAID](https://github.com/osx86-ijb/Hackinabox#2-making-the-unraid-usb)
- [3) Configuration de votre dispositif hôte](https://github.com/osx86-ijb/Hackinabox#3-setting-up-your-unraid-server-os-host)
- [4) Configuration de votre VM macOS](https://github.com/osx86-ijb/Hackinabox#4-setting-up-your-macos-vm)
- [5) Création de la clé USB de récupération sous Linux](https://github.com/osx86-ijb/Hackinabox#5-making-the-recovery-usb-on-linux)
- [6) Création de l'EFI](https://github.com/osx86-ijb/Hackinabox#6-making-the-efi) 
- [7) Installation de macOS](https://github.com/osx86-ijb/Hackinabox#7-installation-of-macos)
- [8) Finalisation post-installation](https://github.com/osx86-ijb/Hackinabox#8-post-installation-finalization--ssdt-setup)
- [9) Exemples de configuration du SSDT - Avant et après](https://github.com/osx86-ijb/Hackinabox/tree/master#9-ssdt-setup-examples---before--after)

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


## 4) Setting Up Your macOS VM

# !! **DISCLAIMER: YOUR LINE COUNT PLACINGS MIGHT NOT BE THE SAME AS IN THE GUIDE DUE TO ANY POTENTIAL DIFFERENCES IN HARDWARE CONFIGURATION, TAKE NOTE OF THIS AND BE ON THE LOOK OUT FOR THE DIFFERENCES** !!

- 4.1) **Now we're going to head to the "VMS" tab of the backend and click on the "ADD VM" button** ![4.1](https://i.ibb.co/hLhL1jR/12.png)
- 4.2) **When the Add VM page loads, we're going to select FreeBSD** ![4.2](https://i.ibb.co/NCYytQv/13.png)
- 4.3) **Once loaded, click "EDIT" button load the edit page and edit VM settings. When that next page loads, go to the "Logical CPUs" section, and select every other CPU thread combination except for CPU 0 / X (where as X is the variable for your specific core / thread count). After that, make sure to set your Max Memory. Do keep in mind that Unraid needs 4GB of RAM, so anything 4GB less than your Maximum installed RAM should be sufficient if you don't need to allocate more elsewhere.** ![4.3](https://i.ibb.co/0Qdsk5J/14-5.png)
- 4.4) **Next, scroll down on the page and select "3.0 qemu XHCI" from the drop down menu for USB Controller. This is really personal preference regarding the selection of USB Controller, and doesn't really matter since macOS doesn't support any of them. Then make sure to select your GPU from the Graphics Card drop down menu. Then select your appropriate Audio Chipset from the drop down menu in the selection named Sound Card.** ![4.4](https://i.ibb.co/ChvQdtR/14-6.png)
- 4.5) **Make sure that Network Bridge is set to "br0" from the dropdown menu, and Network Model is set to "virtio-net" from the appropriate drop down selection as well. Then place check marks next to the two(or more?) devices in the "Other PCI Devices" section that you want to pass through. Uncheck "Start VM after creation", and then click on the "CREATE" button PS: This is only to be done if the user isn't already passing through, or able to pass through their Ethernet Controller** ![4.5](https://i.ibb.co/wdP7V24/14-7.png)
- 4.6) **Click on FreeBSD icon, and choose "Edit". In the top right corner when the Edit page loads, you'll see a slider with the words "FORM VIEW" next to it.**  \
![4.6.1](https://i.ibb.co/4jFdyLP/14-8.png)  
**Then, click on that to change it to "XML VIEW".**  \
![4.6.2](https://i.ibb.co/t826xfx/14-9.png)
- 4.7) **After the XML view has loaded, we're going to go to line 37 and remove it (the entire line, that is) per what we have selected in the image here. This is done because QEMU's topology isn't read by macOS correctly and would require the topology kernel patch from the AMD kernel patches in order for it to work otherwise**  
![4.7](https://i.ibb.co/Qbq5j1R/15.png)
- 4.8) **Next, we're going to go to line 40 and select the text "utc",** ![4.8.1](https://i.ibb.co/xYFgx8D/17.png)  
**and change it to "localtime"**  
![4.8.2](https://i.ibb.co/5ry53yH/18.png)
- 4.9) **Scroll down to what should be line 63 and copy multifunction =‘on’,** ![4.9.1](https://i.ibb.co/GTCJncP/19.png) **and paste it at the end of line 118 as shown in the corresponding picture, making sure to change the last zero in function='0x0' to function='0x1' after you're done doing the prior pasting job, since it needs to be changed, also.** ![4.9.2](https://i.ibb.co/MZDrV16/20.png)
- 4.10) **On the line below 120 it says bus=‘0x03’ well at 127 change bus=‘0x04’ to 0x03 to match the gpu thats sets it as one device so the audio works.** ![4.10](https://i.ibb.co/MZDrV16/20.png) **Steps number 9 and number 10 are done to make your GPU and GPU audio device be seen to macOS as one device. The multifunction argument tells the hypervisor to allow multiple devices to operate on the same bus. Only the GPU audio device must match the GPU device bus and slot. The GPU audio device must use the function 0x1**
- 4.11) **Head to the bottom on line 144 click the end of </devices> and hit enter to make a new line (145) and paste this:** ![4.11](https://i.ibb.co/P5YDmZy/22.png) 
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
- 4.12) **After pasting in the required text into new line 145, please hit the "UPDATE" button. Now after hitting update button we can shut down and reboot into our Linux Live Distro of choice and continue with making the macOS Installer, if one already doesn't have one made previously.**


## 5) Making the Recovery USB on Linux

- 5.1) [Head to the OpenCore Install Guide page for making a macOS Installer USB via Linux to continue making your USB, and once finished, return here to continue](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html)


## 6) Making the EFI

- 6.1) [So after you've made your macOS Installer USB, please apply the OpenCore EFI from here to your macOS USB Installer's ESP/EFI partition, utilizing the proper folder hierarchies.](https://cdn.discordapp.com/attachments/469592019384270858/855930439343931452/EFI.zip)


## 7) Installation of macOS

- 7.1) **Create your USB based macOS installer as you normally would. I suggest using the ```createinstallmedia``` method on macOS.**
- 7.2) **Apply the EFI that can be obtained from this repo and in the previous step to the ESP partition of the created USB installer.**
- 7.3) **Boot from the created installer inside of booted VM instance of OpenCore.**
- 7.4) **Install your macOS version, and then boot into it after everything has finished! Make sure to copy over the EFI to the install drive so you can boot without using your macOS installer USB!**


## 8) Post Installation Finalization

- 8.1) **We're going to want to set up our hardware via inputting the correct information and values into the correct locations using both IORegistryExplorer to obtain the appropriate Address locations and naming, and MaciASL to edit the SSDT files and place in our hardware's corresponding information. Remember to test your changes non destructively so you don't bork your EFI, and have a working backup EFI to boot from!**
- 8.2) **Mount your EFI/ESP partiton using whatever means/software that you choose.**
- 8.3) **Obtain and open IORegistryExplorer, preferrably the newest one  if possible, although any version of at least 2.x should suffice.**
- 8.4) **Obtain and open MaciASL, preferrably the version from Acidanthera's GitHub repo.**
- 8.5) **oad each of the SSDT's, working on them one at a time so as not to convolute the process.**
- 8.6) **In each loaded SSDT, look for the corresponding Address and Device Name, and copy both sets of information to the corresponding SSDT that you are working on.**
- 8.7) 
- 8.8) 
- 8.9) 
- 8.10) 


## 9) SSDT Setup Examples - Before & After

- 9.1a) **SSDT-ARPT.aml Before Editing:** ![9.1a](https://i.ibb.co/1bVw8XP/SSDT-ARPT-aml.png)  
- 9.1b) **SSDT-ARPT.aml After Editing:** ![9.1b](https://i.ibb.co/3m211Ft/SSDT-ARPT-aml.png)
- 9.2a) **SSDT-GFX.aml Before Editing:** ![9.2a](https://i.ibb.co/sJw0fpp/SSDT-GFX-aml.png)
- 9.2b) **SSDT-GFX.aml After Editing:** ![9.2b](https://i.ibb.co/XD6Wn46/SSDT-GFX-aml.png)
- 9.3a) **SSDT-NVME.aml Before Editing:** ![9.3a](https://i.ibb.co/sWBjgNB/SSDT-NVME-aml.png)
- 9.3b) **SSDT-NVME.aml After Editing:** ![9.3b](https://i.ibb.co/zXC19jX/SSDT-NVME-aml.png)
- 9.4a) **SSDT-PLUG.aml Before Editing:** ![9.4a](https://i.ibb.co/9Y8XyCT/SSDT-PLUG-aml.png)
- 9.4b) **SSDT-PLUG.aml After Editing:** ![9.4b](https://i.ibb.co/TBTmc5s/SSDT-PLUG-aml.png)
- 9.5a) **SSDT-XGE.aml Before Editing:** ![!9.5a](https://i.ibb.co/j5ssrxd/SSDT-XGE-aml.png)
- 9.5b) **SSDT-XGE.aml After Editing:** ![9.5b](https://i.ibb.co/mqyPbc0/SSDT-XGE-aml.png)
- 9.6a) **SSDT-XHC.aml Before Editing:** ![9.6a](https://i.ibb.co/cT5RmmF/SSDT-XHC-aml.png)


# Support

For support, please join the AMD-OSX Discord Server and ask your question there, or join and ask at the AMD-OSX.com forums! One can also seek out assistance regarding Unraid related questions at the Unraid forums! Thank you, enjoy, and regards!
