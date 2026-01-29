+++
title = "Aide-Mémoire: Classe & Objet"
weight = 2
draft = true
+++


## 1. Le concept fondamental : Classe vs Objet

Pour comprendre l'OO, il faut distinguer le **concept** de sa **réalisation**.

| Concept | Analogie | En Code |
| --- | --- | --- |
| **CLASSE** (Le Modèle) | C'est le **plan d'architecte** ou le **moule à biscuits**. Il n'existe pas physiquement, c'est une définition. | `public class Robot { ... }` |
| **OBJET** (L'Instance) | C'est la **maison construite** ou le **biscuit**. Il prend de la place en mémoire. On peut en créer autant qu'on veut. | `Robot r1 = new Robot();` |

> **L'Instanciation :** C'est l'action de créer un objet à partir d'une classe via le mot-clé **`new`**.

---

## 2. Anatomie d'une classe

Ce qu'on trouve à l'intérieur du code de la classe.

### A. Les champs (Attributs)

C'est **l'état** de l'objet (ce qu'il sait).

* **Règle d'or :** Toujours `private` (**Encapsulation**).
* Utilise `this` pour y faire référence.

### B. Le Constructeur

C'est la **naissance** de l'objet.

* Même nom que la classe.
* Pas de type de retour (pas de void, pas de int).
* Sert à **initialiser** les champs (donner des valeurs de départ).

### C. Accesseurs & mutateurs (Getters / Setters)

Les portiers qui contrôlent l'accès aux champs privés.

* **Get (Accesseur) :** Pour lire la valeur. `return this.champ;`
* **Set (Mutateur) :** Pour modifier la valeur. **Permet de valider** les données avant de les enregistrer (ex: empêcher un âge négatif).

---

## 3. L'encapsulation (Le principe de sécurité)

Imaginez une capsule ou une "boîte noire".

* **PUBLIC (+)** : Visible par tout le monde. (Méthodes, constructeurs).
* **PRIVATE (-)** : Visible *uniquement* à l'intérieur de la classe. (Champs).

**Pourquoi ?** Pour empêcher le code extérieur de "casser" votre objet en mettant n'importe quoi dans les variables (ex: `compte.solde = -1000000`). On force le passage par les méthodes (Setters).

---

## 4. Exemple de Code Complet

```java
public class Etudiant {
    // 1. CHAMPS (Privés !)
    private String nom;
    private int note;

    // 2. CONSTRUCTEUR (Initialisation)
    public Etudiant(String nom) {
        this.nom = nom;
        this.note = 0; // Valeur par défaut
    }

    // 3. ACCESSEUR (Lecture)
    public String getNom() {
        return this.nom;
    }

    // 4. MUTATEUR (Écriture avec protection)
    public void setNote(int nouvelleNote) {
        if(nouvelleNote >= 0 && nouvelleNote <= 100) {
            this.note = nouvelleNote;
        } else {
            System.out.println("Erreur : Note invalide !");
        }
    }
}
```
<!--
---

## 5. STATIC vs INSTANCE (La grande confusion)

Le mot-clé `static` change l'appartenance d'une méthode ou d'un champ.

| Type | INSTANCE (Par défaut) | STATIC (Utilitaire) |
| --- | --- | --- |
| **Appartient à...** | L'objet spécifique (le "Moi"). | La Classe (le "Groupe"). |
| **Données** | Chaque objet a sa propre copie. (Ex: Chaque étudiant a *son* nom). | Une seule copie partagée pour tout le monde. (Ex: Le nom du Collège). |
| **Accès aux champs** | Peut utiliser `this.champ`. | **Ne peut pas** utiliser `this` (ignore les données spécifiques). |
| **Comment l'appeler ?** | Via la variable de l'objet.<br>

<br>`monEtudiant.etudier()` | Via le nom de la Classe.<br>

<br>`Math.sqrt(25)` ou `Outils.calculer()` |
| **Quand l'utiliser ?** | Pour tout ce qui décrit l'objet. | Pour des outils universels, des compteurs globaux, ou des constantes. |

### L'Astuce "Mnémotechnique" :

* Si la méthode a besoin de savoir *qui* l'appelle (son nom, son solde, sa couleur) -> **Non-Static**.
* Si la méthode fonctionne de la même manière peu importe qui l'appelle (ex: 2 + 2 = 4) -> **Static**.
-->