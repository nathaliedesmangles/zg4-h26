+++
pre = 'Semaine 3 : '
title = 'Gestion de paquets et édition de texte'
weight = 30
+++


### Objectif de la semaine

* Comprendre les dépôts, mises à jour (apt update/upgrade) et installation (apt install vim/htop).  
* Être capable de modifier un fichier de configuration sur un serveur distant sans souris.


Utiliser le fichier **exo-semaine3.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine3.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

# Théorie


## Gestion des paquets

### 1. La fin des fichiers .exe (Les dépôts)

Sous Windows, quand vous voulez un logiciel, vous ouvrez le navigateur, vous cherchez un site (parfois douteux), vous téléchargez un `.exe` et vous cliquez sur "Suivant, Suivant, Suivant".

Sous Linux, cette méthode est archaïque. Nous utilisons des **Gestionnaires de paquets** connectés à des **dépôts** (*repositories*). C'est une immense bibliothèque centralisée, validée et sécurisée par les créateurs de votre distribution. Linux Mint utilise **APT** (*Advanced Package Tool*), l'ancêtre de l'App Store.


### 2. APT : Le standard Debian/Mint

Sur Linux Mint, le chef d'orchestre s'appelle **APT**. Voici les commandes que vous taperez tous les jours. Notez l'utilisation de `sudo`, la commande magique qui dit "Je suis l'administrateur". Sans elle, vous n'avez pas le droit d'installer des logiciels.

> **Analogie : Le menu du restaurant**
> Beaucoup de débutants confondent `update` et `upgrade`. Imaginez que vous êtes au restaurant :
> * `sudo apt update` : **Vous demandez la carte du jour.**
> *"Qu'est-ce qu'il y a de nouveau ?"* (Cela ne vous donne rien à manger, cela met juste à jour votre connaissance des plats disponibles).
> * `sudo apt upgrade` : **Vous commandez les plats améliorés.**
> *"Remplacez mes vieux plats par les nouvelles versions de la carte."* (C'est là que l'installation réelle se fait).
> 

#### Commandes essentielles :

```bash
# 1. ACTUALISER (Toujours faire ceci en premier !)
# Télécharge la liste des dernières versions disponibles.
sudo apt update

# 2. INSTALLER
# Télécharge et installe le logiciel (ex: l'éditeur vim ou nano) et ses dépendances.
sudo apt install vim  # ou sudo apt install nano

# 3. SUPPRIMER
# Désinstalle le logiciel.
sudo apt remove nano
```

> [!primary]
> **Culture générale : Le gestionnaire de paquet DNF**  
> Linux est une grande famille. Mint et Ubuntu utilisent **APT**. Mais dans le monde professionnel, vous croiserez souvent la famille Red Hat (Fedora, RHEL, AlmaLinux) qui utilise **DNF**. C'est une abréviation de *"Dandified YUM"* et c'est le successeur moderne de l'ancienne commande `yum`.
> C'est exactement la même logique qu'avec **APT**. Seul le mot change :

| Action | Mint / Ubuntu | Red Hat / Fedora |
| --- | --- | --- |
| Mettre à jour la liste | `apt update` | `dnf check-update` |
| Installer | `apt install` | `dnf install` |

### 🟢 Exercice 1 (en classe)

Pour pratiquer l'installation de logiciels, nous allons installer un petit programme inutile mais indispensable pour impressionner vos amis : **cmatrix**.

#### Étape 1 : Le constat d'échec

Essayez de lancer le programme. Comme il n'est pas encore installé, Linux va vous signaler une erreur.

```bash
etudiant@linux:~$ cmatrix
La commande « cmatrix » est introuvable.
```

#### Étape 2 : L'installation

Nous allons récupérer le paquet depuis les dépôts officiels.

```bash
# 1. On met à jour le catalogue (Réflexe !)
sudo apt update

# 2. On installe le paquet
sudo apt install cmatrix
```

*Si le système vous demande confirmation [O/n], appuyez sur **O** (Oui) ou **Entrée**.*

#### Étape 3 : Le résultat

Maintenant, lancez la commande :

```bash
cmatrix
```

Votre terminal devrait se transformer en pluie de code numérique vert ! Appuyez sur **F11** pour passer en plein écran.

> [!primary]
> **Comment sortir ?**  
> Vous êtes coincé dans la Matrice ?
> Pour quitter la plupart des programmes en terminal qui tournent en boucle, appuyez sur la touche **q** (Quit) ou utilisez la combinaison **Ctrl + C**.

