+++
pre = 'Semaine 2.1 : '
title = "Labo #1 - Installation d'AlmaLinux"
weight = 13
draft = false
+++

> [!grey]
> **À remettre au pus tard** :
> * **Groupe 5 lundi/jeudi** : lundi 2 février
> * **Groupe 6 mardi/jeudi** : mardi 3 février

---

## Objectif

* Créer un environnement isolé (sandbox) pour expérimenter avec Linux sans risquer d'abîmer votre système principal (Mint).  

## Outils 

* VirtualBox (devrait être déjà installé, sur votre SSD Linux Mint), 
* Le fichier ISO AlmaLinux 9.7 ou plus.


## Remise : Captures d'écran pour

1. Preuve que la VM AlmaLinux et installée et qu'elle fonctionne.  
2. Preuve que vous savez utiliser le terminal (ex.: la commande `ls -l`).  

 
## Installation

Voir le guide sur Moodle



<!--
> [!primary]
> Si vous avez **VMWare** au lieu de VirtualBox, utilisez [cette procédure](/procedure-alma95.pdf)


---
{{% expand %}}
Si vous n'avez pas Linux Mint sur votre SSD...
> Utilisez une clé USB (fournie par l'enseignante) pour l'installer sur votre SSD.
> Pour l'installation, suivez les étapes fournies sur Moodle (cours 420-ZE5-MO Environnements virtuels et réseaux)
{{% /expand %}}


### Étape 0 : Préparation

> [!À faire avant le laboratoire et avant de lancer VirtualBox)

Assurez-vous d'avoir téléchargé l'image disque d'installation.

* **Fichier :** `AlmaLinux-10.1-x86_64-dvd.iso`.
* **Emplacement :** Rangez-le dans votre dossier `Téléchargements`.

> [!primary]
> Un fichier `.iso` est l'équivalent numérique d'un DVD d'installation physique. On va "insérer" ce disque virtuel dans la machine virtuelle.

---

### Étape 1 : Création de la VM

1. Ouvrez **VirtualBox**.
2. Cliquez sur le bouton bleu **Nouvelle** (New).
3. Remplissez le formulaire comme suit :
   * **Nom :** `AlmaLinux-10-Prenom` (Mettez votre prénom, c'est plus facile pour le prof de vous identifier si vous partagez des captures).
   * **Dossier :** Laissez par défaut.
   * **Image ISO :** Cliquez sur la flèche, sélectionnez "Autre..." et allez chercher votre fichier ISO AlmaLinux téléchargé.
   * **Type :** `Linux`
   * **Version :** `Red Hat (64-bit)` (Alma est un clone de Red Hat).
> [!warning]
> Cochez la case **"Skip Unattended Installation"** (Sauter l'installation sans surveillance).
> **Pourquoi ?** Nous voulons faire l'installation manuellement pour **apprendre**, pas laisser VirtualBox choisir les mots de passe à notre place.

4. **Matériel (Hardware) :**
   * **Mémoire vive (RAM) :** `4096 MB` (4 Go).
      * *Note :* Ne dépassez pas la zone verte de la barre.
   * **Processeurs (CPU) :** `2` processeurs.

5. **Disque dur :**
   * Laissez "Créer un disque dur virtuel".
   * **Taille :** `20 Go`.
   * Cliquez sur **Finish**.


> [!orange]
> Prenez une capture d'écran



### Étape 2 : Optimisation pour Linux Mint

Avant de démarrer, nous devons ajuster les réglages graphiques pour que l'affichage soit fluide sur votre écran.

1. Sélectionnez votre nouvelle VM à gauche et cliquez sur **Configuration** (*Settings*).
2. Allez dans la section **Affichage** (*Display*) :
   * **Mémoire Vidéo :** Montez le curseur au maximum (`128 MB`).
   * **Contrôleur graphique :** Vérifiez qu'il est sur `VMSVGA`.
   * **Accélération 3D : Cochez cette case**. *Essentiel pour que l'interface moderne d'AlmaLinux ne "lague" pas*.

3. Allez dans la section **Système** > Onglet **Carte mère** :
   * Vérifiez que **Activer EFI** est coché. *C'est le standard moderne pour démarrer les OS, remplaçant le vieux BIOS*.

4. Cliquez sur **OK**.

> [!orange]
> Prenez une capture d'écran



### Étape 3 : L'Installation (Anaconda)

1. Cliquez sur **Démarrer** (*Start*).
2. Une fenêtre noire apparaît. Utilisez les flèches du clavier pour sélectionner (passer en blanc) :
   * `Install AlmaLinux 10.1`
   * Appuyez sur `Entrée`.

> [!primary]
> Si la souris est "capturée" par la fenêtre et que vous ne pouvez plus sortir, appuyez sur la touche **Ctrl Droite** de votre clavier.

Une fois l'interface graphique chargée :

1. **Langue :** Choisissez **Français** > **Français (Canada)**. Cliquez sur **Continuer**.
2. Vous arrivez sur le **Tableau de bord de l'installation**. Vous devez corriger tout ce qui a un ⚠️ ou un ❗.

   ##### 2.1. Destination de l'installation

   * Cliquez sur l'icône **Destination**.
   * Vérifiez que le disque virtuel de 20/25 Go est coché.
   * Ne touchez pas à "Configuration du stockage" (Laissez sur Automatique).
   * Cliquez sur **Fait/Terminé** (*Done*) en haut à gauche.

   ##### 2.2. Sélection des logiciels (**Crucial**)

   * Par défaut, c'est "Server with GUI". C'est bien, mais pour ce cours débutant, nous voulons un environnement de travail complet.
   * Sélectionnez **"Workstation"** (Station de travail) dans la liste de gauche.
   * Dans la liste de droite, cochez **"Outils de développement"** (*Development Tools*). Cela nous aidera plus tard.
   * Cliquez sur **Fait/Terminé** (*Done*).

   ##### 2.3. Réseau et nom d'hôte

   * Cliquez sur l'icône.
   * En haut à droite, **Activez l'interrupteur** (ON) pour connecter la carte réseau virtuelle. Vous devriez voir une adresse IP apparaître.
   * En bas, changez `localhost` pour : `alma-etudiant`.
   * Cliquez sur **Appliquer** puis sur **Fait/Terminé** (*Done*).

   ##### 2.4. Création de l'utilisateur

   * **Ne définissez PAS le mot de passe Root** (C'est une bonne pratique de sécurité moderne de désactiver le compte root direct).
   * Cliquez sur **Création de l'utilisateur**.
   * **Nom complet :** Votre prénom et votre nom.
   * **Nom d'utilisateur :** Première lettre de votre prénom, suivi de votre nom, tout en minuscules (ex:`ndesmangles`).
   * **Cochez : "Faire de cet utilisateur un administrateur"**.
      * *Note :* Cela vous donnera le droit d'utiliser la commande `sudo` (SuperUser DO).
   * **Mot de passe :** Choisissez un mot de passe simple pour le cours (ex: `linux`).
   * Cliquez sur **Fait/Terminé** (*Done*).

3. Cliquez sur le bouton **Commencer l'installation**.
   * **Note :** L'installation prendra 5 à 10 minutes.

4. Une fois terminé, cliquez sur **Redémarrer le système**.



### Étape 4 : Post-installation & "Guest Additions"

Au redémarrage, connectez vous. Vous remarquerez que l'écran est petit et ne remplit pas votre moniteur. Nous allons installer les "pilotes" VirtualBox pour régler ça.

1. Ouvrez le **Terminal** dans AlmaLinux (*Activités > Terminal*).
2. Tapez les commandes suivantes (appuyez sur Entrée après chaque ligne et entrez votre mot de passe si demandé) :
```bash
# Mise à jour rapide du système
sudo dnf update -y

# Installation des outils nécessaires pour compiler les pilotes
sudo dnf install kernel-devel gcc make perl -y
```

*(Si une mise à jour du noyau/kernel a eu lieu, redémarrez la VM avec `sudo reboot` avant de continuer).*

3. Une fois prêt, regardez le menu de la fenêtre **VirtualBox** (en haut de la fenêtre de la VM, pas dans Linux).
   * Allez dans **Périphériques** > **Insérer l'image CD des Additions Invité**.

4. Retournez dans le terminal d'AlmaLinux :
```bash
# On va dans le dossier du CD
cd /run/media/$USER/VBox_GAs_*

# On lance l'installation
sudo ./VBoxLinuxAdditions.run
```

5. Attendez que ça finisse, puis redémarrez une dernière fois :
```bash
sudo reboot
```

**Bravo !** Vous pouvez maintenant agrandir la fenêtre, passer en plein écran, et faire des copier-coller entre votre Linux Mint et votre AlmaLinux.

---

### 🆘 Dépannage fréquent pour les hôtes Mint

**Problème :** VirtualBox ne me laisse pas choisir "64-bit" ou plante au démarrage de la VM.
**Solution :** C'est souvent parce que la virtualisation est désactivée dans le BIOS de votre ordinateur physique (cherchez "Intel VT-x" ou "AMD-V" dans votre BIOS).

**Problème :** Je n'ai pas accès aux ports USB.  
**Solution (sur votre machine hôte) :** Ouvrez un terminal sur votre Mint (pas dans la VM) et tapez :
`sudo usermod -aG vboxusers $USER`  
Puis redémarrez votre ordinateur complet.

==========================================================================================

## Prérequis

Avant de commencer, assurez-vous d'avoir :

1. **VirtualBox** installé sur votre SSD (voir guide du cours "ZE5-Environnements virtuels et réseau" disponible sur Moodle)
2. **L'image ISO d'AlmaLinux (DVD)**.
   * [*Lien de téléchargement :*](https://almalinux.org/get-almalinux/)
      -> Télécharger -> L'**ISO DVD** (environ 9-10 Go).

      ![DVD ISO](../almalinux-dvd-iso.jpg?width=35vw)


## Remise : Captures d'écran pour

> Preuve que la VM AlmaLinux et installée et qu'elle fonctionne.  
> Preuve que vous savez utiliser le terminal (ex.: la commande `ls -l`).  
> Preuve que l'exercice à la fin de l'atelier est fait.  
-->

<!-- 
---

### Partie 1 : Création de la machine virtuelle

Nous allons d'abord construire l'ordinateur virtuel avant d'y installer le système.

1. Ouvrez **VirtualBox**.
2. Cliquez sur l'icône bleue **"Nouvelle"** (ou "New").

 ![Nouvelle VM](../1-nouvelleVM.jpg?width=35vw)

3. **Nom et OS :**
   * **Nom :** `AlmaLinux-ZG4`
   * **Dossier :** Laissez par défaut.
   * **Type :** Linux
   * **Version :** Red Hat (64-bit) *(AlmaLinux est un clone de Red Hat)*.

 ![Nom et OS](../3-coquille.jpg?width=35vw)

4. **Mémoire (RAM) :**
   * Mettez 4096 Mo (4 Go) pour que ce soit fluide.
5. **Disque dur :**
   * Sélectionnez "Créer un disque dur virtuel maintenant".
   * Type de fichier : **VDI** (VirtualBox Disk Image).
   * Stockage : **Dynamiquement alloué** (prendra peu de place au début et grossira selon les besoins).
   * Taille : Mettez **20 Go** minimum (le système en prendra déjà 5 ou 6).

> [!primary]
> **Capture #1** - Preuve que la VM a été crée.

### Partie 2 : Insertion du CD d'installation

Votre machine est créée, mais elle est vide. Il faut mettre le CD (l'ISO) dans le lecteur.

1. Sélectionnez votre VM `AlmaLinux-ZG4` dans la liste à gauche.
 ![Config](./almalinux-dvd-iso.jpg?width=35vw)
2. Cliquez sur **Configuration** (Settings) -> **Stockage** (Storage).
3. Dans la zone "Contrôleur : IDE", cliquez sur l'icône de CD qui dit **"Vide"**.
4. À droite, cliquez sur la petite icône de disque bleu -> **Choose a disk file...**
5. Allez chercher le fichier **`.iso`** d'AlmaLinux que vous avez téléchargé.
6. Cliquez sur OK.


### Partie 3 : L'installation

1. Cliquez sur la grosse flèche verte **Démarrer** (Start).
2. Une fenêtre noire apparaît. Utilisez les flèches de votre clavier pour sélectionner :
`Install AlmaLinux 9.x` et appuyez sur **Entrée**.
*(Si la souris est capturée par la fenêtre, appuyez sur la touche **Ctrl droite** de votre clavier pour la libérer).*

#### L'installateur Anaconda

Après quelques lignes de code qui défilent, vous arrivez sur l'interface graphique.

1. **Langue :** Choisissez "Français" -> "Français (Canada)". Cliquez sur "Continuer".
2. Vous arrivez sur le **Tableau de bord de l'installation**. Vous verrez des panneaux avec des points d'exclamation oranges. Il faut les régler.
3. **Destination de l'installation (Disque) :**
   * Cliquez sur l'icône du disque dur.
   * Assurez-vous que votre disque virtuel de 20 Go est coché (il a un petit crochet).
   * Laissez "Configuration du stockage" sur **Automatique**.
   * Cliquez sur **Fait** (Done) en haut à gauche.


4. **Sélection des logiciels :**
   * Par défaut, c'est sur "Serveur avec GUI" (Interface graphique). Laissez ça pour l'instant. Cela vous donnera un bureau type Windows/Mac, mais nous utiliserons le Terminal.


5. **Réseau et nom d'hôte (TRÈS IMPORTANT) :**
   * C'est l'erreur classique : par défaut, la carte réseau est éteinte.
   * Cliquez sur "Réseau et nom d'hôte".
   * En haut à droite, **basculez l'interrupteur sur ON**. Vous devriez voir une adresse IP apparaître (ex: 10.0.2.15).
   * Cliquez sur **Fait**.


6. **Paramètres utilisateur (Création du compte) :**
   * Cliquez sur **Création de l'utilisateur**.
   * Nom complet : `votre nom` 
   * Nom d'utilisateur : `**première lettre de votre prénom**, suivi de votre **nom**` (tout en minuscule, c'est mieux). (**OBLIGATOIRE**)
   * Cochez la case : **[x] Faire de cet utilisateur un administrateur** (Indispensable pour utiliser la commande `sudo`).
   * Mot de passe : Choisissez quelque chose de simple, facile à retenir pour les labo.
   * Cliquez sur **Fait** (deux fois si le mot de passe est trop simple).


7. **Lancer l'installation :**
   * Cliquez sur le bouton **Commencer l'installation** en bas à droite.
   * Cela va prendre 5 à 15 minutes selon la vitesse de votre SSD.



### Partie 4 : Premier démarrage

1. Une fois fini, cliquez sur **Redémarrer le système**.
2. **Note importante :** VirtualBox est intelligent et devrait éjecter le fichier ISO automatiquement. Si vous retournez sur l'écran d'installation, éteignez la machine, allez dans **`Configuration -> Stockage -> Retirez le disque du lecteur virtuel`**, puis rallumez.
3. Au démarrage, vous verrez un écran de connexion.
4. Cliquez sur votre utilisateur `prenom-nom`, entrez votre mot de passe.
5. Bienvenue sur le bureau GNOME d'AlmaLinux !



### Partie 5 : Connexion au Terminal

Puisque nous sommes là pour apprendre la ligne de commande :

1. Appuyez sur la touche **Windows** (ou "Super") de votre clavier.
2. Tapez `Terminal` dans la barre de recherche.
3. Cliquez sur l'icône noire **Terminal**.

Vous voilà devant l'invite de commande :
`[prenom-nom@localhost ~]$`

**Testez vos nouvelles connaissances :**
Essayez de recréer l'exercice de la semaine dernière ici même :

```bash
mkdir -p Exercice/Planetes
cd Exercice
pwd
```

Félicitations ! Vous avez maintenant un environnement Linux professionnel complet et isolé pour faire toutes vos expérimentations sans risque de casser votre ordinateur principal. 
-->

<!-- -->