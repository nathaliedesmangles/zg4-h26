+++
pre = 'Semaine 13 : '
title = 'Logique conditionnelle & tests sur fichiers'
weight = 130
+++

## Objectif la semaine

* Créer des scripts dynamiques qui s'adaptent aux paramètres donnés et à l'état du système.

**Fichier pour les exercices (en classe)**  
Utiliser le fichier **exo-semaine2.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine2.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

# Théorie

## Le flux de contrôle (`if/else/fi`)

Jusqu'à présent, vos scripts étaient comme des **robots aveugles** : ils exécutaient une liste d'ordres l'un après l'autre, sans se soucier de savoir si l'ordre précédent avait fonctionné.

Avec le flux de contrôle, nous allons leur donner une **intelligence**. Ils pourront dire : *"Si la copie échoue, je m'arrête"* ou *"Si le dossier n'existe pas, je le crée"*.


### 1. Le code de retour (`$?`) et `exit`

Comment le script sait-il si une commande a réussi ? Grâce à un petit numéro invisible qu'on appelle le **Code de Retour** (Exit Code).

Chaque fois qu'une commande se termine, elle laisse un message au système, stocké dans la variable spéciale `$?`.


> [!primary]
> **LA LOGIQUE INVERSÉE DE LINUX**  
> Oubliez la logique scolaire où 100% ou 1 est le meilleur score. En informatique système :
> * **0 (Zéro) = SUCCÈS** ("Zéro erreur", tout s'est bien passé).
> * **1 à 255 = ÉCHEC** (Il y a eu un problème).

**Exemples :**

```bash
ls /home
echo $?   
# Affiche 0 (Succès : le dossier existe)

ls /dossier_qui_n_existe_pas
echo $?   
# Affiche 2 (ou autre chiffre > 0 : Échec)
```
> [!primary]
> * **Arrêter le script :** `exit 1` (Arrêt d'urgence avec code d'erreur).


### 2. Les opérateurs de combinaison (`&&`, `||`, `;`)

On peut enchaîner des commandes "à la suite" sur une seule ligne. Le choix du symbole dicte le comportement du script.

| Opérateur | Nom | Logique (Le "Si") | 
| --- | --- | --- | 
| **`;`** | Point-virgule | **Fais A puis fais B** (Peu importe si A explose) | 
| **`&&`** | **ET** (AND) | **Si A réussit, fais B** (Arrêt immédiat si A échoue) |
| `\|\|` | **OU** (OR) | **Si A échoue, fais B** (Arrêt immédiat si A réussit) |

**Exemple concret :**

```bash
# Créer un dossier ET entrer dedans immédiatement (si la création a marché)
mkdir projet && cd projet

# Tenter une copie, OU afficher une alerte si elle rate
cp rapport.txt /backup/ || echo " Erreur : Sauvegarde échouée !"
```


### 3. La structure IF-THEN / ELSE / FI

Les opérateurs `&&` et `||` sont parfaits pour des actions courtes. Mais pour des scénarios complexes, on utilise le bloc `if`.

**La syntaxe**
La structure commence par `if` (si) et se termine obligatoirement par `fi` (if à l'envers).

```bash
if [ condition ]; then
    # Commandes exécutées si c'est VRAI (Code de retour 0)
    echo "C'est vrai !"
else
    # Commandes exécutées si c'est FAUX (Code de retour > 0)
    echo "C'est faux !"
fi
```

> [!primary]
> **LE PIÈGE MORTEL DES ESPACES**  
> En Bash, le crochet `[` n'est pas une simple décoration. C'est une **commande** (un alias de la commande `test`). Comme toute commande, elle a besoin d'espaces autour d'elle.
> * ❌ `if [$a="non"]` : **ERREUR** (Bash cherche une commande nommée `[$a="non"]` qui n'existe pas).
> * ✅ `if [ "$a" = "non" ]` : **SUCCÈS** (Il y a un espace après `[` et avant `]`).



### 4. Les tests

Bash ne teste pas un fichier comme il teste un nombre. Il faut utiliser les bons "drapeaux" (flags).

#### Pour les FICHIERS

* `-e` : **E**xiste (que ce soit un fichier ou un dossier).
* `-f` : Est un **F**ichier (File) standard.
* `-d` : Est un **D**ossier (Directory).

#### Pour les NOMBRES (Entiers)

* `-eq` : **Eq**ual (Égal à).
* `-gt` : **G**reater **T**han (Plus grand que).
* `-lt` : **L**ess **T**han (Plus petit que).
* `-ne` : **N**ot **E**qual (Différent de).

#### Pour le TEXTE (Chaînes de caractères)

* `=` : Est identique à.
* `!=` : Est différent de.
* `-z` : La chaîne est vide (Zero length).

**L'astuce de l'inverse (!)**
On peut inverser n'importe quel test avec le point d'exclamation (le "NOT").
`if [ ! -d "/tmp" ]` signifie "Si le dossier /tmp **N'EXISTE PAS**".



Voici des petits exercices pour tester votre compréhension immédiate.


### 🟢 Exercice 1 (en classe)

*Qu'est-ce qui va s'afficher si on lance `./script.sh 5`.*

```bash
#!/bin/bash
VALEUR=$1
if [ $VALEUR -gt 10 ]; then
    echo "Grand"
else
    echo "Petit"
fi
```
<!--
**Réponse :** "Petit". (Car 5 n'est pas plus grand que 10).
-->


### 🟢 Exercice 2 (en classe)

Sans utiliser `if`, écrivez une seule ligne de commande qui :

1. Crée un dossier nommé `Archives`.
2. **Si** la création réussit, affiche le message "Dossier créé".
3. **Si** la création échoue (car il existe déjà), affiche "Le dossier existe déjà".

> ***Indice** : Utilisez `mkdir`, `&&` et `||`.*

### 🟢 Exercice 3 (en classe)

Le script suivant contient **3 erreurs** qui l'empêchent de fonctionner. Trouvez-les et corrigez-les.


```bash
#!/bin/bash
NOM="Batman"

if [$NOM = "Batman"]
    echo "Je suis la nuit."
else
    echo "Tu n'es pas le héros."
fi
```

### 🟢 Exercice 4 (en classe)

Créez un script nommé `verif_age.sh`.

1. Déclarez une variable `AGE=15`.
2. Utilisez un `if` pour vérifier si l'âge est plus grand ou égal (`-ge`) à 18.
3. Si oui, affichez "Majeur". Sinon, affichez "Mineur".

<!--

Solutions (Pour le professeur)

**Solution Ex 1 :**

```bash
mkdir Archives && echo "Dossier créé" || echo "Le dossier existe déjà"

```

**Solution Ex 2 :**

1. Manque d'espace après `[` et avant `]`.
2. Manque le `;` après le crochet fermant (ou retour à la ligne avant `then`).
3. Manque le mot-clé `then`.

*Correction :*

```bash
if [ "$NOM" = "Batman" ]; then
```

**Solution Ex 3 :**

```bash
#!/bin/bash
AGE=15
if [ "$AGE" -ge 18 ]; then
    echo "Majeur"
else
    echo "Mineur"
fi
```
-->

---

# LABORATOIRE

**Objectif :** Créer des outils d'administration robustes qui ne plantent pas à la moindre erreur.

Utiliser le fichier **labo13.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo13.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

### Étape 1 : Les arguments

*Comprendre `$1`, `$2` etc.*

1. Créez `analyse.sh`.
2. Il doit afficher :
   * "Le nom du script est : [nom]"
   * "Tu m'as donné [nombre] arguments."
   * "Le premier est : [arg1]"
   * "Le deuxième est : [arg2]"

3. Testez avec : `./analyse.sh banane pomme` et `./analyse.sh`.

### Étape 2 : Conditions fichiers

***Scénario** : Créer un dossier de projet, mais afficher un message informant l'utilisateur s'il existe déjà.*

1. Créez `creer_projet.sh`.
2. Il prend un nom de dossier en argument (`$1`).
3. **Logique :**  
   * Vérifier si `$1` est vide (sécurité).
   * Vérifier si le dossier existe déjà (`if [ -d "$1" ]`).
   * **SI OUI :** Afficher "Erreur : Le projet $1 existe déjà !" et quitter (`exit 1`).
   * **SI NON :** Le créer (`mkdir "$1"`) et afficher "Projet $1 créé avec succès.".



### Étape 3 : UID et Exit 

***Scénario** : Un script de maintenance qui DOIT être lancé en root.*

1. Créez `maintenance.sh`.
2. Le script doit vérifier qui le lance.
   * **Astuce** : La variable `$UID` vaut 0 pour root.
3. **Logique :**
   * `if [ "$UID" -ne 0 ]; then`
   * Afficher "STOP ! Vous devez être root (sudo) pour lancer ce script."
   * `exit 1`
   * `fi`
   * Afficher "Lancement de la mise à jour..." (et faire un faux `sleep 2`).
4. Testez-le en tant qu'étudiant (Échec) et avec `sudo` (Succès).


### Étape 4 : Synthèse

*Combiner `grep`, variables, arguments et conditions.*

1. Créez `check_user.sh`.
2. Il prend un nom d'utilisateur en argument.
3. Il cherche cet utilisateur dans `/etc/passwd` (utilisez `grep -q` pour le mode silencieux).
4. Si `grep` trouve (code retour `$?` égal à 0) : Affichez "L'utilisateur existe."
5. Sinon : Affichez "Utilisateur inconnu."


## Corrigé du laboratoire

> À venir (samedi ou dimanche)