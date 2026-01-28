+++
title = "Aide-Mémoire: Javadoc, JUnit & git"
weight = 2
draft = true
+++



## 1. JAVADOC (Documenter)

*Le manuel d'instruction de votre code.*

Contrairement aux commentaires simples (`//`) qui sont pour le développeur, la Javadoc génère une documentation HTML pour ceux qui **utilisent** votre classe.

**La Syntaxe :**
Tapez `/**` puis `Entrée` juste au-dessus d'une méthode ou d'une classe.

```java
/**
 * Calcule le prix total avec les taxes.
 * * @param prixHT  Le prix hors taxes (doit être positif).
 * @return Le prix total incluant la TPS et TVQ.
 */
public double calculerTotal(double prixHT) { ... }
```

| Tag | Usage |
| --- | --- |
| **`@param`** | Décrit un paramètre d'entrée (ce que la méthode mange). |
| **`@return`** | Décrit ce que la méthode renvoie (ce qui ressort). |
| **`@throws`** | (Optionnel) Indique si la méthode peut planter (Exception). |
| **`@author`** | (Sur la classe) Votre nom. |

---

## 2. JUNIT (Tester)

*L'assurance qualité automatisée.*

Ne testez plus avec des `System.out.println` dans le `main` ! Créez une classe de test dédiée (ex: `CalculatriceTest`).

**Structure de base :**

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class MaClasseTest {

    @Test
    public void testCalculerTotal() {
        // 1. Préparer (Arrange)
        double entree = 100.0;
        double attendu = 114.975; 

        // 2. Exécuter (Act)
        double resultat = MaClasse.calculerTotal(entree);

        // 3. Vérifier (Assert)
        assertEquals(attendu, resultat, 0.01); // 0.01 est la marge d'erreur (delta) pour les double
    }
}
```

**Les Assertions :**

| Méthode | Ce qu'elle vérifie |
| --- | --- |
| `assertEquals(A, B)` | Vérifie que A est égal à B. |
| `assertTrue(condition)` | Vérifie que la condition est VRAIE. |
| `assertFalse(condition)` | Vérifie que la condition est FAUSSE. |
| `assertNull(objet)` | Vérifie que l'objet est `null`. |
| `assertNotNull(objet)` | Vérifie que l'objet existe. |

---

## 3. GIT (Sauvegarder & partager)

*La machine à voyager dans le temps.*

**Le mantra du succès :** `Status` -> `Add` -> `Commit` -> `Push`

| Commande | Action (analogie) |
| --- | --- |
| `git status` | **GPS** : "Où suis-je ? Qu'est-ce qui a changé ?" (À faire tout le temps !) |
| `git add .` | **Le carton** : Je mets *tous* mes fichiers modifiés dans le carton d'expédition. |
| `git commit -m "Message"` | **La photo** : Je scelle le carton et je prends une photo. Le message doit être un verbe d'action (ex: *"Ajoute la validation du prix"*). |
| `git push` | **L'envoi** : J'envoie mes cartons vers le serveur (GitLab/GitHub). |
| `git pull` | **Réception** : Je télécharge le travail de mes collègues. (À faire en début de séance !) |

> [!secondary]
> **Règle d'or :** Un commit = Une tâche terminée. Ne committez pas du code qui ne compile pas !