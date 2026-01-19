+++
pre = 'Semaine 12 : '
title = 'Introduction au Scripting Bash'
weight = 120
+++


## Objectif de la semaine 

* Comprendre comment automatiser des tâches simples en transformant des commandes manuelles en un programme exécutable.

**Fichier pour les exercices (en classe)**  
Utiliser le fichier **exo-semaine12.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine12.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

## Qu'est-ce qu'un script Bash ?

Imaginez que Linux est un orchestre rempli d'instruments (les commandes `ls`, `cd`, `mkdir`, etc.).

* **En ligne de commande**, vous jouez d'un instrument à la fois.
* **Un script Bash**, c'est le **chef d'orchestre**. Il a une partition (le fichier texte) et il dit à chaque instrument quand jouer, combien de temps attendre, et comment jouer ensemble.

Un script n'est rien de magique. C'est simplement **un fichier texte avec l'extension `.sh`** qui contient une liste de commandes que vous auriez tapées à la main, l'une après l'autre.



## L'anatomie d'un script

Un script a besoin de deux choses pour exister :

1. **Le Shebang** (`#!/bin/bash`) : La première ligne qui dit "Ceci est un script Bash".
2. **La permission** (`chmod +x`) : Le droit d'être exécuté.


### 1. Le Shebang (`#!`)

C'est la ligne la plus importante. Elle doit être **tout en haut**, sur la ligne 1.

* `#!` : "Hey Linux, voici le chemin vers l'interprète pour ce code".
* `/bin/bash` : "Utilise le langage Bash standard".

### 2. Le droit d'être exécuté

Si vous essayez de lancer un script directement (ex.: taper juste `mon_script.sh`), cela échouera. Il y a deux règles de sécurité à respecter.

#### Étape 1 : Donner la permission (`chmod`)

Par défaut, Linux empêche l'exécution de fichiers textes pour éviter les virus ou les erreurs. Vous devez donner le "Droit d'exécution" (`x` pour eXecute).

```bash
chmod +x mon_script.sh
```

#### Étape 2 : L'appel explicite (`./`)

Si vous tapez juste `mon_script.sh`, Linux répondra "Commande introuvable" ou *"Permission denied"*. Par sécurité, Linux ne cherche jamais dans le dossier actuel. Il faut lui dire : *"Lance le script qui est **ici** (`.`) dans ce dossier (`/`)"*.

```bash
./mon_script.sh
```


### 3. Les commentaires (`#`)

Tout ce qui suit un `#` est pour **vous**, les humains. L'ordinateur l'ignore. Utilisez-les pour expliquer votre code à votre futur vous (qui aura tout oublié dans 2 semaines).


### 🟢 Exercice 1 (En classe)

**But :** Créer un fichier, comprendre le Shebang et utiliser `echo`.

1. Ouvrez votre éditeur de texte.
2. Écrivez un script qui :
   * A le bon Shebang.
   * Contient le commentaire "C'est mon premier script Bash"
   * Affiche la phrase : "Aujourd'hui, nous sommes le :".
   * Exécute la commande `date` juste après.
3. Sauvegardez le fichier sous le nom `bonjour.sh`.

> [!primary]
> Utiliser `echo` pour afficher des messages à l'écran.

## Les variables

En programmation, une variable est une boîte avec une étiquette. On peut y mettre de l'information pour la réutiliser plus tard.

### 1. Les espaces (Erreur #1 des débutants)

En Bash, la syntaxe est impitoyable. Il ne faut **JAMAIS** mettre d'espaces autour du signe égal.

* ❌ `PRENOM = "Mario"` (Linux cherche la commande "PRENOM").
* ✅ `PRENOM="Mario"` (Linux stocke "Mario" dans la boîte PRENOM).

### 2. Initialisation vs lecture d'une variable

* **Pour remplir la boîte** : On utilise le nom seul (`COURS="Linux"`).
* **Pour voir le contenu** : On utilise le symbole dollar `$` (`echo $COURS`).

```bash
#!/bin/bash
FRUIT="Pomme"           # Je mets "Pomme" dans la boîte
echo "J'aime la $FRUIT" # J'affiche le contenu
```


### 🟢 Exercice 2 (En classe)

**But :** Manipuler des variables sans faire d'erreurs d'espaces.

1. Créez un nouveau script `identite.sh`.
2. Créez deux variables : `NOM` (avec votre nom) et `AGE` (avec votre âge).
3. Faites afficher une phrase complète en utilisant ces variables :
   * *Exemple de résultat attendu :* "Je m'appelle Patrick et j'ai 25 ans."
4. N'oubliez pas le rituel d'exécution (`chmod` + `./`) !



## Rendre le script intelligent (Interactivité)

Un script statique est ennuyeux. Rendons-le dynamique.

### 1. Méthode A : Poser une question (`read`)

Le script s'arrête et attend que l'utilisateur tape quelque chose.

