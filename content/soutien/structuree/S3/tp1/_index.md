+++
title = "TP1-Javadoc, JUnit & git"
weight = 3
draft = true
+++



### Structure du Projet

Selon les directives, vos fichiers doivent être dans un package :
`package votrenom.tp01;`

### Analyse et correction : `dateValide`

C'est la fondation du programme. Si cette méthode échoue, tout le reste échoue.

**Critères :**
   * Année entre 1800 et 2200 inclus.
   * Mois entre 1 et 12.
   * Jour valide pour ce mois spécifique (attention aux années bissextiles gérées par `joursDansMois`).

**Code corrigé :**

```java
/**
 * Valide si une date est conforme aux limites du système.
 * @param jour Le jour du mois
 * @param mois Le mois (1-12)
 * @param annee L'année (1800-2200)
 * @return true si la date est valide, false sinon
 */
public static boolean dateValide(int jour, int mois, int annee) {
    [cite_start]// 1. Validation de l'année [cite: 191]
    if (annee < 1800 || annee > 2200) {
        return false;
    }

    // 2. Validation du mois
    if (mois < 1 || mois > 12) {
        return false;
    }

    // 3. Validation du jour
    [cite_start]// On doit utiliser joursDansMois pour savoir si c'est 28, 29, 30 ou 31 [cite: 193]
    int maxJours = joursDansMois(mois, annee);
    if (jour < 1 || jour > maxJours) {
        return false;
    }

    return true;
}

```

---

### Les méthodes de navigation "Hier"

