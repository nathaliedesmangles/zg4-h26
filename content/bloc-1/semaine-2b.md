+++
pre = 'Labo #2 : '
title = 'Les commandes de base'
weight = 22
draft = true
+++


## Objectif

* Développer la "mémoire musculaire" des commandes de base.


Utiliser le fichier **labo2.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo2.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}


### Exercice #1 : Trouver la bonne commande

À l’aide de **`apropos`**, trouve une commande qui permet :

1. D’afficher l’heure.
2. De changer les permissions.
3. D’arrêter le système.
4. De rechercher du texte dans un fichier.
5. De compter les lignes d’un fichier.

> **Écris la commande trouvée + sa courte description.**


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

---


## Corrigé du laboratoire

> À venir (samedi ou dimanche)


