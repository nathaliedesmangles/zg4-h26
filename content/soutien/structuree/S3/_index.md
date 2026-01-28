+++
title = "Javadoc, JUnit & git"
weight = 1
draft = true
+++


**Objectif** : Comprendre et utiliser les outils de documentation du code, de tests unitaires et de versionnage.

---


## 1. Introduction : Le workflow (5 min)

* Explication du cercle vertueux :
1. J'écris du code que les autres comprennent (**Javadoc**).
2. Je prouve que mon code fonctionne sans relancer le `main` 50 fois (**JUnit**).
3. Je sauvegarde des étapes logiques pour pouvoir revenir en arrière (**Git**).


## 2. Module 1 : Javadoc – Laisser une trace (10 min)

* **Le concept :** Différence entre commentaire pour le dév (`//`) et documentation pour l'utilisateur (`/**`).
* **La syntaxe :** Démo en direct.
* Se placer au-dessus d'une méthode.
* Taper `/**` + `Entrée` (Montrer l'autogénération de l'IDE).


* **Les Tags à maîtriser :**
* `@param` : Expliquer ce qu'on attend (ex: "Le prix doit être positif").
* `@return` : Expliquer ce qui ressort.


* **L'effet "Wow" :** Générer la vue HTML (ou passer la souris sur la méthode pour voir le popup jaune/blanc apparaître).

## 3. Module 2 : JUnit – L'assurance qualité (20 min)

* **Problème :** "Qui a déjà effacé son `main` pour tester une autre méthode et a tout perdu ?" -> JUnit est la solution.
* **Anatomie d'un test :**
* La classe de test (souvent `MaClasseTest`).
* L'annotation `@Test`.


* **Les Assertions (Le juge) :**
* `assertEquals(attendu, resultat)` : Le standard.
* `assertTrue(condition)` / `assertFalse(condition)`.


* **Exercice guidé (Live Coding) :**
* Coder une méthode simple `estPair(int n)`.
* Coder le test : `assertTrue(estPair(4))` et `assertFalse(estPair(5))`.
* Voir la barre **VERTE**.
* Saboter le code pour voir la barre **ROUGE**.



## 4. Module 3 : Git (20 min)

* **Les 3 zones (Schéma au tableau indispensable) :**
1. **Working Directory :** L'établi (bordélique).
2. **Staging Area (Index) :** Le carton de déménagement (on choisit ce qu'on met dedans).
3. **Repository (Commit) :** Le camion scellé (photo indélébile).


* **Le "Mantra" des commandes :**
1. `git status` (Toujours commencer par là !).
2. `git add .` (Tout mettre dans le carton) ou `git add Fichier.java`.
3. `git commit -m "Verbe d'action..."` (Sceller le carton).
4. `git push` (Envoyer au dépôt distant/Labo).


* **Bonne pratique :** Un commit = Une tâche finie (ex: "Ajout de la Javadoc" ou "Correction du bug #12").

## 5. Conclusion & questions (5 min)

* Résumé : "On ne commit pas un code qui ne passe pas les tests JUnit !"
* Distribution de l'aide-mémoire.

---

## Exemple concret pour la séance (Fil rouge)

Utilisez une classe simple `UtilitaireMotDePasse` pour illustrer les 3 points.

1. **Code :** Une méthode `verifierForce(String mdp)` qui retourne `true` si longueur > 8.
2. **Javadoc :** Documenter que `@param mdp` ne doit pas être null.
3. **JUnit :**
* `testMdpCourt()` -> assert `false`.
* `testMdpLong()` -> assert `true`.


4. **Git :**
* Commit 1 : "Création de la classe vide".
* Commit 2 : "Implémentation de la logique et des tests".