+++
pre = 'Semaine 4.1 : '
title = "Labo #3 - Utilisateurs, groupes et droits d'accès"
weight = 17
draft = false
+++


> Directives à venir


<!--

## Objectif de la semaine

* Passer de la "manipulation fichier par fichier" à la "génération massive d'arguments".

**Fichier pour les exercices (en classe)**  
Utiliser le fichier **exo-semaine4.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine4.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---


## Pourquoi taper quand on peut générer ?


> *"Jusqu'à présent, si je vous demandais de créer 3 dossiers pour vos projets (projet_A, projet_B, projet_C), vous feriez 3 commandes ou vous taperiez les 3 noms. C'est lent et sujet aux fautes de frappe. Aujourd'hui, on va apprendre à faire travailler le Shell pour nous. On ne va plus écrire les noms de fichiers, on va écrire une **règle** pour les générer."*

**Concept clé :** Les accolades `{}` servent à **générer du texte** avant même que la commande ne s'exécute.



## Le mécanisme de base : Les Listes


* **Syntaxe** : `prefixe{terme1,terme2,terme3}suffixe`
* **Règle d'or** : **JAMAIS D'ESPACES** après les virgules !
* Exemple :
```bash
echo Je mange une {pomme,banane,poire}
# Résultat : Je mange une pomme Je mange une banane Je mange une poire
```
-->


