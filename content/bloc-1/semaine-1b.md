+++
pre = 'Semaine 1.2 : '
title = 'Le système de fichiers de Linux'
weight = 12
draft = false
+++



## Objectif de la séance

* Connaitre et comprendre la structure des fichiers sous Linux
* Se déplacer dans le système et manipuler des fichiers à l'aide de la ligne de commande. 

> [!warning]

> C'est le cours le plus important pour arriver à s'orienter dans l'architecture de Linux.


---


# Théorie

## Windows vs Linux

Sous Windows, vous avez des lecteurs physiques : `C:\` (Disque système), `D:\` (USB), `Z:\` (Réseau).
Sous Linux, **tout est un fichier** et tout commence au même endroit .

![Arborescence](../arborescence.png?width=50vw)

* **La Racine (`/`) :** C'est le point de départ unique. Il n'y a pas de "C:". Tout ce qui est branché à l'ordinateur (disque dur, clé USB, DVD) apparaît comme un dossier quelque part sous la racine.
* **La distinction majuscule/minuscule (Case Sensitivity) :**
    * **Windows** : `Dossier` = `dossier`.
    * **Linux** : `Dossier`, `dossier` et `DOSSIER` sont trois dossiers différents.


## Les incontournables du système de fichiers

Pas besoin de tout connaître, mais vous devez reconnaître ceux-ci :

* `/` (Slash) : La racine (The Root). Le début de tout.
* `/home` : Vos documents. C'est l'équivalent de `C:\Users`. C'est le *seul* endroit où vous avez le droit d'écrire par défaut.
* `/root` : Le dossier personnel de l'administrateur suprême. (Ne pas confondre avec `/`).
* `/etc` : **Etc**etera. Contient les fichiers de configuration système.
* `/bin` & `/usr/bin` : **Bin**aries. Contient les programmes (les commandes comme `ls`, `cp`).
* `/var` : **Var**iable. Ce qui change souvent (Logs, site web, bases de données).


## Chemins absolus vs relatifs

La différence entre chemin absolu et relatif est souvent le point de blocage numéro 1 des débutants, alors qu'elle est très simple une fois visualisée.

> [!warning]

> C'est la **cause #1 des erreurs** au début **ET AUSSI** aussi dans les cours de programmation.

Commençons par une analogie avec Windows, que vous connaissez probablement déjà.


### L'analogie Windows

Imaginez que vous êtes sur votre ordinateur Windows.

1. **Le chemin absolu (C'est le GPS) :**
C'est l'adresse complète et incontestable.
* ***Windows :*** `C:\Users\Jean\Documents\Vacances`
* ***Linux :*** `/home/jean/documents/vacances`
* ***Pourquoi l'utiliser ?*** Peu importe où vous êtes dans l'ordinateur, si vous tapez cette adresse, vous arriverez **toujours** au même endroit. C'est comme donner une coordonnée GPS exacte.


2. **Le chemin relatif :**
C'est indiquer une direction **par rapport à là où vous êtes *maintenant***.
* *Windows :* Vous êtes déjà dans le dossier `Documents`. Vous voyez le dossier `Vacances` devant vous. Vous double-cliquez dessus.
* *Linux :* Vous tapez `cd vacances`.
* *Pourquoi l'utiliser ?* C'est plus court ! Pourquoi taper toute l'adresse (C:\Users...) si le dossier est juste devant vous ?



### Résumé : Quand utiliser quoi ?

| Situation | Utilisez... | Pourquoi ? |
| --- | --- | --- |
| Le dossier ciblé est **dans** le dossier actuel | **Relatif** | Rapide (`cd photos`) |
| Le dossier ciblé est **très loin** ou dans une autre branche | **Absolu** | Pas besoin de calculer le trajet (`cd /var/log`) |
| Je suis perdu dans l'arborescence | **Absolu** | Pour être sûr d'atterrir au bon endroit |
| Je veux remonter d'un niveau | **Relatif** | Très rapide (`cd ..`) |

* **Les raccourcis magiques :**
   * `.` (Un point) = Ici (Dossier courant).
   * `..` (Deux points) = Le dossier parent (Remonter d'un cran).
   * `~` (Tilde) = Ma maison (`/home/etudiant`).


## L'anatomie d'un prompt (AlmaLinux)

`etudiant@almalinux:~/Documents$`

1.  `etudiant` : **QUI** je suis ? (Mon identité).
2.  `@almalinux` : **OÙ** je suis ? Sur quelle machine ?
3.  `~/Documents` : **DANS QUEL DOSSIER** je suis ? (Mon emplacement).
4.  `$` : **QUEL POUVOIR** j'ai ?
    * `$` = Utilisateur normal.
    * `#` = Root / Superutilisateur. <span style="color:red;"><b>Si vous voyez ça, faites gaffe</b></span>.



