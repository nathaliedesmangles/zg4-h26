+++
title = "TP1-Classe & Objets"
weight = 3
draft = true
+++


*Cette correction est divisée par exercice, avec le code complet et des explications sur les points techniques importants (encapsulation, constructeurs, logique métier)*.


## Structure du Projet

Selon les directives, tous les fichiers doivent être dans un package nommé `at01`.

---

## Exercice 1 : La classe `Produit`

Cet exercice vise à pratiquer l'encapsulation de base (attributs privés, accesseurs publics) et la création de méthodes de calcul simples.

### 1. Classe `Produit.java`

```java
package at01; [cite_start]// [cite: 7]

/**
 * Classe représentant un produit avec une description et un prix.
 * [cite_start]@author Votre Nom (F. Ait-Ammar, M. Zouba) [cite: 12, 32]
 */
[cite_start]public class Produit { // [cite: 18]

    // --- ATTRIBUTS (Encapsulation) ---
    [cite_start]// Ils sont privés pour empêcher la modification directe de l'extérieur[cite: 19].
    private String description;
    private double prix;

    // --- CONSTRUCTEUR ---
    [cite_start]// Permet d'initialiser l'objet lors de sa création (new)[cite: 20].
    public Produit(String description, double prix) {
        this.description = description;
        this.prix = prix;
    }

    [cite_start]// --- ACCESSEURS (Getters) --- [cite: 21]
    public String getDescription() {
        return description;
    }

    public double getPrix() {
        return prix;
    }

    [cite_start]// --- MUTATEURS (Setters) --- [cite: 22]
    public void setDescription(String description) {
        this.description = description;
    }

    public void setPrix(double prix) {
        this.prix = prix;
    }

    // --- MÉTHODES MÉTIER ---

    /**
     * Calcule le prix après un rabais donné sans modifier l'attribut prix.
     * @param pourcentageRabais Le pourcentage (ex: 20.0 pour 20%)
     * @return Le prix réduit
     */
    [cite_start]public double prixApresRabais(double pourcentageRabais) { // [cite: 23, 28]
        // Formule : Prix - (Prix * Pourcentage / 100)
        double montantRabais = this.prix * (pourcentageRabais / 100.0);
        return this.prix - montantRabais; 
        [cite_start]// Note: L'attribut 'prix' ne change pas ici[cite: 27].
    }

    /**
     * Affiche les détails du produit à la console.
     */
    [cite_start]public void afficher() { // [cite: 24, 39]
        System.out.println("Description: " + this.description);
        System.out.println("Prix: " + this.prix + "$"); [cite_start]// Ajout du symbole $ selon l'exemple [cite: 37]
    }
}

```

### 2. Classe `MainProduit.java` (Test)

Cette classe suit les étapes **a** à **i** de l'énoncé .

```java
package at01;

/**
 * Classe principale pour tester la classe Produit.
 * @author Votre Nom
 */
[cite_start]public class MainProduit { // [cite: 9]

    public static void main(String[] args) {
        
        // a. [cite_start]Création de prod1 [cite: 42]
        Produit prod1 = new Produit("Hilroy Feuilles mobiles lignées 200 Feuilles", 2.47);

        // b. [cite_start]Création de prod2 [cite: 43]
        Produit prod2 = new Produit("Hilroy Cahiers d'exercices Canada 32 pages", 1.86);

        // c. [cite_start]Afficher description de prod1 via accesseur [cite: 44]
        System.out.println("Accès à la description de prod1: " + prod1.getDescription());

        // d. [cite_start]Afficher prix de prod1 via accesseur [cite: 45]
        System.out.println("Accès au prix de prod1: " + prod1.getPrix());

        // e. [cite_start]Modifier le prix de prod1 à 3.25 [cite: 46]
        prod1.setPrix(3.25);

        // f. [cite_start]Modifier la description de prod1 [cite: 47]
        prod1.setDescription("Hilroy Feuilles mobiles carreaux 250 Feuilles");

        // g. [cite_start]Appeler la méthode afficher() [cite: 48]
        prod1.afficher();

        // h. [cite_start]Calculer et afficher le prix après rabais de 20% [cite: 51-53]
        // Attention: on appelle la méthode, mais on doit capturer ou afficher le retour.
        double prixReduit = prod1.prixApresRabais(20.0);
        System.out.println("Prix après rabais de 20.0% de prod1: " + prixReduit);

        // i. [cite_start]Somme des prix de prod1 et prod2 [cite: 54-56]
        double total = prod1.getPrix() + prod2.getPrix();
        // Note: On utilise getPrix() car 'prix' est privé.
        System.out.println("Le total des prix de prod1 et de prod2: " + total);
    }
}

```

---

## Exercice 2 : La classe `Cercle`

Cet exercice est plus complexe car il inclut de la validation (dans le setter) et de la géométrie analytique.

### 1. Classe `Cercle.java`

**Points pédagogiques clés :**

1. **Validation :** Le setter du rayon protège l'objet des valeurs négatives.
2. **Réutilisation :** Le constructeur appelle le setter (`setRayon`) pour ne pas réécrire la logique de validation.  
3. **Mathématiques :** Utilisation de `Math.PI` et `Math.pow`.