<!--
*(Montrez que `echo` nous permet de voir ce que le shell "cuisine" avant de l'exécuter).*
-->

<!--

### 🟢 Exercice #1 (en classe)


* **Contexte :** Vous préparez l'arborescence pour l'année.
* **Tâche :** Créez en **une seule commande** trois répertoires nommés `Session_1`, `Session_2` et `Session_3`.
* **Commande à trouver :** `mkdir Session_{1,2,3}`


## L'automatisation numérique 


* Au lieu de tout lister, on donne un début et une fin.
* **Syntaxe** : `{début..fin}`
* L'importance du "Padding" (Zéro non significatif) : Expliquez pourquoi `01` est mieux que `1` pour le tri informatique.


### 🟢 Exercice #2 (en classe)

Vous avez besoin de "faux" fichiers pour tester un script de tri.

* **Tâche :** Créez 20 fichiers vides nommés `test_01.log` jusqu'à `test_20.log`.
* **Commande à trouver :** `touch test_{01..20}.log`
* *Challenge pour les rapides :* Créez aussi les fichiers `test_A.log` à `test_F.log` (`{A..F}`).
 


## L'élément vide


* Si on met une virgule sans rien avant (ou après), cela génère "rien".
* `{a,}` se déploie en "a" et "".
* C'est l'outil ultime pour ajouter une extension sans retaper le nom d'origine.


### 🟢 Exercice #3 (en classe)

> **Exercice 3 : La sauvegarde éclair**
> * **Contexte :** Vous allez modifier le fichier critique `configuration.conf` (créez-le d'abord avec `touch`). Vous devez en faire une copie `.bak` de sécurité.
> * **Contrainte :** Interdiction de retaper "configuration.conf" une deuxième fois.
> * **Commande à trouver :** `cp configuration.conf{,.bak}`
> * *Explication :* Le shell transforme ça en `cp configuration.conf configuration.conf.bak`



## Accolades `{}` vs Wildcards `*`

> [!warning]

> C'est ici que les étudiants confondent tout.

* **Wildcard (`*`)** : "Je regarde le disque. Je prends ce qui **existe**." (Filtre)
* **Accolades (`{}`)** : "Je m'en fiche du disque. Je **crée** des mots." (Générateur)


### 🟢 Exercice #4 (en classe)


1. Créez les fichiers `a.txt`, `b.txt` et `c.txt`.
2. Lancez : `ls {a,b,c,d}.txt`
3. Lancez : `ls [a-d].txt`


* **Question :** Pourquoi la première commande affiche-t-elle une erreur sur `d.txt` et pas la deuxième ?
<!--* **Réponse attendue :** Les accolades forcent la commande à chercher `d.txt` (qui n'existe pas), alors que le wildcard `*` ou `[]` ne liste que ce qu'il trouve.-->

<!--
### 🟢 Exercice #5 (en classe)

> * **Contexte :** Vous avez un dossier avec 100 images (`img_1.jpg` ... `img_100.jpg`).
> * **Tâche :** Vous devez supprimer **uniquement** les images 2, 4 et 6. `rm img_*.jpg` est interdit (trop dangereux).
> * **Commande à trouver :** `rm img_{2,4,6}.jpg`



## Conclusion 

1. Pas d'espaces dans les accolades.
2. Utilisez `{}` pour **créer** (mkdir, touch) ou pour des listes précises connues d'avance.
3. Utilisez `*` pour **chercher** ou sélectionner l'inconnu.


Utiliser le fichier **devoir_accolades.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/devoir_accolades.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

**Devoir maison :**
* Créer la structure de projet web complète en une ligne :

```
MonSite/
├── assets/
│   ├── css/
│   ├── img/
│   └── js/
├── index.html
└── contact.html
```
-->


<!--
### Objectif :

* Valider votre maîtrise de l'environnement, la navigation et l'édition de base.

---

# Révision

*« C'est le dernier arrêt avant le premier examen. Posez vos questions maintenant. »*

### 1. Le *"Crash course"* des erreurs fréquentes

Au lieu de répéter la matière, on attaque les erreurs que *tout le monde* fait à l'examen.

1. **Le drame du Slash (`/`) :**
   * *Erreur :* Taper `cd /home/etudiant/Documents` alors qu'on est déjà dans `/home/etudiant`.
   * *Rappel :* Si ça commence par `/`, tu repars du début du disque. Si ça ne commence pas par `/`, tu pars d'ici.

2. **Le dossier qui n'existe pas :**
   * *Erreur :* `cp fichier dossier` (Mais `dossier` n'existe pas encore).
   * *Règle :* `cp` ne crée pas de dossier. Il faut faire `mkdir dossier` **avant** de copier dedans.

3. **La panique VI :**
   * *Erreur :* Bloqué en train d'écrire des commandes dans le texte.
   * *Mantra :* "Si tu es perdu, `Esc` deux fois."

4. **La casse (Majuscules) :**
   * `Documents` n'est pas `documents`. Linux ne pardonne pas les fautes de frappe.



### 2. La simulation "Sprint"

*Exercice chronométré (non noté) pour mettre la pression et vérifier que les VMs fonctionnent.*

**Défi 15 minutes :**

1. Allez dans votre `home`.
2. Créez un dossier `PRE-TEST`.
3. Dedans, créez un fichier `moi.txt` avec votre prénom (utilisez VI).
4. Copiez ce fichier et nommez la copie `moi.bak`.
5. Supprimez le fichier original.
6. *Le prof passe vérifier les écrans.*

### 3. Apt et VIM

#### Piège #1 : La confusion `update` vs `upgrade`

C'est l'erreur numéro 1.

* ❌ **L'erreur :** Penser que `sudo apt update` met à jour vos logiciels (comme Windows Update).
* ✅ **La réalité :**
* `update` = **Télécharge la liste** (Le menu du restaurant). Ça ne change rien sur votre disque dur, sauf des fichiers textes.
* `upgrade` = **Installe les nouvelles versions** (Commande le plat).


> **Question d'examen type :** "Quelle commande permet de rafraîchir la liste des paquets disponibles ?"
> **Mauvaise réponse :** `upgrade`
> **Bonne réponse :** `update`



#### Piège #2 : L'oubli du "Chef" (`sudo`)

Vous ne pouvez pas toucher au système avec votre compte étudiant de base.

* ❌ **L'erreur :** Taper `apt install sl` et voir "Permission denied" ou "Verrouillage impossible".
* ✅ **La réalité :** Dès qu'on installe, modifie ou supprime un logiciel système, il faut **`sudo`** devant.



#### Piège #3 : Le "Mode" VI (Écrire des commandes dans le texte)

C'est le classique de VI. L'étudiant veut quitter, tape `:wq`, mais ça écrit `:wq` dans son fichier texte au lieu de fermer la fenêtre.

* ❌ **L'erreur :** Oublier dans quel mode on est.
* ✅ **La règle d'or :**
* Si vous voulez écrire : appuyez sur **`i`**.
* Si vous voulez faire **N'IMPORTE QUOI D'AUTRE** (Sauver, Quitter, Effacer) : **Appuyez sur `Esc` d'abord !**


> **Astuce de survie :** Si vous êtes perdu, martelez la touche `Esc` jusqu'à entendre un *bip*. Là, vous êtes en sécurité.


#### Piège #4 : La panique "Je suis coincé"

Vous avez ouvert un fichier important (`/etc/fstab`), vous avez appuyé sur des touches au hasard, tout est cassé, et vous ne savez plus comment sortir sans faire de dégâts.

* ❌ **L'erreur :** Fermer le terminal avec la croix (X) ou débrancher la prise.
* ✅ **La sortie de secours :**
1. `Esc` (Pour être sûr d'être en mode commande).
2. `:q!` (Quitter **Force**).
*Le point d'exclamation dit à Linux : "Oui, je sais que j'ai fait des modifs, jette-les à la poubelle et sors d'ici".*



#### Le "Cheat Sheet" mental à mémoriser

Ayez ces séquences en tête :

1. **Installer un truc :** `sudo apt update` **PUIS** `sudo apt install nom_du_truc`
2. **VI - La séquence d'écriture :** `vi fichier` -> `i` -> (Tapez le texte) -> `Esc` -> `:wq`
3. **VI - La séquence de correction :** `vi fichier` -> (Aller sur la ligne) -> `dd` -> `Esc` -> `:wq`

*Des questions avant qu'on attaque la suite ?*

### Instantané d'une VM 

C'est le moment de :

* Redémarrer les VM pour être sûr qu'elles sont stables .
* Aller aux toilettes / Boire de l'eau.
* **Snapshots :** Comment faire un "Instantané" de la VM propre.



---

### 💡Astuces de la semaine

*Pour clore ce premier bloc :*

> **Le bilan du survivant (Bloc 1)**
> Si vous lisez ceci avant l'examen : Félicitations.
> Vous savez maintenant installer un OS, créer des dossiers, copier des fichiers et utiliser un éditeur modal.
> **À ce stade de la session, ce que vous devriez avoir acquis :**
> 1. L'indépendance vis-à-vis de la souris
>    * la structure du système Linux.
>    * les commandes de base et leurs options
> 2. La rigueur de la syntaxe (commandes).
> 3. La compréhension que "Tout est fichier".
> 4. L'autonomie pour chercher et installer de nouveaux logiciels.
> 5. Les bases pour survivre avec l'éditeur de texte VIM.


**Prochaine étape** :  
> **Séance 2** : Examen 1   
> **Semaine 5** :
> Maintenant que vous êtes installés, on va apprendre à gérer les humains (**Utilisateurs**) et à sécuriser vos fichiers (**Permissions**). Préparez-vous, vous allez devenir Administrateurs.
-->
---
<!--
# EXAMEN PRATIQUE 1 (1h30)

* Examen 1 (Bloc 1): 
   * **Temps alloué :** 90 minutes (1h30)
   * **Pondération :** 20%
   * **Documentation permise :** 1 feuille recto-verso manuscrite avec votre nom (Je la ramasserai après l'examen)
   * **Format :** Examen pratique

**Consignes pour l'enseignant :**

* L'examen se fait sur l'ordinateur, dans la VM Linux Mint installée en Semaine 1 .
* L'accès Internet/IA est interdit, ainsi que toute communication avec un.e autre étudiant.e.
* Livrable : Captures d'écran

---

## EXAMEN (Simulation)


### SECTION A : Arborescence et navigation (15 points)

*Effectuez toutes les opérations suivantes comme si vous étiez en ligne de commande, ce qui veut dire que toutes les commandes doivent être complètes (commande + option + cible).*

1. Vous êtes à cet emplacement: `XXX`. Positionnez vous dans votre dossier personnel (`/home/etudiant`). Quelles commandes (chemins absolu et relatif) permettent de le faire ? (**2 points**)
2. Écrire les commandes pour créer la structure de dossiers suivante (**9 points**: 2+3+4):
   ```
   EXAMEN_1
   |--- Sources
   |--- Backup
   |--- Projets
   |------- Web
   ```
3. Vérifiez que la structure est correcte. Quelle commande utiliseriez vous et Pourquoi ? (**4 points**)


### SECTION B : Manipulation de fichiers (15 points)

* Écrire les commandes complètes correspondantes. Au départ pour êtes dans votre dossier personnel.

1. Copiez le fichier système `/etc/fstab` vers votre dossier `EXAMEN_1/Sources`. Nommez la copie `copie_fstab`.(**3 points**)
2. Copiez le fichier `/etc/passwd` vers votre dossier `EXAMEN_1/Backup`. (**3 points**)
3. Renommez le fichier qui se trouve dans `Backup` : il doit s'appeler `copie_passwd.txt`. (**3 points**)
4. Supprimez le dossier `Projets/Web` (qui est vide). (**3 points**)
5. Déplacez le fichier `copie_fstab` (qui est dans `Sources`) vers le dossier `EXAMEN_1`. (**3 points**) 


### SECTION C : Édition de texte (VI/VIM) (15 points)

1. Vous utilisez **VI** (ou VIM) pour créer un nouveau fichier nommé `reponses.txt` à l'intérieur du dossier `EXAMEN_1`.
   * Écrire la commande nécessaire.
2. Ce fichier doit contenir exactement les informations suivantes (sur 3 lignes). Écrire les commandes :
   * Ligne 1 : Votre Nom et Prénom
   * Ligne 2 : Le code du cours (420-ZG4-MO)
   * Ligne 3 : "J'aime VI"
3. Sauvegardez et quittez l'éditeur proprement. Quelle commande faites-vous ?
4. Vous vous rendez compte que vous avez oublié d'écrire le numéro de votre groupe dans le fichier. Quelles commandes faites-vous pour pouvoir l'ajouter sur une 4e ligne ?
   * Ligne 4 : Le numéro du groupe


### SECTION D : Question générale (Recherche) (5 points)

* Votre collège André Mathieu doit faire des tests pour son patron. Il a tapé la commande ci-dessous sur son terminal Linux (Mint) :
```bash
$ mkdir /root/mes_projets
```
* Cependant, ça ne veut pas fonctionner et pire encore, il a un message qui dit `"Permission denied"`
1. Expliquez pourquoi. (**3 points**)
2. Que pourriez-vous suggerer à André pour qu'il puisse faire ces tests ? (**2 points**)

---

## GRILLE DE CORRECTION (Pour le prof)

**Total : /50 points (ramené à 20%)**

* **Structure Dossiers (15 pts) :**
* Dossier `EXAMEN_1` présent : 2 pts
* Sous-dossiers `Sources`, `Backup`, `Projets` : 6 pts (2 ch.)
* Sous-dossier `Web` absent (car supprimé en C.4) : 2 pts


* **Fichiers (15 pts) :**
* `fstab` présent dans `EXAMEN_1` (déplacé de Sources) : 5 pts
* `Sources` est vide : 2 pts
* `users_save.txt` présent dans `Backup` : 5 pts
* `passwd` absent de `Backup` (car renommé) : 3 pts


* **VI (15 pts) :**
* Fichier `reponses.txt` existe : 2 pts
* Contenu correct (3 lignes) : 5 pts
* Pas de fichier `.swp` (signe d'un crash VI mal fermé) : 3 pts

* **Générale (5 pts) :**
* Mention du rôle (pouvoirs) : 3 pts
* Suggestion sécuritaire (ex: faire les tests sur une copie) : 2 pts

-->


<!--
---

## EXAMEN VERSION MOODLE

Voici une adaptation de l'examen pratique au format **Test Moodle**, en utilisant exclusivement les types de questions **Réponse courte** (auto-correction stricte sur la syntaxe) et **Composition** (correction manuelle, permet de coller des blocs de texte ou d'expliquer).

Ce format est idéal pour valider les compétences techniques sans avoir à passer voir chaque écran individuellement.

---

# Test Moodle : Examen 1 - Bases de Linux (420 ZG4 MO)

**Consignes pour l'étudiant :**

> * Ouvrez votre Machine Virtuelle Linux Mint.
> * Ouvrez un Terminal.
> * Réalisez les actions demandées sur votre VM.
> * Une fois l'action réussie, reportez la commande exacte ou le résultat demandé dans les champs ci-dessous.
> 
> 

---

### SECTION A : Environnement

**Question 1**

* **Type :** Réponse courte
* **Énoncé :** Vous êtes connecté dans votre terminal. Quelle commande exacte permet d'afficher le nom de l'utilisateur actuel pour confirmer que vous êtes bien connecté sous le compte `etudiant` ?
* **Réponse attendue :** `whoami`

**Question 2**

* **Type :** Réponse courte
* **Énoncé :** Quelle commande permet d'afficher le chemin absolu du dossier dans lequel vous vous trouvez actuellement ?
* **Réponse attendue :** `pwd`

---

### SECTION B : Arborescence et Navigation

**Question 3**

* **Type :** Réponse courte
* **Énoncé :** Vous êtes dans votre dossier personnel (`~`). Écrivez la commande **unique** qui permet de créer le dossier `EXAMEN_1` et le sous-dossier `Projets` d'un seul coup (sans faire deux commandes séparées).
* **Réponse attendue :** `mkdir -p EXAMEN_1/Projets` (Accepter aussi : `mkdir -p ~/EXAMEN_1/Projets`)

**Question 4**

* **Type :** Composition (Texte)
* **Énoncé :** Réalisez la structure complète demandée :
* `EXAMEN_1/`
* `Sources/`
* `Backup/`
* `Projets/` (avec un sous-dossier `Web`)
Une fois créée, lancez la commande `ls -R ~/EXAMEN_1` dans votre terminal. **Copiez et collez le résultat complet de l'affichage ci-dessous.**

---

### SECTION C : Manipulation de Fichiers

**Question 5**

* **Type :** Réponse courte
* **Énoncé :** Quelle commande permet de copier le fichier `/etc/fstab` vers le dossier `~/EXAMEN_1/Sources` en conservant le nom original ?
* **Réponse attendue :** `cp /etc/fstab ~/EXAMEN_1/Sources/` (Accepter avec ou sans le slash final).

**Question 6**

* **Type :** Réponse courte
* **Énoncé :** Vous avez copié `/etc/passwd` dans le dossier `Backup`. Vous devez maintenant le renommer en `users_save.txt`. Si vous êtes déjà positionné dans le dossier `Backup`, quelle commande tapez-vous ?
* **Réponse attendue :** `mv passwd users_save.txt`

**Question 7**

* **Type :** Réponse courte
* **Énoncé :** Quelle commande permet de supprimer le dossier `Web` (qui se trouve dans `Projets`) et tout son contenu, sans demander de confirmation ?
* **Réponse attendue :** `rm -rf Web` (Accepter `rm -r Web` ou le chemin complet).

---

### SECTION D : Édition de Texte (VI/VIM)

**Question 8**

* **Type :** Composition (Texte)
* **Énoncé :** Cette question valide votre compréhension du fonctionnement de l'éditeur VI.
Décrivez, étape par étape, la **séquence de touches exacte** que vous devez presser pour réaliser les actions suivantes :
1. Ouvrir un fichier nommé `reponses.txt`.
2. Passer en mode insertion.
3. Écrire "Bonjour".
4. Sauvegarder et quitter.
*(Exemple de format de réponse : "Je tape la commande X, puis j'appuie sur la touche Y...")*


*Correction suggérée pour le prof : L'étudiant doit mentionner explicitement `vi reponses.txt`, la touche `i`, la touche `Esc` (Échap) et `:wq`.*

---

### SECTION E : Bonus

**Question 9**

* **Type :** Réponse courte
* **Énoncé :** Quelle commande permet de lister le contenu du dossier `/etc` et d'envoyer (rediriger) le résultat directement dans un fichier texte nommé `liste_etc.txt` au lieu de l'afficher à l'écran ?
* **Réponse attendue :** `ls /etc > liste_etc.txt`

---

### 💡 Conseil pour la configuration Moodle

Pour les questions de type **Réponse courte**, assurez-vous de configurer le **caractère joker (wildcard)** `*` dans Moodle si vous voulez être tolérant.

* *Exemple pour la question 5 :*
* Réponse 1 (100%) : `cp /etc/fstab ~/EXAMEN_1/Sources/`
* Réponse 2 (100%) : `cp /etc/fstab ~/EXAMEN_1/Sources`
* Réponse 3 (100%) : `cp /etc/fstab EXAMEN_1/Sources` (Si chemin relatif)

---

Dans Moodle, pour une question de type **Réponse courte** (Short Answer), le caractère joker `*` est activé par défaut. Vous n'avez pas besoin de cocher une option spéciale dans les paramètres globaux; vous devez simplement l'insérer intelligemment dans vos champs **Réponse**.

Voici comment procéder concrètement lors de la création de votre question :

### 1. Où placer le joker ?

Dans la section **Réponses** de votre question, Moodle interprète le symbole `*` comme "n'importe quel nombre de caractères (ou aucun)".

* **`*`** = Remplace tout ce qui précède ou suit.

### 2. Scénarios concrets pour votre examen Linux

Voici comment configurer vos réponses pour tolérer les variations des étudiants (espaces, slashs finaux, chemins relatifs) tout en validant la commande.

#### Cas A : Tolérer le "slash" final ou son absence

Pour la copie d'un dossier, l'étudiant peut écrire `~/Dossier` ou `~/Dossier/`.

* **Dans le champ Réponse 1 :** `cp /etc/fstab ~/EXAMEN_1/Sources*`
* **Résultat :** Moodle acceptera `Sources` et `Sources/`.

#### Cas B : Tolérer les espaces multiples ou les options facultatives

Si vous voulez accepter `rm -rf` ou `rm -r` ou `rm -r -f`.

* **Dans le champ Réponse 1 :** `rm -r* Web`
* **Résultat :** Moodle acceptera tout ce qui commence par `rm -r` et finit par `Web`.
* *Attention :* Cela accepterait aussi `rm -rIMPROBABLE Web`, mais c'est un risque calculé acceptable pour une correction automatique simple.

#### Cas C : Tolérer chemin relatif OU absolu

Pour la création de dossier, l'étudiant peut taper `mkdir Dossier` ou `mkdir ./Dossier` ou `mkdir /home/etudiant/Dossier`.

* **Dans le champ Réponse 1 :** `mkdir *EXAMEN_1/Projets`
* **Résultat :** Tant que la commande finit par le bon chemin, elle est acceptée.

### 3. Le paramètre crucial : La "Sensibilité à la casse"

Juste au-dessus des champs de réponse, vous avez l'option **Sensibilité à la casse**.

* **Pour Linux, choisissez :** *"La casse doit être respectée"* (Yes, case must match).
* *Pourquoi ?* Parce que `Desktop` n'est pas `desktop`. Si un étudiant écrit `CP` au lieu de `cp`, la commande échouera dans la vraie vie, donc elle doit être fausse dans Moodle.
* Le joker `*` fonctionne même si la casse est activée (il remplacera n'importe quel caractère, majuscule ou minuscule, mais les parties fixes de votre réponse devront respecter la casse).



### Résumé de la configuration idéale pour une question :

1. **Type de question :** Réponse courte.
2. **Sensibilité à la casse :** "La casse doit être respectée".
3. **Réponse 1 :** `cp /etc/fstab *EXAMEN_1/Sources*`
4. **Note :** 100%.

Cela valide la commande (`cp`), le fichier source (`/etc/fstab`) et la destination, tout en étant flexible sur le début du chemin destination (`~` ou `/home/...`) et la fin (`/` ou rien).

-->
