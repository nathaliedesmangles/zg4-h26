+++
pre = 'Semaine 2 (cours 1) : '
title = "Labo #1 - Installation d'AlmaLinux"
weight = 21
draft = true
+++



## Prérequis

Avant de commencer, assurez-vous d'avoir :

1. **VirtualBox** installé sur votre SSD (voir cours "ZE5-Environnements virtuels et réseau")
2. **L'image ISO d'AlmaLinux**.
   * [*Lien de téléchargement :*](https://almalinux.org/get-almalinux/)
      -> Télécharger -> x86_64 -> Choisissez le miroir le plus proche -> Prenez le fichier qui se termine par **`dvd.iso`** (environ 9-10 Go).

      ![DVD ISO](./almalinux-dvd-iso.jpg?width=35vw)


## Remise

✔ Preuve que la VM AlmaLinux fonctionne
✔ Preuve que vous savez utiliser le terminal
✔ Preuve que l'exercice est fait

<!--
---

### Partie 1 : Création de la machine virtuelle

Nous allons d'abord construire l'ordinateur virtuel avant d'y installer le système.

1. Ouvrez **VirtualBox**.
2. Cliquez sur l'icône bleue **"Nouvelle"** (ou "New").
3. **Nom et OS :**
   * **Nom :** `AlmaLinux-ZG4`
   * **Dossier :** Laissez par défaut.
   * **Type :** Linux
   * **Version :** Red Hat (64-bit) *(AlmaLinux est un clone de Red Hat)*.


4. **Mémoire (RAM) :**
   * Mettez au moins **2048 Mo (2 Go)**. Si vous avez 16 Go de RAM ou plus sur votre SSD, mettez 4096 Mo (4 Go) pour que ce soit fluide.


5. **Disque dur :**
   * Sélectionnez "Créer un disque dur virtuel maintenant".
   * Type de fichier : **VDI** (VirtualBox Disk Image).
   * Stockage : **Dynamiquement alloué** (prendra peu de place au début et grossira selon les besoins).
   * Taille : Mettez **20 Go** minimum (le système en prendra déjà 5 ou 6).


### Partie 2 : Insertion du CD d'installation

Votre machine est créée, mais elle est vide. Il faut mettre le CD (l'ISO) dans le lecteur.

1. Sélectionnez votre VM `AlmaLinux-ZG4` dans la liste à gauche.
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
   * Nom d'utilisateur : `prenom-nom` (tout en minuscule, c'est mieux). (**OBLIGATOIRE**)
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