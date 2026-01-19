+++
pre = 'Semaine 6 : '
title = "Permissions & droits d'accès"
weight = 60
+++


## Objectifs de la semaine

*  **Déchiffrer** la ligne cryptique `drwxr-xr-x` commande `ls -l`.
*  **Comprendre** la différence entre les droits sur un fichier et sur un dossier.
*  **Manipuler** les permissions avec `chmod` (Mode Symbolique et Octal).
*  **Gérer** les propriétaires avec `chown`, les groupes avec `chgrp`.

**Fichier pour les exercices (en classe)**
Utiliser le fichier **exo-semaine6.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine6.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

# Théorie

## Qui a le droit ? (Les 3 identités)

Sous Linux, chaque fichier appartient à un propriétaire et à un groupe. Quand vous essayez d'ouvrir un fichier, Linux se pose trois questions dans l'ordre :

1.  **Est-ce le propriétaire (User - u) ?** "C'est mon fichier à moi."
2.  **Est-ce un membre du Groupe (Group - g) ?** "C'est un fichier de mon équipe."
3.  **Est-ce n'importe qui d'autre (Others - o) ?** "C'est le reste du monde."

> [!primary]
> **User (u) :** Vous. Vous avez les clés de votre chambre.  
> **Group (g) :** Votre famille. Ils ont les clés de la maison (salon, cuisine), mais pas forcément de votre journal intime.  
> **Others (o) :** Les passants dans la rue. Ils peuvent voir la façade (lecture), mais ne peuvent pas entrer.  


## Quels sont les pouvoirs ? (R, W, X)

Il existe trois permissions fondamentales. 

> [!warning]

> Attention, elles n'ont pas le même sens pour un **fichier** et un **dossier** !

| Lettre | Pouvoir | Sur un FICHIER 📄 | Sur un DOSSIER 📁 |
| :---: | :--- | :--- | :--- |
| **r** | **Read** (Lire) | Voir le contenu (cat, vim). | Lister les fichiers (ls). |
| **w** | **Write** (Écrire) | Modifier ou supprimer le contenu. | **Créer ou supprimer** des fichiers dans ce dossier. |
| **x** | **eXecute** (Exécuter) | Lancer le programme (script). | **Entrer** dans le dossier (cd). |

> [!primary]
> Avoir le droit de lecture (`r`) sur un dossier permet de voir les noms des fichiers (`ls`), mais si vous n'avez pas le droit d'exécution (`x`), vous ne pouvez pas "traverser" le dossier pour lire les fichiers à l'intérieur !  
**En résumé : Pour un dossier, on donne presque toujours `r` et `x` ensemble.**

### 🟢 Exercice 1 (En classe)

Traduisez ces lignes en français. 

1.  `-rw-r--r--  jean  compta  rapport.txt`
    * *Réponse :* Jean peut lire/écrire. Le groupe Compta peut lire. Les autres peuvent lire. C'est un fichier standard.
2.  `drwxr-x---  marie  devs    projet_secret`
    * *Réponse :* Marie a tous les droits. Les devs peuvent entrer et lister. Les autres n'ont aucun accès. C'est un dossier.

## La notation octale (4, 2, 1)

Les administrateurs préfèrent les chiffres aux lettres. C'est plus rapide.
Chaque permission a une valeur :

* **Read (r) = 4**
* **Write (w) = 2**
* **Execute (x) = 1**
* **Rien (-) = 0**

On additionne ces chiffres pour obtenir un score par identité.

*Exemple : `rwx` = 4 + 2 + 1 = **7***.  
*Exemple : `r-x` = 4 + 0 + 1 = **5***.


**Tableau de conversion binaire --> octal (À apprendre par cœur) :**
| Symbole | Binaire | Octal | Signification |
| :--- | :--- | :---: | :--- |
| `rwx` | 111 | **7** | Tout permis |
| `rw-` | 110 | **6** | Lecture + Écriture |
| `r-x` | 101 | **5** | Lecture + Exécution |
| `r--` | 100 | **4** | Lecture seule |
| `---` | 000 | **0** | Aucun accès |

### 🟢 Exercice 2 (En classe)

Calculez le code octal pour ces situations :

1.  Je veux lire et écrire, mais pas exécuter. (4 + 2 + 0) = **?**
2.  Je veux tout interdire. (0 + 0 + 0) = **?**
3.  Je veux lire et exécuter (cas typique script web). (4 + 0 + 1) = **?**
4.  **Le grand classique :** Moi j'ai tout (7), mon groupe lit et exécute (5), les autres ne font rien (0). Code = **?**