#### Étape 4 : Le nettoyage

L'exercice est fini. Désinstallons le programme pour laisser la machine propre.

```bash
sudo apt remove cmatrix
```


## Pourquoi un éditeur texte ?

*« Imaginez : Le serveur de l'entreprise a planté. Vous êtes connecté à distance (SSH). Pas de souris. Pas de Word. Juste un écran noir. Comment vous réparez le fichier de config ? »*

Sur Linux, la configuration ne se fait pas avec des cases à cocher, mais en écrivant du texte dans des fichiers `.conf` .

* **GUI (Gedit, VS Code) :** Confortable, mais inutile si vous n'avez pas d'interface graphique (99% des serveurs).
* **Nano :** Facile, les raccourcis sont écrits en bas (`^O`, `^X`). Idéal pour débuter, mais limité.
* **VI / VIM**[^1]: Il a fait ses preuves depuis plusieurs décennies (1976 / 1991).
   * **Inconvénient :** Contre-intuitif au début.
   * **Avantage :** Installé par défaut sur TOUS les systèmes Unix/Linux du monde (même les routeurs). Si vous connaissez VI, vous pouvez tout réparer .



### 1. Le concept des "Modes"

C'est là que tout le monde bloque. VI est un éditeur **modal**.

On trouve plusieurs modes :

1. **Mode Commande (Normal) :** C'est le mode par défaut au démarrage. Les touches ne servent pas à écrire, mais à *agir*. Par exemple, si vous tapez "j", ça ne s'écrit pas, ça descend le curseur. C'est dans ce mode qu'on peut:
   * Supprimer, copier, couper, coller, sauvegarder.
   * Déplacer le curseur (haut, bas, droite et gauche, avec quantificateurs).
   * Rechercher des caractères.
2. **Mode Insertion :** C'est le mode "Notepad". Il permet d’ajouter/insérer des caractères. D'écrire donc.
3. **Mode ligne de commande** : C'est le mode qui permet :
   * Exécuter une commande externe.
   * Remplacer une commande.
   * Quitter, enregistrer, fermer le terminal.


> [!primary]
> Pour passer de **Commande** à **Insertion**, on tape `i`. Pour revenir, on tape **toujours** `Esc` (Échappement).  
> **Conseil :** Si vous êtes perdu, utilisez la touche `Esc` jusqu'à ce que ça bip.


#### Les commandes VIM à connaitre

1. **Mode Insertion** (Écrire)

*Pour sortir de ce mode, appuyez sur `ESC`.*

| Touche | Action | Position du curseur après action |
| --- | --- | --- |
| **i** | **I**nsérer | Avant le curseur |
| **a** | **A**jouter | Après le curseur |
| **I** | **I**nsérer au début | Au tout début de la ligne |
| **A** | **A**jouter à la fin | À la toute fin de la ligne |
| **o** | **O**uvrir (dessous) | Sur une nouvelle ligne **après** le curseur |
| **O** | **O**uvrir (dessus) | Sur une nouvelle ligne **avant** le curseur |
| **ESC** | Revenir au **Mode Commande** (à faire si vous êtes perdu) |

2. **Mouvements** (Déplacements)


| Touche | Déplacement |
| --- | --- |
| **h** | Un caractère à **gauche**  |
| **l** (Lettre L) | Un caractère à **droite**  |
| **j** | Une ligne **en bas**  |
| **k** | Une ligne **en haut**  |
| **0** (Zéro) | Revient au **début** de la ligne |
| **$** | Va à la **fin** de la ligne |
| **w** | Va au début du **mot suivant** (*word*) |
| **e** | Va à la **fin** du mot courant (*end*) |
| **b** | Va au début du mot **précédent** (*back*) |
| **gg** | Aller au tout **début** du document (1ère ligne) |
| **G** | Aller à la **dernière ligne** du document |
| **G$** | Aller à la **fin de la dernière ligne** |
| **:numéro** (ex. `:100`) | Aller à une ligne précise  |


> [!primary]
Une virgule (`,`) sera considérée comme un mot.


3. **Quantificateurs**

*La plupart des commandes de **mouvement** ou **d'édition** peuvent être précédées d'un chiffre.*

| Commande | Action |
| --- | --- |
| **2w** | Avancer de **2** mots |
| **5j** | Descendre de **5** lignes |
| **10dd** | Effacer **10** lignes |

4. **Effacer et couper**

***Note** : Dans VI, "Effacer" met le texte en mémoire (comme "Couper").*