```bash
read -p "Quel est ton film préféré ? " FILM
echo "Ah oui ? J'adore aussi $FILM !"
```
> [!primary]
> L’option `-p` de la commande `read` permet d’afficher un message avant la saisie de l’utilisateur. Elle sert à guider l’utilisateur en lui indiquant quoi entrer.


### 2. Méthode B : Les arguments (`$1`, `$2`...)

Au lieu de poser une question pendant l'exécution, on donne l'info **au lancement**.
Exemple : `./mon_script.sh Toto`

Ce que Bash comprend automatiquement :

* `$0` : Le nom du script (`./mon_script.sh`)
* `$1` : Le premier mot (argument) après le script (`Toto`)
* `$2` : Le deuxième mot, etc.
* `$#` : Le nombre total d'arguments donnés.

**Exemple concret : `script.sh`**

```bash
#!/bin/bash
echo "Création du dossier $1 pour le projet."
mkdir $1
```

*Si je lance `./script.sh SiteWeb`, il créera le dossier "SiteWeb" et affichera la phrase "Création du dossier SiteWeb pour le projet.".*



### 🟢 Exercice 3 (En classe)

**But :** Combiner `read` ou les arguments `$1` avec une vraie commande système (`mkdir`).

1. Créez un script `archive.sh`.
2. Le script doit demander à l'utilisateur : "Quel est le nom du dossier à créer ?".
3. Récupérez la réponse dans une variable.
4. Créez ce dossier avec la commande `mkdir`.
5. Ensuite, entrez dans ce dossier (`cd`) et créez un fichier vide nommé `info.txt` (`touch info.txt`).
6. Affichez "Opération terminée !" à la fin.



## Contrôler le temps : sleep et wait

Parfois, l'exécution du script doit marquer une pause.

### 1. La sieste (`sleep`)

La commande `sleep` suspend l'exécution. Utile pour attendre qu'un téléchargement finisse ou pour laisser le temps à l'utilisateur de lire.

```bash
echo "Attention, fermeture dans 3 secondes..."
sleep 1
echo "2..."
sleep 1
echo "1..."
sleep 1
echo "Bye !"
```

### 2. La synchronisation (`wait`)

Si vous lancez des tâches lourdes en arrière-plan (avec le symbole `&` à la fin de la ligne), le script risque de se terminer avant elles.
`wait` dit au script : *"Ne quitte pas tant que les tâches lancées ne sont pas finies"*.

```bash
./gros_calcul.sh &   # Je lance ça en arrière-plan
echo "Je fais autre chose pendant ce temps..."
wait                 # J'attends que le gros calcul finisse
echo "Tout est fini."
```

---

# Laboratoire 

Vous êtes administrateur système. Vous en avez assez de demander les informations manuellement aux nouveaux employés. Vous allez créer un script d'accueil automatisé.

### Étape 1 : Le squelette

Créez un script nommé `setup_user.sh`.

* Il doit commencer par le bon Shebang.
* Il doit afficher "Bienvenue dans l'assistant de configuration".
* Rendez-le exécutable et testez-le.

### Étape 2 : Collecte d'informations (Mode interactif)

Modifiez le script pour qu'il demande (avec `read`) :

1. Le nom de l'utilisateur (stocké dans `NOM`).
2. Le département de travail (stocké dans `DEPT`).

### Étape 3 : Simulation d'installation (Sleep)

Affichez un message "Création du compte pour $NOM dans le département $DEPT...".

* Ajoutez une pause de 3 secondes (`sleep 3`) pour faire "style" que l'ordinateur travaille fort.
* Affichez "Configuration terminée !".

### Étape 4 : Les arguments (Mode expert)

Modifiez votre script pour qu'il puisse **aussi** accepter le nom et le département directement en arguments au lancement du script (ex: `./setup_user.sh Frédéric Comptabilité`).

***Indice** : Affichez simplement les variables `$1` et `$2` au début pour vérifier si elles existent.*

### Étape 5 : Le fichier journal

Faites en sorte que votre script écrive le résultat (Nom et Département) dans un fichier texte nommé `log.txt` au lieu de (ou en plus de) l'afficher à l'écran.  
***Indice** : Utilisez les chevrons de redirection `>>`.*


---

## Corrigé du laboratoire

> À venir (samedi ou dimanche)

<!--
## Aide-mémoire pour l'enseignant (Solutions du Lab)

**Solution Étape 4 (Version simple sans conditionnelle `if`)**
Note : À ce stade, ils ne connaissent pas encore `if/else`, donc on leur montre simplement comment afficher les arguments.

```bash
#!/bin/bash

# --- Mode Arguments ---
echo "Mode arguments détecté :"
echo "Nom: $1"
echo "Dépt: $2"

echo "-----------------"

# --- Mode Interactif ---
echo "Bienvenue dans l'assistant."
read -p "Entrez votre nom : " NOM
read -p "Entrez votre département : " DEPT

echo "Installation du compte pour $NOM ($DEPT)..."
sleep 3
echo "Terminé !"
```
-->