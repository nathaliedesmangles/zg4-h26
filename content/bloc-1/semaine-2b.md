+++
pre = 'Semaine 2.2 : '
title = 'Labo #2 - Les commandes de base'
weight = 14
draft = false
+++


**Objectifs** : Comprendre la ligne de commande et développer la "mémoire musculaire" des commandes de base.

 
> [!primary]
> **Pour les exercices #3 et #4**, n'oubliez pas de prendre une **capture d'écran** pour chacune des **étapes et** que dans les captures on doit voir votre ligne de commande, la (les) commande(s) effectuées et leurs résultats.


### Exercice #1 : Comprendre la ligne de commande

Répondre aux questions suivantes dans un fichier texte.

1. Dans l'invite de commande `[utilisateur@localhost ~]$`, que signifie le symbole $ à la fin ?
2. Dans l'invite `[root@serveur01 ~]#`, que représente le tilde `~` ?
3. Quelle partie de l'invite `[marie@bureau-dev documents]$` correspond au nom d'hôte de la machine ?
4. Si vous passez en mode superutilisateur avec `su -`, quel changement visuel attendez-vous principalement sur l'invite ?
5. Dans l'invite `[admin@almalinux /var/log]$`, que représente `/var/log` ?

### Exercice #2 : Trouver la bonne commande

Répondre aux questions suivantes dans le fichier texte.