| Commande | Action |
| --- | --- |
| **x** | Efface le **caractère** sous le curseur |
| **dw** | Efface le **mot** sous le curseur |
| **de** | Efface jusqu'à la **fin du mot** |
| **d$** | Efface jusqu'à la **fin de la ligne** |
| **dd** | Efface **toute la ligne** actuelle |
| **d2w** | Efface les **2** prochains mots |
| **2dd** | Efface les **2** prochaines lignes |

5. **Annuler et rétablir**

| Touche | Action |
| --- | --- |
| **u** | **Annule** la dernière commande (*undo*) |
| **U** | Annule tous les changements sur la ligne courante |
| **CTRL + R** | **Rétablit** l'annulation (*redo*) |

6. **Copier et coller**

| Commande | Action |
| --- | --- |
| **yy** | Copie la ligne courante (*yank*) |
| **Y** | Copie la ligne courante (Similaire à `yy`) |
| **y$** | Copie jusqu'à la fin de la ligne |
| **p** | **Colle** le contenu après le curseur (*paste*) |
| **P** | Colle le contenu avant le curseur |
| **r** | Remplace le caractère sous le curseur (un seul) |
| **v0$y** | Copie la ligne (enchaînement : mode **v**isuel, début, fin, **y**ank) |

7. **Rechercher**

