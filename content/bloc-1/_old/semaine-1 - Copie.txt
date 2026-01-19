+++
pre = 'Semaine 1 : '
title = 'Introduction et installation'
weight = 10
+++


### Objectif de la semaine

* Avoir une machine Linux fonctionnelle et comprendre pourquoi on l'utilise.


---

# Théorie


## Qu'est-ce que Linux ?

Ce n'est pas juste "un autre Windows". C'est l'OS qui fait tourner Internet, le Cloud, Android et les supercalculateurs. Votre routeur Wi-Fi, c'est Linux. Si vous voulez travailler en TI, vous ne pouvez pas éviter ce pingouin.

> En date de décembre 2025, **"96,3 %** du top 1 million des serveurs web tournent sous Linux. 


* **Philosophie Open Source :**
* **Windows/macOS :** Code fermé (propriétaire). Vous louez le logiciel, vous ne pouvez pas voir comment il est fait.
* **Linux :** Code ouvert. C'est comme une recette de cuisine publique : tout le monde peut la copier, la modifier et la redistribuer .
* **Analogie :** Windows, c'est un restaurant (on mange, on paie, on ne va pas en cuisine). Linux, c'est un "Potluck" communautaire (on apporte, on modifie, on partage).



## Architecture : Le moteur et la carrosserie

Il faut distinguer le **Noyau** du **Système d'exploitation** .

* **Le Kernel (Noyau) :** C'est le chef d'orchestre. Il parle directement au matériel (CPU, RAM, Disque). C'est le "moteur".
* **Le Shell (Coquille) :** L'interface textuelle qui permet à l'humain de parler au Noyau. C'est le "volant".
* **La Distribution (Distro) :** C'est le package complet (Moteur + Volant + Sièges en cuir + Climatisation).
	* **Ubuntu** (Grand public), 
	* **RedHat** (Entreprise), 
	* **Kali** (Sécurité), 
	* **Mint** (<span style="color:blue;"><b>Notre choix</b></span>: stable avec une interface proche de Windows pour débuter).



## La virtualisation (cours 420-ZE5-MO)

Pourquoi on n'installe pas Linux directement sur vos portables aujourd'hui ? Pour éviter la catastrophe.

* **Concept :** Un ordinateur "fantôme" qui tourne à l'intérieur de votre vrai ordinateur .
* **Vocabulaire technique :**
	* **Hôte (Host) :** Votre machine physique (Windows/Mac).
	* **Invité (Guest) :** La machine virtuelle (Linux Mint).
	* **Hyperviseur :** Le logiciel qui fait le pont (VirtualBox).
* **Avantage majeur :** Le "Sandbox" (Bac à sable). Si vous brisez Linux, vous supprimez juste un fichier. Votre Windows reste intact.

---

# Exercice (en classe)

### Exercice 1

Utiliser le fichier **exo-semaine1.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine1.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}


* **Audit RAM** : Avant de créer la VM, on doit savoir ce qu'on peut se permettre.

**Consignes** :

1. Ouvrez votre **Gestionnaire des tâches** (Ctrl+Shift+Esc) ou **Moniteur d'activité** (Mac).
2. Onglet **Performance** > **Mémoire**.
3. Répondez à ces 2 questions sur une feuille/Note :
   * Combien de RAM **Totale** avez-vous ? (ex: 16 Go)
   * Combien de RAM est **Disponible** maintenant ? (ex: 9 Go)
4. **Décision :**
   * Si vous avez > 8 Go de libre : Donnez **4096 Mo (4 Go)** à la VM.
   * Si vous avez < 4 Go de libre : Donnez **2048 Mo (2 Go)** à la VM (Minimum vital).


### Préparation au laboratoire (30 min)