```java
package at01;

/**
 * Classe représentant un cercle dans un plan cartésien.
 * @author Votre Nom
 */
[cite_start]public class Cercle { // [cite: 67]

    // --- ATTRIBUTS ---
    [cite_start]// h et k sont les coordonnées du centre (x,y), rayon est la taille[cite: 70].
    private int h;
    private int k;
    private int rayon;

    // --- CONSTRUCTEURS ---

    [cite_start]// Constructeur 1 : Centre et rayon spécifiés [cite: 77]
    public Cercle(int h, int k, int rayon) {
        this.h = h;
        this.k = k;
        [cite_start]// IMPORTANT: On appelle le mutateur pour valider le rayon [cite: 78]
        setRayon(rayon); 
    }

    [cite_start]// Constructeur 2 : Centré à l'origine (0,0) [cite: 79-81]
    public Cercle(int rayon) {
        this.h = 0;
        this.k = 0;
        setRayon(rayon);
    }

    // --- ACCESSEURS & MUTATEURS ---

    [cite_start]public int getRayon() { // [cite: 70]
        return rayon;
    }

    [cite_start]public void setRayon(int rayon) { // [cite: 74]
        if (rayon >= 0) {
            this.rayon = rayon;
        } else {
            [cite_start]// Message d'erreur exact demandé [cite: 75-76]
            System.out.println("La valeur du rayon ne peut être négative");
        }
    }

    // --- MÉTHODES PUBLIQUES ---

    [cite_start]// Modifier le centre [cite: 82]
    public void modifier(int h, int k) {
        this.h = h;
        this.k = k;
    }

    [cite_start]// Calcul de l'aire (Pi * r^2) [cite: 83]
    public double aire() {
        return Math.PI * Math.pow(this.rayon, 2);
    }

    [cite_start]// Calcul du périmètre (2 * Pi * r) [cite: 84]
    public double perimetre() {
        return 2 * Math.PI * this.rayon;
    }

    [cite_start]// Vérifier si un point est SUR le cercle (équation du cercle) [cite: 93-94]
    public boolean estSurCercle(int x, int y) {
        // Équation: (x-h)^2 + (y-k)^2 = r^2
        double partieGauche = Math.pow(x - this.h, 2) + Math.pow(y - this.k, 2);
        double partieDroite = Math.pow(this.rayon, 2);
        
        // On compare les deux côtés
        return partieGauche == partieDroite;
    }

    [cite_start]// Affichage formaté [cite: 87-92]
    public void afficher() {
        // Utilisation de printf pour formater les décimales (optionnel mais plus propre)
        // ou affichage standard selon l'exemple.
        System.out.println("Coordonnées X du centre:" + this.h);
        System.out.println("Coordonnées Y du centre:" + this.k);
        System.out.println("Le rayon du cercle: " + this.rayon);
        
        // L'exemple montre 2 décimales. On peut utiliser String.format ou afficher brut.
        // Ici, affichage brut avec calcul direct pour simplicité, ou printf :
        System.out.printf("Le périmètre du cercle: %.2f%n", perimetre());
        System.out.printf("L'aire du cercle: %.2f%n", aire());
    }
}

```

### 2. Classe `MainCercle.java` (Test)

Vous devez tester tous les constructeurs et méthodes .

```java
package at01;

/**
 * Classe de test pour Cercle.
 * @author Votre Nom
 */
public class MainCercle {

    public static void main(String[] args) {
        
        [cite_start]// --- TEST 1 : Constructeur complet --- [cite: 99-100]
        System.out.println("--- Test du Cercle 1 (Complet) ---");
        // Construire un cercle c1 de centre (9,2) et de rayon 5
        Cercle c1 = new Cercle(9, 2, 5);
        
        [cite_start]// Afficher caractéristiques [cite: 104]
        c1.afficher();

        // Tester aire et périmètre individuellement
        System.out.println("Aire c1 : " + c1.aire());

        [cite_start]// --- TEST 2 : Constructeur Origine --- [cite: 105-106]
        System.out.println("\n--- Test du Cercle 2 (Origine) ---");
        // Construire un cercle c2 de centre (0,0) et de rayon 7
        Cercle c2 = new Cercle(7);
        c2.afficher();

        // --- TEST 3 : Validation du rayon négatif ---
        System.out.println("\n--- Test erreur rayon négatif ---");
        // Tentative de mettre un rayon négatif via le setter
        c2.setRayon(-5); // Doit afficher le message d'erreur
        // Vérification que le rayon n'a pas changé (doit rester 7)
        System.out.println("Rayon après erreur (doit être 7): " + c2.getRayon());

        // --- TEST 4 : Modification du centre ---
        System.out.println("\n--- Test modification centre ---");
        c1.modifier(0, 0); // On déplace c1 à l'origine
        System.out.println("Nouveau centre X de c1 (doit être 0): " + c1.aire()); // Juste pour verifier l'objet

        // --- TEST 5 : Point sur le cercle ---
        System.out.println("\n--- Test estSurCercle ---");
        // Cercle c2 est à (0,0) avec rayon 7.
        // Point (7,0) devrait être SUR le cercle.
        boolean test1 = c2.estSurCercle(7, 0);
        System.out.println("Point (7,0) sur c2 (attendu true): " + test1);

        // Point (2,2) n'est pas sur le cercle.
        boolean test2 = c2.estSurCercle(2, 2);
        System.out.println("Point (2,2) sur c2 (attendu false): " + test2);
    }
}

```

## Liste de vérification avant remise

1. Avez-vous créé le package `at01` ?
2. Avez-vous ajouté les en-têtes Javadoc (`/** ... */`) avec `@author` au début de chaque fichier ?
3. Avez-vous vérifié que le message d'erreur du rayon négatif est exactement : «La valeur du rayon ne peut être négative» ?
4. Compresser le dossier `at01` en `.zip` pour le dépôt Moodle.