## La syntaxe d'une commande

Une commande suit presque toujours cette logique :  
   ```
   COMMANDE + OPTIONS + CIBLE
   ```

Exemple : `ls -l /etc`
* **Quoi faire ?** `ls` (Lister).
* **Comment ?** `-l` (Format long/détails).
* **Où ?** `/etc` (Dans le dossier de config).


## Les commandes de base

Voici les commandes essentielles pour naviguer, créer et manipuler une structure de dossiers dans le système.

### 1. Navigation et exploration

Ces commandes vous permettent de savoir où vous êtes et de vous déplacer dans l'arborescence.

| Commande | Description | Exemple |
| --- | --- | --- |
| **`pwd`** | **P**rint **W**orking **D**irectory. Affiche le chemin absolu du dossier actuel. | `pwd` <br> |
| **`ls`** | **L**i**s**t. Liste les fichiers et dossiers du répertoire actuel. | `ls` |
| **`ls -l`** | Liste détaillée (permissions, propriétaire, taille, date). | `ls -l` |
| **`ls -a`** | Affiche **tous** les fichiers, y compris les cachés (commençant par `.`). | `ls -a` |
| **`cd [dossier]`** | **C**hange **D**irectory. Entre dans un dossier spécifique. | `cd Documents` |
| **`cd ..`** | Remonte d'un niveau (vers le dossier parent). | `cd ..` |
| **`cd ~`** | Retourne directement à votre dossier personnel (home). | `cd ~` |
| **`cd -`** | Retourne au dossier précédent (celui où vous étiez juste avant). | `cd -` |



### 2. Création et manipulation

Commandes pour créer, copier, déplacer ou supprimer des éléments.

