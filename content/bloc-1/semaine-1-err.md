+++
title = "Erreur2-Installation AlmaLinux"
weight = 300
draft = true
+++

![Erreur 2](../err2-install-alma.jpg?width=45vw)

Cette erreur (`rc=-1908`) est l'un des problèmes les plus courants sur VirtualBox sous Linux. Voici une explication des sources du problème et les étapes pour le corriger.

### 1. Les sources du problème

Il y a deux causes principales pour ce message d'erreur :

* **Mise à jour du noyau (Kernel) :** Votre système Linux (l'hôte) a probablement mis à jour son noyau récemment. VirtualBox a besoin de modules spécifiques pour communiquer avec ce noyau. Si ces modules n'ont pas été recompilés pour la *nouvelle* version du noyau, VirtualBox ne peut pas démarrer.
* **Secure Boot (Démarrage sécurisé) :** Comme indiqué dans le message d'erreur ("EFI Secure Boot enabled"), si le "Secure Boot" est activé dans le BIOS de votre ordinateur, il empêche le chargement des pilotes VirtualBox car ils ne sont pas signés numériquement par une autorité de confiance (comme Microsoft).

---

### 2. Comment corriger le problème

Je vais vous proposer deux solutions. Commencez par la **Solution A**, car c'est la plus fréquente.

Le nom de votre fichier image mentionne "AlmaLinux", mais le message d'erreur ("reinstall virtualbox-dkms") suggère que votre système principal (l'hôte) est peut-être basé sur **Ubuntu/Debian** ou que vous utilisez **AlmaLinux** comme hôte. Je vais couvrir les deux cas.

#### Solution A : Recompiler les modules du noyau

Ouvrez votre terminal et suivez les instructions selon votre système d'exploitation **Hôte** (celui sur lequel VirtualBox est installé).

**Option 1 : Si votre système hôte est Ubuntu, Debian, ou Linux Mint**
Le message d'erreur suggère spécifiquement ce cas.

1. Mettez à jour vos dépôts et installez les outils nécessaires :
```bash
sudo apt-get update
sudo apt-get install build-essential dkms linux-headers-$(uname -r)

```


2. Réinstallez le paquet mentionné dans l'erreur :
```bash
sudo apt-get install --reinstall virtualbox-dkms
sudo modprobe vboxdrv
```


*Si la commande `modprobe` ne renvoie aucune erreur, essayez de relancer VirtualBox.*

<!--

**Option 2 : Si votre système hôte est AlmaLinux, Fedora ou CentOS**

1. Installez les outils de développement et les en-têtes du noyau :
```bash
sudo dnf install kernel-devel-$(uname -r) kernel-headers gcc make perl

```


2. Lancez le script de configuration de VirtualBox :
```bash
sudo /sbin/vboxconfig

```


*Note : Regardez si le script termine avec succès (OK).*

---

-->

#### Solution B : Désactiver le *Secure Boot* (Si la solution A échoue)

Si vous avez exécuté les commandes ci-dessus et que vous voyez un message d'erreur mentionnant "Key is rejected" ou "Operation not permitted" lors du chargement du module, c'est le **Secure Boot** qui bloque.

La méthode la plus simple (plutôt que de signer manuellement les modules, ce qui est complexe) est la suivante :

1. Redémarrez votre ordinateur.
2. Entrez dans le **BIOS/UEFI** (généralement en appuyant sur F2, F10, F12 ou Del au démarrage).
3. Cherchez l'onglet **Boot**, **Security** ou **Authentication**.
4. Trouvez l'option **Secure Boot** et passez-la sur **Disabled**.
5. Sauvegardez et quittez (F10).

Une fois de retour sous Linux, retentez la commande suivante dans le terminal pour charger le pilote :

```bash
sudo modprobe vboxdrv
```

---

### Résumé technique pour le dépannage

Si cela ne fonctionne toujours pas, le message d'erreur vous invite à regarder un journal spécifique. Vous pouvez voir la cause exacte de l'échec de la compilation en tapant cette commande :

```bash
cat /var/log/vbox-setup.log
```