> > [!warning]
> * Vous devez avoir terminé cette étape **avant de quitter**.  
> * Si ce n'est pas fait, vous n'aurez pas le temps de terminer le laboratoire (séance #2).

1. Télécharger le fichier ISO de Linux Mint (Édition Cinnamon)

      * [**Lien > Édition Cinnamon**](https://www.linuxmint.com/edition.php?id=322)
      * Choisir un *Miroir* canadien de préférence. Vous obtiendrez le fichier `linuxmint-22.2-cinnamon-64bit.iso`.

2. Création d'une clé bootable Linux Mint **(À FAIRE)**


Utiliser le fichier **Creation-cle-bootable.pdf** {{% button href="/docs/Guides/Creation-cle-bootable.pdf" icon="download" %}}Télécharger le fichier pdf{{% /button %}}


   * Créer la clé USB bootable avec Rufus en sélectionnant l'ISO de Mint et votre clé USB (attention, toutes les données seront effacées).
   * Débrancher tous les autres disques durs internes (Windows inclus) pour ne laisser que le SSD externe branché à l'ordinateur. 

> Cette vidéo vous montre comment créer une clé USB bootable avec Rufus :
[Vidéo: Installation de Linux Mint 22](https://youtu.be/XynoQjL2QWo?si=Yni_7dDvSYCpicjy)

<!--
> [!IMPORTANT]
> Si vous n'êtes pas inscrit.e.s au cours **420-ZE5-MO**: *Environnements virtuels et réseau* et que vous avez déjà installé Linux (*Mint Cinnamon*) et VirtualBox sur votre SSD, avant de quitter, vous devrez montrer le bon fonctionnement de votre système (Linux + VirtualBox)

**AVANT** la séance de laboratoire.


### Guide d'installation (format PDF)

Pour installer Linux Mint sur un SSD externe, vous devez créer une **clé USB d'installation bootable**, démarrer votre PC sur cette clé, puis, lors de l'installation, sélectionner le **SSD externe** comme cible, en veillant à bien déconnecter tout autre disque dur (surtout Windows) pour éviter les erreurs, et en faisant attention au choix du disque pour le chargeur d'amorçage (bootloader), souvent en utilisant l'option avancée pour le placer sur le SSD externe. 
-->



**Installation**
1. **Démarrer sur la clé USB** en utilisant le menu de démarrage (touche F12, F10, DEL, ou ESC au démarrage) et choisir la clé d'installation.
2. **Lancer l'installateur** depuis le bureau de Linux Mint.
3. **Choisir le type d'installation** : Pour un contrôle total, optez pour l'installation personnalisée ("Something Else").
4. **Partitionner le SSD externe** : Créez au moins une partition racine (/) sur le SSD.
5. **Installer le chargeur d'amorçage (GRUB)** : C'est crucial : dans la section "Périphérique où installer le programme de démarrage", sélectionnez votre SSD externe (ex: /dev/sda, pas une partition comme /dev/sda1) et non votre disque interne.
6. **Suivre les étapes** (fuseau horaire, utilisateur, mot de passe). 

> Voici une démonstration de l'installation de Linux Mint sur un disque externe :
[Video: Comment installer facilement Linux Mint](https://youtu.be/rqQMQAbD3i4?si=rIhM84fi3VA1r2Pw)

**Premier démarrage**
1. **Redémarrer le PC** et retirer la clé USB d'installation.
2. **Accéder au menu de démarrage** (F12, etc.) et sélectionner le SSD externe pour démarrer sur votre nouvelle installation de Linux Mint. 

> Cette vidéo explique comment démarrer votre ordinateur sur le disque dur externe :
[Video: Installer Linux sur disque dur externe](https://youtu.be/0KdD081LN0I?si=-ADQmgUtrhzw4kmY)

**Références ZE5 et Mint**   

Utiliser le fichier **ZE5: Procédure-installation-Linux-Mint.pdf** {{% button href="/docs/Guides/Procédure-installation-Linux-Mint.pdf" icon="download" %}}Télécharger le fichier pdf{{% /button %}}

[Mint: Guide d'installation](https://linuxmint-installation-guide.readthedocs.io/en/latest/install.html)

<!--
1. **VirtualBox** : Télécharger la version la plus récente et stable (en ce moment c'est la v. 7.2.4)
   * [**Lien vers la page de téléchargement**](https://www.virtualbox.org/wiki/Downloads)
   * Choisir la plateforme pour Windows.
   * Vous devriez obtenir le fichier `VirtualBox-7.2.4-170995-Win.exe`.
2. L'**Extension Pack**: Télécharger en cliquant sur *Accept and download*. vous devriez obtenir ce fichier `Oracle_VirtualBox_Extension_Pack-7.2.4.vbox-extpack`.
2. 
-->

### 💡 Astuces de la semaine

> **Le piège classique :**
> Si votre souris reste "capturée" à l'intérieur de la fenêtre virtuelle et que vous ne pouvez plus cliquer sur votre Windows, ne paniquez pas !
> Appuyez sur la touche **CTRL de DROITE** de votre clavier. C'est la touche de libération (Host Key).

---

# LABORATOIRE

### Objectif

* Installation de Linux sur votre SSD.
   * Assurez-vous d'avoir installer les "Guest Additions"
* Installation d'un hyperviseur (VirtualBox) sur votre SSD.
* Création d'une VM Linux (*Mint*).


Utiliser le fichier **labo1.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo1.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

### Étape 1 : Installer Linux Mint sur votre SSD (30 min)

1. **Installer VirtualBox :** Installation standard ("Suivant, Suivant, Terminer").
* **Problème fréquent :** Si VirtualBox refuse de se lancer ou affiche une erreur VTX/AMD-V, vous devez redémarrer et activer la **Virtualisation** dans son BIOS.


### Étape 2 : Construction de la machine (30 min)

Dans VirtualBox, cliquez sur **Nouvelle** :

* **Nom :** `NomPrenom_Linux`
* **Type :** Linux
* **Version :** Ubuntu (64-bit) *(Mint est basé sur Ubuntu)*.
* **RAM :** Selon le Micro-Exo (2048 Mo ou 4096 Mo).
* **Disque Dur :** Créer un disque virtuel maintenant -> VDI -> Dynamiquement alloué -> **30 Go** (Important : Ne mettez pas 10 Go, c'est trop peu pour la session).

### Étape 3 : Installation de l'OS (45 min)

1. **Montage de l'ISO :** Paramètres de la VM -> Stockage -> Vide (sous Contrôleur IDE) -> Petit disque bleu à droite -> Choisir le fichier de disque -> Sélectionner l'ISO de Mint.
2. **Démarrage :** Cliquez sur **Démarrer**.
3. **Lancer l'installateur :** Double-cliquez sur "Install Linux Mint" sur le bureau virtuel.
4. **La frayeur du débutant :**
* À l'étape "Type d'installation", choisir : **Effacer le disque et installer Linux Mint**.
* *Rassurer la classe :* "Ceci efface le disque VIRTUEL de 30 Go créé tantôt. Ça ne touche PAS à votre Windows."


5. **Création compte :**
* Nom : `etudiant` (pour faciliter la correction).
* Mot de passe : `420` ou `cmomo` (standardiser pour le labo).
* Nom de l'ordinateur : `station-linux`.


### Étape 4 : Post-installation & "Guest Additions" (15 min)

*Une fois l'installation finie et redémarrée :*

1. **Problème :** L'écran est tout petit (800x600).
2. **Solution :** Dans le menu de la fenêtre VirtualBox (en haut) -> **Périphériques** -> **Insérer l'image CD des additions invité**.
3. Linux va demander de lancer le logiciel -> "Lancer" -> Mot de passe -> Entrée.
4. Une fois fini, **Redémarrer la VM**.
5. Test final : Agrandissez la fenêtre, Linux doit s'adapter en plein écran.

---

### Checklist de fin de cours (Pour pouvoir partir)

> [!primary]
> Vous devez **montrer votre écran** pour savoir si vous pouvez partir :

1. VM démarrée.
2. Session ouverte (Login réussi).
3. Résolution d'écran dynamique (Plein écran fonctionnel).
4. Connexion Internet fonctionnelle (Ouvrir Firefox dans la VM et aller sur Google).

**Livrable (Preuve de travail à déposer sur Moodle) :**
- Une capture d'écran de votre Linux Mint en **plein écran**, 
- Avec le terminal ouvert montrant votre nom d'utilisateur, la date (commande `date`), et 
- Le gestionnaire de mises à jour vert (à jour).




<!--

## 1.2 Laboratoire / Pratique : "On touche au métal"


{{% notice style="red" title="Prérequis (à faire pendant la séance théorique)" groupid="notice-toggle" expanded="true" %}}
* **VirtualBox** doit être déjà téléchargé et installé. 
    * **NB** : La VM sera créée pendant la séance de pratique. 
* **L'ISO de Linux Mint** doit être déjà téléchargé (clé USB, OneDrive, etc.).
{{% /notice %}}

### Mission 1 : La Matrice (Configuration de la VM)
On utilise VirtualBox. C'est le moment de comprendre que l'on crée un "ordi dans l'ordi".
* **RAM :** Ne soyez pas radins. Mettez 4 Go si possible
* **Disque :** 20-30 Go.
* **L'erreur classique :** Oublier de "monter" l'ISO dans le lecteur CD virtuel.

### Mission 2 : L'Installation (Le moment critique)
Lancer la VM. Montrer que Linux peut tourner en "Live" (sans installer).
* Lancer l'installateur.
* **Le moment effrayant :** "Effacer le disque et installer Linux Mint".
    * *Rappel rassurant :* "C'est le disque *virtuel* qu'on efface, pas le disque dur du collège !"
* **Partitionnement :** Cocher **LVM** (Gestion par volumes logiques).
    * *L'explication simple :* "C'est une option magique qui permettra d'agrandir votre disque dur plus tard sans tout casser."


### Mission 3 : "Guest Additions" (Le test de QI)
Une fois installé, l'écran sera tout petit (800x600). C'est moche et inutilisable.
* **Le défi :** "Celui qui reste avec un petit écran a échoué son labo."
* **L'action :** Menu Périphériques -> Insérer l'image CD des Additions Invité.
* Lancer le script d'installation (nécessite le mot de passe root -> première utilisation de `sudo` sans le savoir !).
* Redémarrer.
* *Résultat :* Le plein écran fonctionne, le copier-coller entre Windows et Linux aussi.

### Mission 4 : Appropriation (Le "Hook" final)
1.  **Mise à jour :** Lancer le "bouclier" (Update Manager) en bas à droite.
2.  **Look :** Changer le fond d'écran (Clic droit -> Fond d'écran). Mettre une image de chien 🐶 ou de chat 🐱, peu importe.
3.  **Exploration :** Ouvrir le terminal (le carré noir). Taper `matrix` (si installé) ou juste `echo "Je suis un chat"`.


-->