| Commande | Description | Exemple |
| --- | --- | --- |
| **`mkdir [nom]`** | **M**a**k**e **Dir**ectory. Crée un nouveau dossier. | `mkdir Projets` |
| **`touch [nom]`** | Crée un fichier vide (ou met à jour la date s'il existe déjà). | `touch notes.txt` |
| **`cp [src] [dest]`** | **C**o**p**y. Copie un fichier d'une source vers une destination. | `cp notes.txt backup_notes.txt` |
| **`cp -r [src] [dest]`** | Copie récursive (pour copier tout un dossier et son contenu). | `cp -r Dossier1 Dossier2` |
| **`mv [src] [dest]`** | **M**o**v**e. Déplace un fichier/dossier OU le renomme. | `mv notes.txt archives/` (déplace)<br>`mv notes.txt ancien.txt` (renomme) |
| **`rm [fichier]`** | **R**e**m**ove. Supprime un fichier. | `rm ancien.txt` |
| **`rm -r [dossier]`** | Supprime un dossier et tout son contenu (Récursif). | `rm -r DossierInutile` |
| **`rm -rf [dossier]`** | <span style="color:red;"><b>Attention</b>  </span>: Force la suppression sans demander confirmation. | `rm -rf DossierRebelle` |



### 3. Lecture de contenu

Pour voir ce qu'il y a à l'intérieur des fichiers sans les ouvrir dans un éditeur.

| Commande | Description | Exemple |
| --- | --- | --- |
| **`cat [fichier]`** | Affiche tout le contenu du fichier d'un coup dans le terminal. | `cat config.txt` |
| **`less [fichier]`** | Affiche le contenu page par page (touche `q` pour quitter). Idéal pour les longs fichiers. | `less gros_fichier.log` |
| **`head [fichier]`** | Affiche les 10 premières lignes d'un fichier. | `head -n 5 liste.txt` (affiche 5 lignes) |
| **`tail [fichier]`** | Affiche les 10 dernières lignes d'un fichier. | `tail error.log` |
| **`tail -f [fichier]`** | Suit la fin d'un fichier en temps réel (utile pour surveiller des logs). | `tail -f /var/log/messages` |
 

## Astuces 

> **L'autocomplétion (Tab)**
> Ne tapez jamais les noms de fichiers en entier !
> Tapez les 3 premières lettres (ex: `cd Doc`) et appuyez sur la touche **TAB**.
> Linux finira le mot pour vous (`cd Documents/`).
> *Si ça ne marche pas, appuyez **2 fois sur TAB** : Linux vous montrera les choix possibles.*

> **Historique (Flèches)**
> Vous avez fait une faute de frappe dans une longue commande ?
> Appuyez sur la **Flèche du Haut** pour rappeler la dernière commande et corrigez-la.

> **Effacer l'affichage du Terminal**  
> La commande `clear` efface **de l'écran du Terminal** (pas de la mémoire de la session), toutes les commandes effectuées et leurs résultat

---

# Exercices (en classe)

## 🟢 Exercice #1 : Création d'un compte utilisateur

En attendant que votre machine virtuelle **AlmaLinux** soit fonctionnelle sur VirtualBox, nous utiliserons un environnement temporaire en ligne.

Nous utiliserons **JSLinux (version Fedora)**. Fedora étant la distribution "source" de Red Hat (et donc d'AlmaLinux), les commandes sont identiques.


> [!warning]

> Cet outil fonctionne dans la mémoire vive (RAM) de votre navigateur.
> 1. **Ne fermez pas l'onglet** et ne rafraîchissez pas la page (F5) tant que vous n'avez pas fini.
> 2. Si vous quittez la page, **tout votre travail est perdu**.
> 3. Prenez des captures d'écran de vos réussites au fur et à mesure.

> [!primary]
> Certaines des commandes utilisées dans **cet exercice** seront approfondies plus tard dans le cours.

### Étape 1 : Accéder au terminal

1. Ouvrez votre navigateur.
2. Allez sur : **[https://bellard.org/jslinux/](https://bellard.org/jslinux/)**
3. Cherchez **Fedora 33**.
4. Cliquez sur le lien **Console**.

> *Le système démarre. Attendez l'apparition de l'invite de commande :*
   
```bash
[root@localhost ~]#
```



### Étape 2 : Préparer son environnement

Par défaut, vous êtes connecté en **root** (Super-administrateur). Sur un vrai système Linux, on ne travaille jamais en **root** pour des tâches courantes. Nous allons donc créer un compte étudiant.

Tapez les commandes suivantes une par une :

**1. Créer l'utilisateur "etudiant" avec son dossier personnel :**

```bash
useradd -m etudiant
```

**2. Définir un mot de passe :**

```bash
passwd etudiant
```

> [!warning]

> *Le système vous demandera de taper le mot de passe **deux fois**.  
> <span style="color:red;"><b>Attention</b>  </span>:  rien ne s'affiche quand vous tapez (ni étoiles, ni points), c'est une sécurité normale sous Linux. Tapez simplement `linux` (ou un autre choix) et faites Entrée.*

**3. Donner les droits d'administration :**

```bash
usermod -aG wheel etudiant
```

**4. Se connecter en tant qu'étudiant :**

```bash
su - etudiant
```

> [!primary]
> **Observez le changement :**
> Votre invite de commande a changé de `root@... #` à `etudiant@... $`.

```bash
[etudiant@localhost ~]$
```

* **`#`** = Vous êtes super-utilisateur (***root***). <span style="color:red;"><b>Danger</b></span>.
* **`$`** = Vous êtes un utilisateur standard.  <span style="color:green;"><b>Sécuritaire</b></span>.



### Étape 3 : Vérification finale

Assurez-vous que tout est prêt en tapant ces commandes :

```bash
whoami
```

> *Doit afficher : `etudiant`*

```bash
pwd
```

> *Doit afficher : `/home/etudiant` (C'est votre dossier personnel)*

```bash
cat /etc/redhat-release
```

> *Doit afficher : `Fedora release...` (Confirme la compatibilité avec le cours)*


## 🟢 Exercice #2 : Absolu vs relatif

### Étape 0 : Préparation de la structure

Pour que cet exercice fonctionne, nous allons créer une petite structure de dossiers (une arborescence).

Copiez et collez ces commandes dans votre terminal pour créer toute la structure d'un coup :

```bash
cd ~
mkdir -p Exercice/Continent/Amerique/Canada
mkdir -p Exercice/Continent/Europe/France
mkdir -p Exercice/Planetes/Mars
```

> [!primary]
> *L'option `-p` permet de créer les dossiers **parents** et **enfants** d'un seul coup.*

Maintenant, nous avons cette structure :

```text
Exercice/              <-- Racine de votre projet
├── Continent/
│   ├── Amerique/
│   │   └── Canada/    <-- Bout de la branche
│   └── Europe/
│       └── France/    <-- Bout de la branche
└── Planetes/
    └── Mars/          <-- Une autre branche distincte
```


### Étape 1 : La vérification

On commence à la racine de notre exercice.

* **Commande :** 
   ```bash
   cd ~/Exercice`
   ```
* **Vérification :** Tapez `ls -R` (le -R liste tout le contenu récursif). Vous devriez voir vos dossiers Continent et Planetes.

> [!primary]
> * Dans **AlmaLinux** on pourra taper la commande `tree` qui affiche l'arborescence de la structure (visuellement plus clair)
> * Il y a aussi la commande `find` (qu'on étudiera plus tard) qui permet d'afficher l'arborescence. (ex: `find Exercice`)


### Étape 2 : Le chemin relatif

Vous êtes dans `Exercice`. Vous voulez aller dans `Continent`. C'est juste à côté.

* **Action :** On utilise le chemin relatif. On ne met **pas** de `/` au début.
* **Commande :**
   ```bash
   cd Continent
   ```

* *Pourquoi ?* C'est rapide. Pas besoin de dire `/home/user/Exercice/Continent`. Juste "Continent".


### Étape 3 : Le chemin relatif "Profond"

Vous êtes dans `Continent`. Vous voulez aller directement voir `Canada`, qui est dans `Amerique`.

* **Action :** On descend de deux niveaux d'un coup.
* **Commande :**
```bash
cd Amerique/Canada
```

* **Vérification :** Tapez `pwd`. Vous devriez voir finir par `.../Continent/Amerique/Canada`.


### Étape 4 : Pourquoi l'absolu est utile

> [!primary]
> **Lisez attentivement.**

* Vous êtes actuellement dans le dossier `Canada`.
* Vous voulez aller dans le dossier `Mars` (qui est dans `Exercice/Planetes/Mars`).

> Si vous tapez `cd Planetes/Mars`... **ça ne marchera pas !**  
> *Pourquoi ?* Parce que Linux cherche le dossier `Planetes` **à l'intérieur** du dossier `Canada` (là où vous êtes). Et il n'existe pas à cet endroit.

Ici, le chemin **Absolu** est plus adéquat. On repart de la racine (le `/`).

* **Action :** Utiliser le chemin absolu (le GPS).
* **Commande :**
   ```bash
   cd ~/Exercice/Planetes/Mars
   ```

**Note** : *(Le `~` remplace `/home/votre_user`, c'est donc un chemin absolu qui part du début).*


### Étape 5 : Le retour en arrière (Relatif avec `..`)

Vous êtes sur `Mars`. Vous voulez remonter juste d'un cran pour revenir dans `Planetes`.

* **Action :** Utiliser le raccourci relatif `..` (le parent).
* **Commande :**
   ```bash
   cd ..
   ```

### Étape 6 : Absolu vs relatif

Vous êtes dans `Planetes`. Vous voulez retourner en `France`.

Essayons les deux méthodes pour comparer (ne faites que la **B** si vous voulez gagner du temps, ou faites **A** puis revenez).

* **Méthode A (Relatif - compliquée ici) :**
Il faut remonter dans `Exercice` (`..`), puis aller dans `Continent`, puis `Europe`, puis `France`.
`cd ../Continent/Europe/France`
> *C'est pénible à calculer mentalement, non ?*

* **Méthode B (Absolu - facile) :**
On donne l'adresse complète. On ne réfléchit pas au trajet, juste à la destination.
   ```bash
   cd ~/Exercice/Continent/Europe/France
   ```


### Étape 7 : Nettoyage de l'exercice

* **Action :** Une fois que vous avez compris, vous pouvez supprimer toute la structure.
* **Commande :**
   ```bash
   rm -rf ~/Exercice`
   ```


## 🟢 Exercice #3 : Création de fichiers et dossiers

**Objectif :** Créer une structure de dossiers, y ajouter des fichiers, les manipuler, vérifier le contenu, et enfin nettoyer les traces.

Suivez ces étapes une par une. Essayez de deviner la commande avant de regarder la solution.



### Étape 1 : Lieu de départ

Assurez-vous d'être dans votre répertoire personnel (Home), pour ne pas mettre le désordre ailleurs.

* **Action :** Aller dans le dossier personnel et vérifier le chemin.
* **Commande :**
   ```bash
   cd ~
   pwd
   ```

### Étape 2 : Dossier de travail

Nous avons besoin d'un dossier principal pour travailler.

* **Action :** Créer un dossier nommé `MissionMars`.
* **Commande :**
   ```bash
   mkdir MissionMars
   ```

### Étape 3 : On se déplace

* **Action :** Déplacez-vous à l'intérieur du dossier que vous venez de créer.
* **Commande :**
   ```bash
   cd MissionMars
   ```

### Étape 4 : Archive

À l'intérieur de `MissionMars`, nous avons besoin d'un sous-dossier pour les archives.

* **Action :** Créer un dossier nommé `Archives`.
* **Commande :**
   ```bash
   mkdir Archives
   ```


### Étape 5 : Un premier fichier texte

Créons un fichier vide pour commencer.

* **Action :** Créer un fichier vide nommé `rapport.txt`.
* **Commande :**
   ```bash
   touch rapport.txt
   ```


### Étape 6 : Ajouter du contenu (Vu plus tard)

Comme `touch` crée un fichier vide, `cat` n'affichera rien. Ajoutons une ligne de texte

* **Action :** Écrire "Ceci est confidentiel" dans le fichier.
* **Commande :**
   ```bash
   echo "Ceci est confidentiel" > rapport.txt
   ```


### Étape 7 : Vérification

* **Action :** Lire le contenu du fichier `rapport.txt` dans le terminal.
* **Commande :**
   ```bash
   cat rapport.txt
   ```


### Étape 8 : Une copie du rapport

Nous voulons garder une copie du rapport dans le dossier `Archives`.

* **Action :** Copier `rapport.txt` vers le dossier `Archives`.
* **Commande :**
   ```bash
   cp rapport.txt Archives/
   ```


### Étape 9 : Renommer

Le fichier original doit changer de nom pour indiquer qu'il est en cours de traitement.

* **Action :** Renommer `rapport.txt` en `rapport_final.txt`.
* **Commande :**
   ```bash
   mv rapport.txt rapport_final.txt
   ```


### Étape 10 : Vérification

* **Action :** Affichez la liste détaillée des fichiers pour voir le fichier renommé et le dossier Archives.
* **Commande :**
   ```bash
   ls -l
   ```

   > *Vous devriez voir `rapport_final.txt` et le dossier `Archives`*.


### Étape 11 : Faison le ménage

Nous avons fini l'exercice, supprimons tout pour laisser le système propre.

* **Action :** Remontez d'un niveau (vers le dossier parent), puis supprimez le dossier `MissionMars` et tout son contenu.
* **Commande :**
   ```bash
   cd ..
   rm -rf MissionMars
   ```

### Étape 12 : L'historique des commandes

Nous souhaitons quand même avoir un historique des commandes effectuées avant de fermer ou rafraichir le terminal.

* **Action :** Afficher la liste des commandes tapées depuis l'ouverture du terminal.
* **Commande :**
   ```bash
   history
   ```

---

## Aide intégrée

Linux fourni des commandes permettant de chercher des commandes, d'obtenir leur description et plus.

**Commandes** : `man`, `apropos`, `history`

| Besoin | Commande | Explication |
|--------|----------|-------------|
| Lire la documentation | `man commande` | Manuel officiel |
| Quitter | `q` | Très important |
| Chercher une commande | `apropos mot` | Recherche par mot-clé |
| Voir l’historique | `history` | Toutes les commandes récentes |


**Commande `man`**

* La commande `man` affiche les pages de manuel des commandes.
* On l'utilise pour comprendre comment une commande fonctionne grâce à son **manuel**.

```bash
man ls
man cp
man chmod
```

Les sections importantes :

| Section     | Ce qu’elle contient                                |
| ----------- | -------------------------------------------------- |
| **NAME**        | Nom + courte description                           |
| **SYNOPSIS**    | Format général de la commande (arguments, options) |
| **DESCRIPTION** | Détails sur le fonctionnement                      |
| **OPTIONS**     | Toutes les options disponibles                     |
| **EXAMPLES**    | Exemples (si la page en contient)                  |


**Commande `apropos`**

* On l'utilise quand on **ne connaît pas** la commande mais qu’on connaît l’action souhaitée. 
* Elle permet de trouver la bonne commande quand on **ne la connaît pas**.
* Elle renvoie une liste de commandes dont la description contient le mot recherché.

```bash
apropos directory
apropos remove
apropos copy
```
--- 

# Exercices (à la maison)

## 🟢 Exercice #1 : Le système de fichiers

*Vous devez trouver les réponses en explorant.*

1. Allez dans le dossier `/etc`. Combien y a-t-il de fichiers (approximativement) ? (`ls`). <!--*(réponse : plus de 2000)*-->
2. Trouvez le fichier nommé `passwd` dans `/etc`. Copiez-le dans votre dossier personnel (`~`).
> [!warning]
> <span style="color:red;"><b>Attention</b></span> : Ne modifiez pas le fichier `passwd` original. Travaillez sur la copie.
3. Renommez votre copie `utilisateurs_backup.txt`.
4. Créez un dossier `Confidentiel` dans votre home.
5. Déplacez `utilisateurs_backup.txt` à l'intérieur de `Confidentiel`.
6. Revenez à votre point de départ (`~`) et supprimez le dossier `Confidentiel` et son contenu en une seule commande.


## 🟢 Exercice 2 : L'aide

1. Quelle est la différence entre `man` et `apropos` ?
2. Que représente la section **SYNOPSIS** dans une page `man` ?
3. Comment chercher une chaîne de texte **à l’intérieur d’un manuel** ?
   *(indice : `/texte`)*
4. Quelle touche du clavier permet de **quitter** le manuel ?
5. Quel symbole indique qu’un argument est **optionnel** dans le SYNOPSIS ?
   <!--*(réponse : `[ ]`)*-->

---

# Laboratoires 

>> Semaine 2

* Cours 1: **Labo #1** - Installation d'AlmaLinux (VirtualBox)
* Cours 2: **Labo #2** - Les commandes de base
