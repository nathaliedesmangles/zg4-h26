+++
pre = 'Semaine 3 : '
title = "Utilisateurs, groupes & droits d'accès"
weight = 14
draft = true
+++


## Objectifs de la semaine


1. **Distinguer** les concepts d'UID, GID, groupes primaires et secondaires.
2. **Administrer** les utilisateurs (création, modification, cycle de vie) via le terminal.
3. **Interpréter** et **modifier** les permissions de fichiers et dossiers (rwx, octal, symbolique).
4. **Assigner** la propriété des fichiers (`chown`/`chgrp`) pour sécuriser l'accès.

---

# Théorie

## 1. Introduction & contexte (5 min)

* Linux est un système multi-utilisateurs. Même si vous êtes seul sur votre PC, le système, lui, utilise des dizaines d'utilisateurs fantômes (services) pour fonctionner."

**Analogie :** 
   * Le système est un immeuble sécurisé.
   * **Utilisateurs :** Les résidents (avec un badge d'identité).
   * **Groupes :** Les équipes (Maintenance, Direction, Invités).
   * **Permissions :** Qui a la clé de quelle porte (Salon, Salle des coffres).



## 2. Gestion des utilisateurs et groupes (40 min)

### 2.1 L'identité numérique (10 min)

* **UID & GID :** Le noyau ne voit que des numéros :
   * *Root (0)*  
   * *Système (1-999)*  
   * *Humains (1000+)*  

* **Les fichiers de configuration :**
   * `/etc/passwd` (L'annuaire public).
   * `/etc/shadow` (Le coffre-fort des mots de passe).
   * `/etc/group` (Les clubs/groupes).

=============================================================================
* *Suggestion : Schéma montrant le lien entre passwd (UID) et group (GID).*
=============================================================================


### 2.2 Groupes primaires vs secondaires (10 min)

* **Distinction cruciale :**
   * **Primaire :** L'identité par défaut (création de fichiers).
   * **Secondaire :** Les accès additionnels (sudo, docker, etc.).

* **Commandes clés :**
   * `useradd` (options `-m`, `-s`, `-g`, `-G`).
   * `id` (pour vérifier).
   * `usermod` (le danger de l'option `-G` sans `-a` !).



### 2.3 Exercice pratique guidé (20 min)

**Scénario : "L'arrivée du patron"**

1. **Inspection :** Regarder son propre compte avec `id`.
2. **Création des groupes :** Créer le groupe `direction`.
3. **Création utilisateur :** Créer l'utilisateur `pdg` avec un dossier home et le shell Bash.
   * *Commande :* `sudo useradd -m -s /bin/bash pdg`
4. **Assignation (Le piège) :**
   * Mettre `direction` en groupe **primaire** (`-g`).
   * Ajouter `sudo` en groupe **secondaire** (`-aG`).
5. **Vérification :** `id pdg`.
6. **Nettoyage (Cycle de vie) :** Verrouiller le compte (`passwd -l`) puis le supprimer proprement (`userdel -r`).


=======================================================================================================
## *Pause (5 à 10 min)*

*Essentiel pour laisser le cerveau assimiler la logique UID/GID avant d'attaquer les maths de l'octal.*
=======================================================================================================


## 3. Permissions & droits d'accès (45 min)

### 3.1 Comprendre la commande `ls -l` (10 min)

* **Anatomie :** `drwxr-xr-x`.
* **Les 3 identités :** 
   * U (User/Propriétaire), 
   * G (Group/Collègues), 
   * O (Others/Le reste du monde)
* **Les pouvoirs :** 
   * R: lecture (*read*), 
   * W: écriture (*write*)
   * X: exécution (*eXecution*)

* *Point d'attention majeur :* La différence **Fichier vs dossier**.

> [!warning]
> `x` sur un dossier = droit de traverser/entrer. Sans `x`, on ne peut pas faire `cd` ni accéder aux fichiers dedans.



### 3.2 La notation octale (10 min)

* Le calcul binaire simplifié : R=4, W=2, X=1.
   * R = 1 0 0
   * W = 0 1 0
   * X = 0 0 1


* **Tableau magique :** 
   * 7 (Tout), 
   * 6 (RW), 
   * 5 (RX), 
   * 4 (R), 
   * 0 (Aucun).

=======================================================================================================
* **Quiz rapide (Main levée) :**
* "Je veux lire et exécuter ?" 
   * 4 + 1 -> 5
* "Je veux juste écrire (bizarre mais possible) ?" 
   * -> 2
* "750 ça veut dire quoi ?" (
   * Moi tout, mon groupe lit/exécute, les autres rien
=======================================================================================================


### 3.3 Commandes & pratique : chmod & chown (25 min)


* `chmod` : Change le mode (la serrure).
* `chown` : Change le propriétaire (à qui appartient la maison).

=======================================================================================================
* *Suggestion : Un tableau visuel montrant la conversion symboles <-> octal.*
=======================================================================================================

* **Exercice : "Le rapport confidentiel"**
1. **Préparation :**
```bash
touch rapport_secret.txt
ls -l rapport_secret.txt
```

2. **Test destructif :**
* Enlever tous les droits : `chmod 000 rapport_secret.txt`
* Tenter de lire : `cat rapport_secret.txt` (Doit échouer).

3. **Restauration (Octal) :**
* Remettre le propriétaire en maître absolu : `chmod 700 rapport_secret.txt`.

4. **Collaboration (Symbolique) :**
* Ajouter le droit de lecture au groupe seulement : `chmod g+r rapport_secret.txt`.

5. **Changement de proprio :**
* Donner le fichier à l'utilisateur `root` : `sudo chown root rapport_secret.txt`.
* Tenter de le modifier (Doit échouer, même si on a les droits RW, car on n'est plus le propriétaire).


## 4. Conclusion & synthèse (5 min)

* **Récapitulatif :**
* `useradd` crée, `usermod` modifie.
* Toujours utiliser `-aG` pour ajouter un groupe secondaire.
* `chmod` gère le "comment" (droits), `chown` gère le "qui" (propriétaire).
* Dossiers : Il faut toujours `x` pour entrer dedans.


* **Devoirs / Pour la prochaine fois :**
* Réviser la table octale (4-2-1).



---

# Aide-Mémoire pour l'enseignant (Tableau blanc)

Pendant le cours, il est utile de garder ces deux schémas dessinés dans un coin du tableau :

### 1. Structure de la commande `ls -l`

```text
  d  rwx  r-x  ---   bob   devs   ...
  ^   ^    ^    ^     ^      ^
Type User Grp Other Owner  Group

```

### 2. Calcul octal

| Lettre | Valeur octale | Valeur binaire | Signification |
| --- | --- | --- | --- |
| **R** | **4** | 1 0 0 | Lire |
| **W** | **2** | 0 1 0 | Écrire |
| **X** | **1** | 0 0 1 | Exécuter |
| **-** | **0** | 0 0 0 | Rien |

*Additionnez pour obtenir le score !*

---

# Exercices Supplémentaires (Si le temps le permet)

Si le groupe est rapide, proposez ce défi "Debug" :

> *"J'ai créé un dossier `web` avec les droits `700`. J'ai mis un fichier `index.html` dedans avec les droits `777`. Pourtant, mon collègue n'arrive pas à lire le fichier `index.html`. Pourquoi ?"*
> **Réponse :** Le collègue est bloqué à la porte du dossier (le dossier est 700, donc interdit aux autres). Même si le fichier est ouvert à tous, on ne peut pas l'atteindre.