À l’aide des commandes **`apropos`** et/ou **`man`**, trouver la commande (**incluant les options et la cible s'il y a lieu**) qui permet :

1. D’afficher la date au format **MM/JJ/AA**.
2. De changer les permissions sur un fichier ou sur un dossier (indiquez seulement le nom de la commande).
3. D’arrêter le système.
4. De rechercher du texte dans un fichier nommé **recherche.txt**.
5. De compter les lignes dans un fichier nommé **lignes.txt**.


### Exercice #3 : Création & navigation

##### **Étape 0** : Mise en place de la structure de dossiers

> [!primary]
> Pas de capture d'écran pour **cette étape** (0).

Copiez et collez ce bloc de commandes dans votre terminal pour créer l'environnement de l'exercice. Cela va créer la structure de dossiers et fichiers suivante :

```bash
cd ~
mkdir -p Exercice_Chemins/{Projet_Alpha/logs,Projet_Beta/src/images,Archives}
touch Exercice_Chemins/Projet_Alpha/logs/erreur.log
touch Exercice_Chemins/Projet_Beta/src/images/logo.png
touch Exercice_Chemins/readme.txt
echo "Setup terminé !"
```

**Votre structure ressemble maintenant à ceci :**

* Sur **AlmaLinux**: La commande `tree` vous afficher la structure schématique.  
* Sur **Mint** : Utilisez la commande `ls -R` pour voir le découpage de la structure.  

```text
(Votre Dossier Personnel ~)
└── Exercice_Chemins/
    ├── Archives/
    ├── Projet_Alpha/
    │   └── logs/
    │       └── erreur.log  <-- CIBLE 1
    ├── Projet_Beta/
    │   └── src/
    │       └── images/
    │           └── logo.png  <-- CIBLE 2
    └── readme.txt
```

Vous êtes connecté sur un serveur et vous devez réorganiser ces fichiers. **Suivez les instructions à la lettre**.

##### **Étape 1** : Navigation absolue

**Objectif :** Vous êtes actuellement dans votre dossier personnel (`~`).  
**Action :** Rendez-vous directement dans le dossier `images` du `Projet_Beta` en une seule commande.  
**Contrainte :** Vous devez utiliser le **chemin absolu**.

> ***Indice** : Utilisez `pwd` une fois arrivé pour vérifier.*

> [!primary]
> Prenez une capture d'écran pour **cette étape**.

##### **Étape 2** :  Chemin relatif complexe

**Objectif :** Vous êtes maintenant assis dans le dossier `.../src/images/`.  
**Action :** Vous réalisez que le fichier `logo.png` n'est pas à sa place. Déplacez-le vers le dossier `Archives` (qui se trouve dans `Exercice_Chemins`).  
**Contrainte MAJEURE :** **Interdiction** d'utiliser le caractère `/` au début du chemin. Vous devez utiliser des chemins relatifs (`..`).

> ***Réflexion** : Combien de fois devez-vous remonter ("..") pour sortir de "images", puis "src", puis "Projet_Beta" pour enfin voir "Archives" ?*

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 3** : Le retour à la maison

**Objectif :** Vous êtes toujours dans `.../src/images/` (même si vous avez déplacé le fichier).   
**Action :** Copiez le fichier `readme.txt` (qui est à la racine de `Exercice_Chemins`) vers votre emplacement actuel (`.`).    
**Contrainte :** Utilisez le raccourci tilde (`~`) pour désigner le fichier source.

> [!primary]
> Prenez une capture d'écran pour **cette étape**.

##### **Étape 4** : Chemin absolu vers relatif

**Objectif :** Revenez à la racine de l'exercice (`Exercice_Chemins`).  
**Action :** Supprimez le fichier `erreur.log` qui se trouve tout au fond de `Projet_Alpha/logs/`.  
**Contrainte :** Depuis l'endroit où vous êtes, visez le fichier en utilisant un **chemin relatif**.

> [!primary]
> Prenez une capture d'écran pour **cette étape**.

### Exercice #4 : Création et manipulation

*Vous êtes administrateur système. On vous demande de préparer l'arborescence pour un nouveau site web, de faire une sauvegarde, puis de nettoyer les fichiers temporaires.*


##### **Étape 1** : Préparation du terrain

1. Assurez-vous d'être dans votre dossier personnel.
2. Créez un dossier principal pour cet exercice nommé `Labo_Linux`.
3. Entrez dans ce dossier.

> *À partir de maintenant, toutes les commandes se feront dans ce dossier.*

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 2** : Création de la structure 

1. Dans le dossier actuel, créez un répertoire nommé `MonSiteWeb`.
2. À l'intérieur de `MonSiteWeb`, créez deux sous-dossiers : `images` et `config`.  
**Contrainte :** Sans entrer dans le dossier (chemin relatif).  
3. Créez trois fichiers vides directement dans `MonSiteWeb` : `index.html`, `style.css` et `brouillon.txt`.

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 3** : Organisation et correction

*Oups, vous avez fait quelques erreurs de rangement. Corrigeons cela.*

1. Le fichier `style.css` ne doit pas être à la racine du site. Déplacez-le dans le dossier `config` (même si ce n'est pas logique pour un vrai site, c'est pour l'exercice !).
2. Le fichier `brouillon.txt` a un mauvais nom. Renommez-le en `readme.txt`.
3. Finalement, déplacez ce `readme.txt` pour qu'il remonte d'un niveau (il doit se retrouver dans votre dossier `Labo_Linux`, à côté de `MonSiteWeb`, et non dedans).

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 4** : Sauvegarde

*Avant de faire des bêtises, faisons une copie de sécurité.*

1. Copiez le fichier `index.html` (qui est dans `MonSiteWeb`) et nommez la copie `index.html.bak` (dans le même dossier).
2. Copiez **tout** le répertoire `MonSiteWeb` (et son contenu) vers un nouveau répertoire nommé `MonSiteWeb_Backup`.

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 5** : Nettoyage

*Le projet est fini, il faut nettoyer les fichiers inutiles.*

1. Entrez dans `MonSiteWeb`. Supprimez le fichier de sauvegarde `index.html.bak`.
2. Supprimez le dossier `images` (il est vide, utilisez la commande la plus sûre pour les dossiers vides).
3. Remontez dans `Labo_Linux`.
4. Tentez de supprimer le dossier `MonSiteWeb_Backup`.
> ***Attention** : Il contient des fichiers. Vous devrez utiliser une option spécifique pour forcer la suppression récursive.*

> [!primary]
> Prenez une capture d'écran pour **cette étape**.


##### **Étape 6** : Vérification

Tapez la commande `tree` (ou `ls -R`) dans votre dossier `Labo_Linux`, vous ne devriez voir que :

* Le dossier `MonSiteWeb` (contenant `index.html` et le dossier `config` avec `style.css`).
* Le fichier `readme.txt`.
* Le dossier `MonSiteWeb_Backup` a dû disparaître.


> [!primary]
> Prenez une capture d'écran pour **cette étape**.
---


## Corrigé du laboratoire

> À venir (samedi ou dimanche)

<!--

## Solutions (Commandes à taper)

### Exercice #3


#### Solution 1 

Si votre nom d'utilisateur est "etudiant", le chemin absolu ressemble à ça.

```bash
cd /home/etudiant/Exercice_Chemins/Projet_Beta/src/images
# OU
cd ~/Exercice_Chemins/Projet_Beta/src/images
```

*Pourquoi ? Le chemin absolu est l'adresse GPS exacte. Il fonctionne peu importe où vous vous trouvez.*

#### Solution 2 : 

Vous êtes dans `images`.

```bash
mv logo.png ../../../Archives/
```

*Explication :*

* `../` : Je remonte dans `src`
* `../../` : Je remonte dans `Projet_Beta`
* `../../../` : Je remonte dans `Exercice_Chemins` (là où je vois le dossier `Archives`).
* Ensuite, je redescends dans `Archives/`.

#### Solution 3 :

```bash
cp ~/Exercice_Chemins/readme.txt .
```

*Explication :* Le tilde `~` remplace `/home/votre_user`. C'est un chemin absolu déguisé. Le point `.` à la fin signifie "ici même".

#### Solution 4 : 

Vous êtes dans `Exercice_Chemins`.

```bash
rm Projet_Alpha/logs/erreur.log
```

*Explication :* Pas besoin de `cd` pour entrer dans le dossier. On donne juste le chemin relatif "vers le bas" à la commande `rm`.


### Exercice #4

**Étape 1 :**

```bash
cd ~
mkdir Labo_Linux
cd Labo_Linux
```

**Étape 2 :**

```bash
mkdir MonSiteWeb
mkdir MonSiteWeb/images MonSiteWeb/config
touch MonSiteWeb/index.html MonSiteWeb/style.css MonSiteWeb/brouillon.txt
```

**Étape 3 :**

```bash
mv MonSiteWeb/style.css MonSiteWeb/config/
mv MonSiteWeb/brouillon.txt MonSiteWeb/readme.txt
mv MonSiteWeb/readme.txt .   # Le point signifie "ici" (Labo_Linux)
```

**Étape 4 :**

```bash
cp MonSiteWeb/index.html MonSiteWeb/index.html.bak
cp -r MonSiteWeb MonSiteWeb_Backup
```

**Étape 5 :**

```bash
cd MonSiteWeb
rm index.html.bak
rmdir images
cd ..
rm -r MonSiteWeb_Backup
```

-->

<!--

## ANCIENS

### Exercice #2 : Lire un manuel

Avec **`man`**, trouve dans la commande :

1. Dans `man ls` :

   * Quelle option permet **d’afficher les fichiers cachés** ?
   * Quelle option donne un **listing détaillé** ?
2. Dans `man cp` :

   * Quelle est la syntaxe (ligne complète du SYNOPSIS) ?
   * Quelle option permet de demander une **confirmation avant d’écraser** un fichier ?
3. Dans `man rm` :

   * Quelle option permet une suppression **interactive** ?
   * Quelle option force la suppression sans confirmation ?

4. Dans `man mkdir` :

   * Le SYNOPSIS indique `mkdir [OPTION]... DIRECTORY...`.
   	* Que signifie `...` ?
   	* Peut-on créer plusieurs répertoires à la fois ?
   * Trouve comment créer un répertoire **avec ses sous-répertoires automatiquement** (option recursive).

Avec **uniquement** **`apropos`**, trouve dans la commande qui permet de trouver :

   * L’espace disque libre.
   * L’utilisation du CPU.
   * L'informations sur les partitions.

> Puis utilise `man` pour trouver **une option utile** pour chacune des commandes trouvées.



### Exercice #3 : Exploration et manipulation

#### Étape 0 : Ouvrir le Terminal (10 min)

* Raccourci clavier (souvent `Ctrl+Alt+T`) ou via le menu.
* Analyser le prompt : `etudiant@station-linux:~$`
* Qui ? `etudiant`
* Où ? `~` (Dans mon home).


#### Étape 1 : Exploration (30 min)

**Commandes** : `pwd`, `ls`, `cd`

1. **Où suis-je ?** Tapez `pwd` (Print Working Directory). Confirmez que vous êtes dans `/home/etudiant`.
2. **Qu'est-ce qu'il y a ici ?** Tapez `ls`.
3. **Voir l'invisible :** Tapez `ls -a`. Remarquez les fichiers commençant par un point (ex: `.bashrc`). Ce sont les fichiers cachés.
4. **Aller ailleurs :**
	* Allez à la racine : `cd /`
	* Vérifiez avec `pwd`.
	* Listez le contenu : `ls`. Voyez-vous les dossiers `bin`, `etc`, `home` ?
5. **Le ping-pong :**
	* Revenez chez vous avec le raccourci rapide : `cd` (sans argument) ou `cd ~`.
	* Remontez d'un niveau : `cd ..` (Vous êtes maintenant dans `/home`).


#### Étape 2 : Manipulation (45 min)

**Commandes** : `mkdir`, `touch`, `cp`, `mv`, `rm*`

1. **Créer une structure :**
	* Créez un dossier `Labo2` : `mkdir Labo2`
	* Entrez dedans : `cd Labo2`
	* Créez une structure complexe en une ligne : `mkdir -p Projet/Images/Icones`


2. **Créer des fichiers :**
	* Créez un fichier vide : `touch mon_fichier.txt`


3. **Copier (`cp`) :**
	* Copiez le fichier dans le dossier Projet : `cp mon_fichier.txt Projet/`
	* Vérifiez : `ls Projet/`


4. **Déplacer et Renommer (`mv`) :**
	* *Note : Linux n'a pas de commande "renommer". On "déplace" un fichier vers un nouveau nom.*
	* Renommez le fichier original : `mv mon_fichier.txt fichier_final.txt`
	* Déplacez-le dans Images : `mv fichier_final.txt Projet/Images/`


5. **Supprimer (`rm`) - ATTENTION :**
	* Essayez de supprimer le dossier Projet : `rm Projet` -> *Erreur ! C'est un dossier.*
	* Supprimez le dossier et tout son contenu : `rm -r Projet` (Recursive).
	* *Rappel : Il n'y a pas de corbeille en ligne de commande. C'est définitif.*


#### Exo #3

Répondre aux questions suivantes dans le fichier texte.

1. Quelle commande permet d'afficher le chemin absolu du répertoire dans lequel vous vous trouvez actuellement ?
2. Quel symbole représente le répertoire racine (root) dans l'arborescence Linux ?
3. Si vous êtes dans `/home/user/Documents`, quelle commande vous permet de remonter directement dans `/home/user` ?
4. Quelle commande utiliseriez vous pour lister le contenu d'un répertoire, y compris les fichiers cachés (commençant par un point) ?
   * ***Hint*** : Utilisez la commande `man` ou `apropos` pour trouver la réponse.
5. Dans quel répertoire se trouvent généralement les fichiers de configuration du système ?
6. Que fait la commande `cd` si elle est exécutée sans aucun argument (option, dossier cible) ?
7. Laquelle des propositions suivantes est un exemple de chemin relatif ?
   a. `/var/log`
   b. `/etc/sysconfig`
   c. `/home/etudiant/test`
   d. `/Documents/projets`
8. Quelle commande permet de revenir au répertoire dans lequel vous étiez juste avant votre dernier déplacement ?
9. Si vous souhaitez accéder aux fichiers journaux (logs) du système, vers quel répertoire devriez-vous naviguer ?


-->