## La commande chmod (*Change Mode*)

Maintenant que vous savez calculer les droits, il faut savoir les appliquer. La commande s'appelle `chmod`. Il existe deux façons de l'utiliser : la méthode "Mathématique" (octal) et la méthode "Symbolique" (lettres).

### 1. Méthode A : Le mode octal

C'est la méthode la plus rapide. On donne le score total pour chaque identité d'un seul coup.

**Syntaxe :** `chmod [User][Group][Other] fichier`

```bash
# Exemple 1 : Le classique "Lecture/Exécution"
# User(7) = rwx, Group(5) = r-x, Other(5) = r-x
chmod 755 script.sh

# Exemple 2 : Le fichier "Top Secret"
# User(6) = rw-, Group(0) = ---, Other(0) = ---
chmod 600 secrets.txt
```

### 2. Méthode B : Le mode symbolique

Parfois, vous ne voulez pas tout recalculer, vous voulez juste ajouter ou enlever un droit précis.
**Syntaxe** : chmod [Qui][Action][Quoi] fichier
* **Qui** : u (user), g (group), o (other), a (all/tous).  
* **Action** : + (ajouter), - (enlever), = (fixer exactement).  
* **Quoi** : r, w, x.

```bash
# Ajouter le droit d'exécution à tout le monde (All + Execute)
chmod a+x script.sh

# Enlever le droit d'écriture aux "autres" (Other - Write)
chmod o-w rapport.txt

# Donner à mon groupe seulement le droit de lecture (écrase les anciens droits)
chmod g=r document.pdf
```

> [!primary]
> Si vous changez les droits d'un dossier, cela ne change pas automatiquement les fichiers à l'intérieur !  
> Pour appliquer les droits au dossier ET à tout son contenu (enfants, petits-enfants...), utilisez l'option majuscule -R.
```bash
chmod -R 755 /opt/mon_dossier_web
```



## La commande chown (*Change Owner*)

Changer les permissions (chmod) est inutile si le fichier n'appartient pas à la bonne personne. La commande `chown` permet de modifier le **propriétaire** d'un fichier ou d'un dossier.

> [!primary]
> Seul l'administrateur (**root**) ou un utilisateur avec les droits `sudo` peut changer le propriétaire d'un fichier pour des raisons de sécurité.

**Syntaxe :** `chown [utilisateur] fichier`

```bash
# Transférer la propriété à l'utilisateur "alice"
sudo chown alice rapport.txt

# On peut aussi changer le groupe en même temps avec le format utilisateur:groupe
sudo chown alice:comptabilite rapport.txt
```

### 🟢 Exercice 3 (En classe)

1.  Créez un fichier vide : `touch test.txt`
2.  Vérifiez ses droits : `ls -l test.txt`
3.  Interdisez tout à tout le monde : `chmod 000 test.txt`
4.  Essayez de le lire : `cat test.txt` *(Permission denied)*
5.  Redonnez vous les droits : `chmod 700 test.txt`


## La commande chgrp (*Change Group*)

Si vous voulez uniquement modifier le **groupe propriétaire** d'un fichier sans toucher à l'utilisateur, on utilise la commande `chgrp`.

**Syntaxe :** `chgrp [groupe] fichier`

```bash
# Changer le groupe du fichier pour "developpeurs"
sudo chgrp developpeurs code_source.py
```

### 🟢 Exercice 4 (En classe)

Quelle commande utiliseriez vous pour donner la propriété d'un fichier nommé 'rapport.txt' à l'utilisateur 'nicolas' ?

**A**. chmod nicolas rapport.txt  
**B**. sudo edit owner nicolas rapport.txt  
**C**. chown nicolas rapport.txt  
**D**. chgrp nicolas rapport.txt  


## Récapitulatif : Propriété vs Permissions

Il est crucial de comprendre l'ordre logique d'administration Linux :

1. **chown** : "À qui appartient cet objet ?"
2. **chmod** : "Qu'est-ce que ce propriétaire a le droit de faire ?"

| Commande | Action | Exemple |
| --- | --- | --- |
| **chmod** | Modifie les droits (rwx) | `chmod 644 file.txt` |
| **chown** | Modifie le propriétaire (user) | `chown bob file.txt` |
| **chgrp** | Modifie le groupe (group) | `chgrp staff file.txt` |


