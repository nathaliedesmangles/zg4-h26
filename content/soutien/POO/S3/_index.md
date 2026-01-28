+++
title = "L'Orienté Objet"
weight = 1
draft = true
+++


**Objectif** : 
* Clarifier les fondements de l'Orienté Objet.
* Comprendre pourquoi on découpe le code en objets.


> Sondage rapide (à main levée) : Qui est à l'aise avec la différence entre une classe et un objet ?


---


## 1. Accueil et mise en contexte (5 min)

* Mot de bienvenue et présentation de l'objectif : "Comprendre *pourquoi* on découpe le code en objets."
* Sondage rapide (à main levée) : Qui est à l'aise avec la différence entre une classe et un objet ?

## 2. L'approche OO et l'encapsulation (20 min)

* **Concept Clé :** Analogie du **Mouleb (Classe)** vs **Gâteau (Objet/Instance)** ou du **Plan d'architecte** vs **Maison**.
* *Classe :* Le modèle abstrait.
* *Objet/Instance :* La réalisation concrète en mémoire.


* **Encapsulation :**
* Image de la "boîte noire" ou de la "capsule".
* Pourquoi ? Protéger les données (champs privés) pour éviter qu'elles ne soient modifiées n'importe comment de l'extérieur.
* *Exemple visuel :* Une voiture (Objet). On ne modifie pas l'injection du moteur directement (privé), on appuie sur la pédale d'accélération (méthode publique).


## 3. Anatomie d'une classe (20 min)

Décortiquer le code d'une classe standard (ex: `CompteBancaire` ou `Etudiant`).

* **Champs (Attributs) :** L'état de l'objet (ce qu'il *est* ou *a*). Importance de la visibilité `private`.
* **Constructeurs :** Le moment de la naissance de l'objet. Initialiser l'état pour éviter les objets "vides" ou invalides.
* **Accesseurs/Mutateurs (Getters/Setters) :**
* Les gardiens de la porte.
* *Mutateur :* Permet d'ajouter de la validation (ex: interdire un âge négatif).
* *Accesseur :* Permet de lire une valeur sans la modifier.


#### 4. Période de pratique (30 min)

Les étudiants travaillent sur leur portable ou sur papier. L'objectif est de coder une structure simple mais complète.

---


C'est une excellente idée pour se concentrer à 100% sur la mécanique **Classe vs Objet** et l'**Encapsulation**, sans introduire de confusion avec le concept de méthode "qui flotte" (static).

Voici la version modifiée. J'ai déplacé la logique des taxes *à l'intérieur* de l'objet `Produit` (puisque c'est une donnée qui le concerne), ce qui renforce le principe de responsabilité de l'objet.

---

# Atelier Pratique : Gestion d'inventaire (30 mins)

**Objectif :** Créer une classe solide avec des attributs privés et des comportements (méthodes) qui manipulent ces données.

---

### Partie 1 : La classe "Produit"

Créez une classe nommée `Produit`.
Imaginez que cette classe est un "expert" qui connait tout sur un article et sait faire ses propres calculs.

1. **Champs (Encapsulation)** :
* Déclarez trois attributs **privés** :
* `nom` (texte)
* `prix` (nombre décimal)
* `quantite` (entier)


2. **Constructeur** :
* Créez un constructeur qui accepte le `nom` et le `prix` en paramètres.
* Initialisez la `quantite` à **0** par défaut.


3. **Accesseurs et Mutateurs (Getters/Setters)** :
* Créez les méthodes `get` pour les trois champs.
* Créez une méthode `setPrix(double nouveauPrix)` :
* *Validation :* Si le prix reçu est négatif, affichez une erreur. Sinon, mettez à jour l'attribut.