Pour reculer une date de X jours, la stratégie la plus simple (et celle suggérée par l'architecture du code fourni) est de reculer **un jour à la fois** dans une boucle. Il nous faut donc savoir quel était le jour, le mois et l'année de la veille.

#### A. `anneeHier`

L'année change seulement si on recule du **1er janvier**.

```java
/**
 * Retourne l'année de la date précédente.
 */
public static int anneeHier(int jour, int mois, int annee) {
    [cite_start]// Toujours vérifier la validité en premier [cite: 205]
    if (!dateValide(jour, mois, annee)) {
        return -1;
    }

    // Si on est le 1er janvier, l'année d'hier est annee - 1
    if (jour == 1 && mois == 1) {
        return annee - 1;
    }

    // Sinon, l'année ne change pas
    return annee;
}

```

#### B. `moisHier`

Le mois change seulement si on est le **1er du mois**.

```java
/**
 * Retourne le mois de la date précédente.
 */
public static int moisHier(int jour, int mois, int annee) {
    if (!dateValide(jour, mois, annee)) {
        return -1;
    }

    if (jour == 1) {
        // Cas spécial : 1er Janvier -> le mois d'avant est Décembre (12)
        if (mois == 1) {
            return 12;
        }
        // Sinon, c'est le mois précédent
        return mois - 1;
    }

    // Si ce n'est pas le 1er du mois, le mois ne change pas
    return mois;
}
```

#### C. `jourHier`

C'est ici que la logique est la plus "complexe".

* Si on est le 10, hier était le 9 (facile).
* Si on est le 1er, hier était le **dernier jour du mois précédent**.

```java
/**
 * Retourne le jour de la date précédente.
 */
public static int jourHier(int jour, int mois, int annee) {
    if (!dateValide(jour, mois, annee)) {
        return -1;
    }

    // Cas simple : Milieu de mois
    if (jour > 1) {
        return jour - 1;
    }

    // Cas complexe : 1er du mois (jour == 1)
    // On doit trouver le dernier jour du mois PRÉCÉDENT.
    
    // 1. On trouve quel était le mois précédent et l'année précédente
    int moisPrecedent = moisHier(jour, mois, annee);
    int anneePrecedente = anneeHier(jour, mois, annee);

    // 2. On demande combien de jours il y a dans ce mois précédent
    // Attention : si l'année change (1er janvier), anneePrecedente gère ça correctement
    return joursDansMois(moisPrecedent, anneePrecedente);
}
```

---

### Le chef d'orchestre : `reculerDate`

Cette méthode utilise les trois méthodes précédentes dans une boucle.

**Logique :**

1. Vérifier si la date de départ est valide.
2. Boucler `decalage` fois.
3. À chaque tour, mettre à jour jour, mois et année vers "hier".
4. Vérifier si la nouvelle date est valide (si on passe en dessous de 1800).
5. Formater le résultat.

```java
/**
 * Calcule une date antérieure selon un décalage donné.
 */
public static String reculerDate(int jour, int mois, int annee, int decalage) {
    // Validation initiale
    if (!dateValide(jour, mois, annee)) {
        return "DATE ILLEGALE";
    }

    int j = jour;
    int m = mois;
    int a = annee;

    // On recule jour par jour
    for (int i = 0; i < decalage; i++) {
        // IMPORTANT : On doit sauvegarder les valeurs temporaires car anneeHier 
        // a besoin du jour/mois ACTUEL avant qu'on ne les modifie.
        int tempJ = jourHier(j, m, a);
        int tempM = moisHier(j, m, a);
        int tempA = anneeHier(j, m, a);

        j = tempJ;
        m = tempM;
        a = tempA;
        
        // Vérification intermédiaire : si on passe en 1799 par exemple
        if (!dateValide(j, m, a)) {
             return "DATE ILLEGALE";
        }
    }

    [cite_start]// Formatage selon la consigne [cite: 197] (AAAA MMM JJ)
    // Utilise la méthode utilitaire moisEnStr fournie dans le code de base
    return "%d %s %02d".formatted(a, moisEnStr(m), j);
}

```



### Tests unitaires (Tp01Test.java)

Le TP demande de créer des tests (minimum 5 par méthode). Voici des exemples utilisant JUnit 5.

```java
package votrenom.tp01;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class Tp01Test {

    // --- Tests dateValide ---
    @Test
    void testDateValide_Normale() {
        assertTrue(Tp01.dateValide(15, 6, 2020));
    }
    
    @Test
    void testDateValide_AnneeInvalide() {
        assertFalse(Tp01.dateValide(1, 1, 1799)); // Trop tôt
        assertFalse(Tp01.dateValide(1, 1, 2201)); // Trop tard
    }

    @Test
    void testDateValide_MoisInvalide() {
        assertFalse(Tp01.dateValide(1, 13, 2020));
    }

    @Test
    void testDateValide_JourInvalide() {
        assertFalse(Tp01.dateValide(32, 1, 2020)); // Janvier a 31 jours
        assertFalse(Tp01.dateValide(30, 2, 2020)); // Février n'a jamais 30 jours
    }

    @Test
    void testDateValide_Bissextile() {
        assertTrue(Tp01.dateValide(29, 2, 2020)); // 2020 est bissextile
        assertFalse(Tp01.dateValide(29, 2, 2019)); // 2019 ne l'est pas
    }

    // --- Tests reculerDate ---
    @Test
    void testReculerDate_MemeMois() {
        // 2018 MAR 3 - 21 jours -> doit être géré par avancerDate dans l'exemple, 
        // mais testons le recul simple : 24 mars - 3 jours = 21 mars
        assertEquals("2018 MAR 21", Tp01.reculerDate(24, 3, 2018, 3));
    }

    @Test
    void testReculerDate_ChangementMois() {
        // 3 Mars - 5 jours = 26 Février (28 jours en 2018)
        assertEquals("2018 FEV 26", Tp01.reculerDate(3, 3, 2018, 5));
    }

    @Test
    void testReculerDate_AnneeBissextile() {
        // 1 Mars 2020 - 1 jour = 29 Fev 2020
        assertEquals("2020 FEV 29", Tp01.reculerDate(1, 3, 2020, 1));
    }

    @Test
    void testReculerDate_ChangementAnnee() {
        // 1 Janvier 2020 - 1 jour = 31 Dec 2019
        assertEquals("2019 DEC 31", Tp01.reculerDate(1, 1, 2020, 1));
    }

    @Test
    void testReculerDate_HorsLimites() {
        // 5 Janvier 1800 - 10 jours = 1799 -> Illégal
        assertEquals("DATE ILLEGALE", Tp01.reculerDate(5, 1, 1800, 10));
    }
}
```

---

### Conseils pour la réussite

1. **Git Commit :** L'énoncé est strict. Faites un commit après avoir codé `dateValide`, un autre après `jourHier`, etc.
  * `git add .`
  * `git commit -m "Implementation de dateValide"`
2. **JavaDoc :** N'oubliez pas de copier les blocs `/** ... */` au-dessus de vos méthodes comme demandé dans le barème.
3. **Ordre :** Commencez absolument par `dateValide`. Tant qu'elle ne retourne pas `true` correctement, vos autres méthodes retourneront souvent `-1` ou `DATE ILLEGALE` car elles commencent toutes par vérifier la validité.