| Commande | Action |
| --- | --- |
| **/** | Recherche une occurrence vers le bas (ex: `/mot`) |
| **n** | Va à l'occurrence suivante |
| **N** | Va à l'occurrence précédente |
| **%** | Va à la parenthèse ou l'accolade correspondante `( { [ ] } )` |


9. **Gestion du fichier**

| Commande | Action |
| --- | --- |
| `:w` | **Enregistre** le fichier (*write*) |
| `:w nom` | Enregistre sous un nouveau nom ("Enregistrer sous...") |
| `:q` | **Quitte** (si aucune modification n'a été faite) |
| `:q!` | Quitte **sans enregistrer** (Force la fermeture) |
| `:wq` ou `:x` | **Enregistre et Quitte** |

10. **Remplacer** (Search & Replace)

| Commande | Portée | Action |
| --- | --- | --- |
| `:s/aa/bb` | Ligne courante | Remplace la **première** occurrence de "aa" par "bb" |
| `:s/aa/bb/g` | Ligne courante | Remplace **toutes** les occurrences (*global*) |
| `:25,30s/aa/bb/g` | Lignes 25 à 30 | Remplace dans la plage de lignes spécifiée |
| `:%s/aa/bb/g` | Fichier entier | Remplace partout dans le fichier |
| `:%s/aa/bb/gc` | Fichier entier | Remplace partout avec **confirmation** à chaque fois |

11. **Divers et commandes Shell**

| Commande | Action |
| --- | --- |
| `:set nu` | Affiche les numéros de ligne |
| `:set nonu` | Masque les numéros de ligne |
| `:set number` | Affiche les numéros de ligne (identique à `:set nu`) |
| `:! cmd` | Exécute une commande shell temporairement (ex: `:! ls`) |
| `:r! cmd` | Insère le **résultat** d'une commande shell dans le fichier (ex: `:r! date`) |


> [!primary]
> On ne veut pas être des experts aujourd'hui, on veut juste survivre. 
> **Voici les commandes vitales** :
> * **Entrer :** `vi nom_fichier`
> * **Écrire :** `i` (Insert)
> * **Sortir du mode écriture :** `Esc` (Toujours faire ça après avoir écrit).
> * **Sauvegarder SANS Quitter :** `:w`
> * **Sauvegarder et Quitter :** `:wq` (Write Quit) ou `ZZ`.
> * **Quitter SANS sauvegarder :** `:q!` (Le point d'exclamation force la sortie).
> * **Supprimer une ligne :** `dd` (En mode commande).
> * **Déplacement** : `h` (haut), `j` (bas), `k` (droite), `l`(gauche), ou les flèches.
> * **Quantificateurs** : avec la plupart des commandes de **mouvement** ou **d'édition**.

> [!primary]
>
> 1. Tutoriel directement sur VIM
>
> Pour acceder au tutoriel en français, dans le Terminal, tapez la commande:
> ```bash
> etudiant@linux:~$ vimtutor fr
> ```
>
> 2. Vidéo YouTube: [Apprendre tout sur Vim](https://youtu.be/yfGbfZUzFq8?si=29JC21SMTF_Fur0l)

### 🟢 Exercice 2 (en classe)

* Lancer vim  
* Tapez i  
* Tapez Bonjour  
* Tapez Esc   
* Tapez :wq  


### 🟢 Exercice 3 (en classe)

* **Sans votre VM**, pour chaque action, dites quelle(s) touche(s) du clavier vous utiliseriez:

   * Je suis en mode Commande. Je veux écrire 'Bonjour'. Que fais-je ?
   <!--* **Réponse :** "Tape i !"-->

   * J'ai fini d'écrire. Que fais-je ?
   <!--* **Réponse :** "Touche Esc !"-->

   * Je veux supprimer ces 3 lignes rapidement. Que fais-je ?
   <!--* **Réponse :** "dd, dd, dd"-->

   * J'ai tout brisé, je veux sortir sans rien casser. Que fais-je ?
   <!--* **Réponse :** ":q!"-->


---

<!--
### 💡 Astuces de la semaine

> **VI CHEAT SHEET**
> 🟢 **JE VEUX ÉCRIRE :**
> 1. Appuie sur `i` (Insert).
> 2. Écris ton texte.
> 3. Appuie sur `Esc` DÈS QUE TU AS FINI.
> 
> 
> 🔴 **JE VEUX SORTIR :**
> * `Esc` + `:wq` -> Sauver et Quitter (Write & Quit).
> * `Esc` + `:q!` -> Quitter sans sauver (En cas d'erreur).
> 
> 
> 🔵 **JE VEUX EFFACER :**
> * `dd` -> Effacer la ligne actuelle.
> * `x` -> Effacer le caractère sous le curseur.
> * `u` -> Annuler.
-->
---

# LABORATOIRE

**Objectif :** Installer des programmes, créer et modifier des fichiers avec VI et Nano.

Utiliser le fichier **labo3.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo3.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}


## Exercice 1 - Gestionnaire de paquets (30 min)


Utilisez les commandes `apt search` (pour chercher) et `apt show` (pour vérifier) afin d'identifier le nom exact des paquets correspondant aux descriptions suivantes. Ensuite, installez-les (`install`) et testez-les.

| Description de l'outil | Indice de recherche | Nom du paquet (À compléter) | Commande pour tester |
| --- | --- | --- | --- |
| Affiche une locomotive quand on se trompe en tapant `ls`. | `locomotive` | `....................` | `sl` |
| Affiche le logo Linux Mint et les infos PC en couleur. | `system info` | `....................` | `neofetch` |
| Crée des bannières en texte géant (ASCII art). | `large characters` | `....................` | `figlet "Mon Nom"` |
| Un navigateur internet sans souris (mode texte). | `text mode web` | `....................` | `links2 www.google.com` |
| Transforme le terminal en écran de hacker de cinéma. | `melodrama` | `....................` | `hollywood` |

## Exercice 2 - Éditeurs de texte (65 min)

### Étape 0 : L'échauffement avec Nano (10 min)

*Juste pour savoir que ça existe.*

1. Ouvrez le terminal.
2. Tapez `nano test_nano.txt`.
3. Écrivez : "Ceci est mon premier fichier."
4. Regardez en bas de l'écran pour trouver comment quitter (`^X` signifie Ctrl+X).
5. Sauvegardez en confirmant avec `O` (Oui) ou `Y` (Yes) et Entrée.

### Étape 1 : Le baptême du feu VIM (15 min)

*On passe aux choses sérieuses.*

1. **Création :** `vi cv.txt`
2. **Insertion :**
* Appuyez sur `i`. (Le mot `-- INSERT --` apparaît en bas).
* Écrivez votre Nom, Prénom et "Technicien Informatique".
* Appuyez sur `Esc`. (Le mot `-- INSERT --` disparaît).


3. **Modification :**
* Utilisez les flèches (ou `h j k l`) pour aller sur la ligne "Technicien".
* Appuyez sur `dd`. La ligne disparaît ! (C'est normal).
* Appuyez sur `u` (Undo). La ligne revient !.


4. **Sauvegarde :**
* Tapez `:wq` et Entrée. Vous revenez au terminal.
* Vérifiez que le fichier existe avec `ls` et lisez-le avec `cat cv.txt`.



### Étape 2 : L'admin système junior (25 min)

*Simulation d'une tâche réelle.*

1. On va copier un fichier système pour s'entraîner sans risques.
   * `cp /etc/fstab ~/mon_fstab`
2. Ouvrez votre copie : `vi mon_fstab`.
3. **Mission de nettoyage :**
   * Supprimez toutes les lignes de commentaires (celles qui commencent par `#`) en utilisant `dd`.
   * Ajoutez une nouvelle ligne à la fin : `// Disque de sauvegarde`.
   * Pour aller à la fin du fichier rapidement : Tapez `G` (Majuscule G) en mode commande.
   * Pour aller au début : Tapez `gg`.
4. Sauvegardez et quittez.

### Étape 3 : Le défi (15 min)

Créez un fichier `secret.txt` avec VI. Il doit contenir exactement :

* Ligne 1 : "Mode Commande"
* Ligne 2 : "Mode Insertion"
* Ligne 3 : "Sauvegarder avec :wq"

**Contrainte :** Si vous ouvrez le fichier et qu'il y a une faute ou une ligne vide en trop, recommencez.

---


## Corrigé du laboratoire

> À venir (samedi ou dimanche)

<!--

Voici les solutions détaillées pour votre laboratoire. Elles incluent les réponses attendues pour le tableau et des notes de validation pour la partie pratique sur les éditeurs de texte.

---

## Solutions : Exercice 1 - Gestionnaire de paquets

Voici le tableau complété avec les noms exacts des paquets.

| Description de l'outil | Indice de recherche | Nom du paquet (Solution) | Commande pour tester |
| --- | --- | --- | --- |
| Affiche une locomotive quand on se trompe en tapant `ls`. | `locomotive` | **`sl`** | `sl` |
| Affiche le logo Linux Mint et les infos PC en couleur. | `system info` | **`neofetch`** | `neofetch` |
| Crée des bannières en texte géant (ASCII art). | `large characters` | **`figlet`** | `figlet "Mon Nom"` |
| Un navigateur internet sans souris (mode texte). | `text mode web` | **`links2`** | `links2 www.google.com` |
| Transforme le terminal en écran de hacker de cinéma. | `melodrama` | **`hollywood`** | `hollywood` |

---

## Solutions : Exercice 2 - Éditeurs de texte

### Étape 0 : L'échauffement avec Nano

**Validation :**
L'étudiant a réussi si la commande suivante affiche le texte sans erreur :

```bash
cat test_nano.txt
# Résultat attendu : Ceci est mon premier fichier.
```

### Étape 1 : Le baptême du feu VI

**Points clés à surveiller :**

* L'étudiant doit comprendre la différence entre le mode **Insertion** (pour écrire) et le mode **Commande** (pour sauvegarder/quitter).
* L'utilisation de `u` (Undo) est souvent une révélation pour eux : c'est le "Ctrl+Z" de VI.

**Validation :**

```bash
cat cv.txt
# Résultat attendu :
# Nom Prénom
# Technicien Informatique

```

### Étape 2 : L'admin système junior

**Séquence de solution optimale :**

1. `vi mon_fstab`
2. `gg` (Aller tout en haut).
3. Positionner le curseur sur les lignes commençant par `#` et appuyer sur `dd` répétitivement jusqu'à ce qu'elles disparaissent toutes.
4. `G` (Aller tout en bas).
5. `o` (Petit "o" : Ouvre une nouvelle ligne *en dessous* et passe en mode insertion automatiquement).
6. Écrire : `// Disque de sauvegarde`.
7. `Esc` (Quitter le mode insertion).
8. `:wq` (Write & Quit).

**Validation :**
Le fichier `mon_fstab` ne doit plus contenir de commentaires (lignes bleues généralement) et doit avoir la nouvelle ligne à la fin.

### Étape 3 : Le défi

**Solution pas à pas (Keystrokes) :**

Voici la séquence exacte de touches pour réussir le défi parfaitement :

1. `vi secret.txt`
2. `i` (Pour entrer en mode insertion)
3. `Mode Commande` (Taper le texte)
4. `Entrée` (Pour changer de ligne)
5. `Mode Insertion` (Taper le texte)
6. `Entrée` (Pour changer de ligne)
7. `Sauvegarder avec :wq` (Taper le texte)
8. `Esc` (Très important : sortir du mode insertion avant de sauvegarder)
9. `:wq`
10. `Entrée`

**Validation stricte :**
Utilisez `cat` pour vérifier le contenu et `wc` pour vérifier le nombre de lignes (doit être 3).

```bash
cat secret.txt
wc -l secret.txt  
# Doit afficher "3 secret.txt"

```

-->

[^1]:  **VIM** c'est pour ***Vi IMproved***, soit "vi amélioré".