4. **Méthodes de comportement (La logique métier)** :
* **Ajouter du stock :** Créez une méthode `ajouterStock(int qte)` qui ajoute la quantité reçue à la quantité actuelle.
* **Calculer le prix final :** Créez une méthode `obtenirPrixAvecTaxes()`.
* Elle ne prend **aucun paramètre** (car l'objet connait déjà son propre prix !).
* Elle retourne le prix multiplié par 1.14975 (Taxes QC).


---

### Partie 2 : Le programme principal (Le Main)

Dans votre classe principale (Main), testez votre "moule" à objets :

1. **Instanciation :** Créez deux objets différents :
* `p1` : "Casque Audio" à 80.00 $    * `p2` : "Souris sans fil" à 25.00$


2. **Manipulation :**
* Ajoutez 5 unités au stock du `p1`.
* Ajoutez 10 unités au stock du `p2`.
* Changez le prix de `p2` pour 20.00 $ (Promotion !).


3. **Affichage :**
* Affichez le nom et le prix avec taxes de chaque produit.
* *Exemple de sortie attendue :* `"Le produit Casque Audio coûte 91.98 $ taxes incluses."`


---

### Défi + (Pour les rapides)

Ajoutez une méthode intelligente pour gérer la valeur de l'inventaire.

1. Dans la classe `Produit`, ajoutez une méthode `obtenirValeurTotaleStock()`.
* Elle doit retourner : `prix` * `quantite`.


2. Dans le `Main`, affichez la valeur totale de tout votre inventaire (Valeur de p1 + Valeur de p2).

---

### Solution (Guide enseignant)

Voici à quoi ressemble la logique "Tout dans l'objet" :

**Classe Produit :**

```java
public class Produit {
    private String nom;
    private double prix;
    private int quantite;

    public Produit(String nom, double prix) {
        this.nom = nom;
        this.prix = prix;
        this.quantite = 0;
    }

    public void setPrix(double prix) {
        if (prix >= 0) {
            this.prix = prix;
        } else {
            System.out.println("Erreur : Prix invalide");
        }
    }

    public void ajouterStock(int qte) {
        if (qte > 0) this.quantite += qte;
    }

    // Méthode d'instance : utilise 'this.prix'
    public double obtenirPrixAvecTaxes() {
        return this.prix * 1.14975;
    }
    
    // Méthode Défi +
    public double obtenirValeurTotaleStock() {
        return this.prix * this.quantite;
    }

    // Getters standards...
    public String getNom() { return nom; }
    public double getPrix() { return prix; }
    public int getQuantite() { return quantite; }
}

```

**Main :**

```java
public class Main {
    public static void main(String[] args) {
        Produit p1 = new Produit("Casque", 80.0);
        p1.ajouterStock(5);
        
        // Appel sur l'instance (p1.) et non sur la classe
        System.out.println("Prix TTC : " + p1.obtenirPrixAvecTaxes());
        
        // Défi
        System.out.println("Valeur du stock p1 : " + p1.obtenirValeurTotaleStock());
    }
}

```

### 📝 Détail de l'activité pratique (30 min)

**Scénario : Gestion d'un Inventaire de Produits**

**Étape 1 : La Classe Modèle (15-20 min)**
Demander aux étudiants de créer une classe `Produit` respectant l'encapsulation :

1. **Champs privés :** `nom` (string), `prix` (double), `quantite` (int).
2. **Constructeur :** Qui force à donner un nom et un prix à la création. La quantité commence à 0 par défaut.
3. **Accesseurs/Mutateurs :**
* `getPrix()`
* `setPrix(double nouveauPrix)` : Doit refuser un prix négatif (validation).
* `ajouterStock(int qte)` : Une méthode métier plutôt qu'un simple "setQuantite".



**Étape 2 : La Classe Utilitaire (10 min)**
Créer une classe `OutilsTaxes` (ou `FinanceUtils`) :

1. Une méthode `static` : `calculerPrixTTC(double prixHorsTaxe)`.
2. Faire remarquer qu'on n'a pas besoin de faire `new OutilsTaxes()` pour l'utiliser.

**Étape 3 : Le "Main" (Test)**
Dans le programme principal :

1. Instancier 2 produits différents.
2. Modifier leur prix (test de la validation négative).
3. Utiliser la méthode statique pour afficher le prix avec taxes d'un des produits.

---

### 💡 Conseils pour l'animation

* **Live Coding :** Si possible, codez l'exemple du "Module 2" en direct au projecteur, en faisant des erreurs volontaires (ex: mettre un champ `public`) et en demandant aux étudiants pourquoi c'est risqué.
* **Tableau comparatif :** Dessinez un tableau avec deux colonnes au tableau blanc : "Plan (Classe)" vs "Réalité (Instance)" et "Méthode d'objet" vs "Méthode de classe (Static)".