## Récapitulatif : Le mode récursif (-R)

Tout comme pour `chmod`, si vous travaillez sur un dossier contenant des dizaines de sous-fichiers, vous ne voulez pas les modifier un par un. L'option `-R` (Recursive) s'applique ici aussi.

```bash
# Donne la propriété de tout le site web à l'utilisateur "www-data"
# ainsi qu'à tous les fichiers et sous-dossiers contenus dedans.
sudo chown -R www-data:www-data /var/www/html
```

> [!warning]

> Soyez prudent avec `chown -R` ! Une erreur de dossier peut rendre votre système instable si vous changez par erreur le propriétaire de fichiers système critiques.


---

# Laboratoire

Utiliser le fichier **labo6.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo6.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

Vous êtes l'administrateur système. Vous devez mettre en place une structure de dossiers sécurisée pour deux départements.

### Étape 1 : Préparation (utilisateurs et groupes)
*Révision de la semaine passée.*

1.  Créez deux utilisateurs : **valerie** et **pierre**.
2.  Créez deux groupes : **direction** et **comptabilite**.
3.  Ajoutez **valerie** au groupe `direction`.
4.  Ajoutez **pierre** au groupe `comptabilite`.

### Étape 2 : Le dossier public
Créez un dossier `/opt/public`.
* **Propriétaire :** Root.
* **Groupe :** Root.
* **Consigne :** Tout le monde doit pouvoir lire, écrire et naviguer dedans.
<!--* **Commande :** `sudo chmod 777 /opt/public`-->
* *Test :* Connectez vous en tant que Valérie, créez un fichier dedans. Connectez vous en Pierre, modifiez le fichier de Valérie.

### Étape 3 : Le dossier confidentiel (Direction)
Créez un dossier `/opt/vip`.
* **Propriétaire :** Valérie.
* **Groupe :** Direction.
* **Consigne :**
    * Valérie a tous les droits.
    * Le groupe `direction` peut lire et entrer, mais pas modifier.
    * Les autres (Pierre/Comptabilite) ne doivent même pas pouvoir entrer.
* **Défi :** Trouvez le `chmod` et le `chown` nécessaires.

### Étape 4 : Le transfert de propriété
Pierre a écrit un rapport important `bilan.txt` dans son dossier personnel. Il quitte l'entreprise.
1.  En tant que Pierre, créez le fichier.
2.  En tant qu'Admin (vous), transférez la propriété du fichier à **Valérie**.
3.  Vérifiez avec `ls -l` que Valérie est bien la nouvelle propriétaire.

### Étape 5 : Le dossier temporaire
*Situation :* Dans le dossier `/opt/public` (créé à l'étape 2), Pierre s'amuse à supprimer les fichiers de Valérie. C'est un problème.
1.  Cherchez ce qu'est le "Sticky Bit" sur internet.
2.  Appliquez-le sur `/opt/public` (`chmod +t` ou `1777`).
3.  Vérifiez maintenant : Pierre peut écrire, mais peut-il supprimer un fichier qui appartient à Valérie ?


> [!primary]
> Pour ce laboratoire, en plus des captures d'écran habituelles, vous devez aussi remettre le résultat de votre recherche Internet sur le "Sticky Bit".


## Corrigé du laboratoire

> À venir (samedi ou dimanche)

<!--

# LABORATOIRE

**Objectif :** Créer un dossier confidentiel et vérifier que personne ne peut y entrer.

### Étape 1 : Manipulation (`chmod`) 

1. Créez un dossier `SECRET`.
2. Créez un fichier `code.txt` à l'intérieur.
3. Retirez le droit d'exécution au dossier (`chmod -x SECRET`).
4. Essayez de faire `cd SECRET`. (Ça échoue, car on ne peut pas traverser).
5. Remettez le droit (`chmod +x SECRET`).

### Étape 2 : Scénario "Top secret" 

1. Créez un dossier `/opt/projet_compta`.
2. Changez le propriétaire groupe pour `comptabilite` (`chown :comptabilite ...`).


3. Configurez les droits pour que :
* Seuls les comptables peuvent lire/écrire (`770`).
* Les informaticiens (et les autres) ne peuvent rien voir.


4. **Le test ultime :** Connectez vous en tant que `bob` (informatique) avec `su - bob` et essayez d'entrer. Ça doit échouer. Connectez-vous en tant que `alice` (compta), ça doit marcher.

-->

