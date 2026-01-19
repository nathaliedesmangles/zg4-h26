+++
pre = 'Semaine 15 : '
title = 'Révision & Examen Final'
weight = 150
+++


### Objectif de la semaine

* Démontrer une autonomie complète sur l'administration, la sécurisation et l'automatisation d'un système Linux.

### Plan de la séance #1

Il n'y a plus de nouvelle matière. C'est le moment de consolider tout ce qui a été vu depuis 15 semaines et de prouver que vous êtes prêts pour le prochain cours dans vos études.

1. La "Big Picture" (Vue d'ensemble) (30 min)  
2. Les 5 pièges de l'examen (30 min)
3. FAQ et dépannage (45 min)
4. Préparation mentale (15 min)

### Plan de la séance #2



---

# RÉVISION & CLÔTURE

*« Aujourd'hui, c'est le *"Boss Battle"*. Vous avez tous les outils en main. On va juste s'assurer qu'ils sont bien acquis. »*

### 15.1 La "Big Picture" (Vue d'ensemble)

On retrace le chemin parcouru pour connecter les points :

1. **Le Matériel (Sem 10) :** On a ajouté des disques (`/dev/sdb`).
2. **Le Système (Sem 1-4) :** On a installé l'OS et appris à bouger dedans.
3. **Les Humains (Sem 5-8) :** On a créé des portes (`rwx`) et des clés (`utilisateurs`).
4. **L'Intelligence (Sem 11-14) :** On a créé des robots (`scripts`) pour gérer tout ça à notre place.

### 15.2 Les 5 pièges de l'examen

Rappel des erreurs qui coûtent 0/100 à une question :

1. **Le FSTAB brisé :** Oublier de faire `mount -a` après avoir édité `/etc/fstab`. Résultat : La VM ne redémarre pas.
2. **L'espace fatal :** Écrire `NOM = Toto` dans un script. (Pas d'espaces autour du égal !).
3. **Le script muet :** Oublier `chmod +x script.sh`. Le Cron ne pourra jamais le lancer.
4. **La mauvaise partition :** Formater `sda1` (le système) au lieu de `sdb1` (le nouveau disque).
5. **Le *Slash* de trop :** `mv /dossier` vs `mv dossier`. (Absolu vs Relatif).

### 15.3 FAQ et dépannage

* Atelier libre.
* Les étudiants testent leurs scripts de sauvegarde une dernière fois.
* Le prof passe voir ceux qui ont encore des problèmes avec les boucles `for`.

### 15.4 Préparation mentale

* Nettoyage des VM (ou déploiement d'une VM d'examen fraîche).
* Consigne : "À l'examen, lisez TOUT l'énoncé avant de taper la première commande."

---

# EXAMEN FINAL (1h30)

**Mise en situation :**
Vous êtes l'administrateur système junior de la compagnie "TechCégep". Votre superviseur est absent et vous laisse une liste de tâches critiques à effectuer sur le serveur. Vous devez livrer un système fonctionnel, sécurisé et automatisé.

---

### 📄 FEUILLE D'EXAMEN

**Consignes :**

* Connectez-vous en tant que `etudiant`.
* Toutes les tâches doivent être vérifiables.
* L'accès à vos notes de cours (PDF/Blackbook) est permis. Internet est interdit.

#### SECTION A : Infrastructure & stockage (10 points)

*Votre serveur manque d'espace. Vous devez ajouter un disque dédié aux archives.*

1. Ajoutez un disque dur virtuel de **2 Go** à votre VM.
2. Partitionnez ce disque pour utiliser tout l'espace disponible (Une seule partition primaire).
3. Formatez cette partition en **ext4**.
4. Créez le répertoire de montage `/mnt/archives`.
5. Montez la partition de manière **permanente** (via `/etc/fstab`) dans `/mnt/archives`.
6. **Preuve :** Redémarrez la VM. Si elle remonte avec le disque accessible, vous avez les points.

#### SECTION B : Gestion Utilisateurs & Sécurité (10 points)

*Vous devez configurer l'environnement pour l'équipe de développement.*

1. Créez un groupe nommé `developpeurs`.
2. Créez deux utilisateurs : `dave` et `anna`.
3. Ajoutez `dave` et `anna` au groupe `developpeurs`.
4. Créez un dossier `/mnt/archives/projet_web`.
5. Configurez les permissions de ce dossier pour que :
* `dave` et `anna` puissent lire et écrire dedans.
* Les autres utilisateurs (non-membres du groupe) n'aient **aucun accès**.
* *(Indice : Pensez au `chown` et au `chmod` en octal).*



#### SECTION C : Scripting & Automatisation (15 points)

*La compagnie a besoin d'une sauvegarde automatique des configurations.*

1. Créez un script nommé `backup_config.sh` dans `/home/etudiant/Scripts`.
2. **Le script doit :**
   * Vérifier si l'utilisateur qui lance le script est **root**. Sinon, afficher une erreur et quitter.
   * Définir une variable `DATE` au format `AAAA-MM-JJ`.
   * Créer une archive compressée (`.tar.gz`) du dossier `/etc`.
   * Nommer l'archive : `config_backup_$DATE.tar.gz`.
   * Enregistrer l'archive dans `/mnt/archives`.
   * Si l'archivage réussit, écrire "Succès : Sauvegarde du $DATE" dans un fichier `/var/log/backup.log`.
3. Rendez ce script exécutable.
4. Programmez ce script (via Cron) pour qu'il s'exécute **tous les jours à 23h30**.

---

### 📝 GRILLE DE CORRECTION (Total /35)

| Section A : Stockage (10 pts) | Points |
| --- | --- |
| Partitionnement correct (`fdisk`) | 3 |
| Formatage `ext4` réussi | 2 |
| Montage manuel fonctionnel | 2 |
| Persistance `fstab` (Le système redémarre sans erreur et le disque est là) | 3 |

| Section B : Sécurité (10 pts) | Points |
| --- | --- |
| Utilisateurs et Groupe créés correctement | 3 |
| Dossier créé au bon endroit | 1 |
| Propriétaire Groupe changé (`chown :developpeurs`) | 3 |
| Permissions restrictives (`770` ou `2770`) | 3 |

| Section C : Scripting (15 pts) | Points |
| --- | --- |
| Shebang et syntaxe de base corrects | 2 |
| Vérification Root (`if [ $UID -ne 0 ]`) | 3 |
| Variable Date correcte | 2 |
| Commande `tar` fonctionnelle (compression) | 3 |
| Logique de succès (vérification ou suite logique) et Log | 2 |
| Tâche Cron correctement configurée (Syntaxe `30 23 * * *`) | 3 |

---

### 💡 Astuces de survie


> **Rappelez-vous toujours :**
> 1. **Backup :** On ne pleure pas des données perdues, on restaure un backup.
> 2. **Logs :** La réponse à "Pourquoi ça marche pas ?" est toujours dans `/var/log`.
> 3. **Root :** Un grand pouvoir implique de grandes responsabilités. Ne restez pas en root pour rien.
> 4. **Veille d'examen :** On se repose. Les études scientifiques montrent qu'étudier à la dernière minute crée un faux sentiment de compétence (on croit apprendre, mais en fait on ne fait que bourrer son cerveau, ce qui le rend moins efficace)
> 
> Bonne chance pour la suite de votre technique !
> *- Votre prof d'info.*


==========================

Voici une proposition de conversion de l'examen final en **Test Moodle**.

J'ai structuré l'examen pour respecter votre ratio de difficulté :

* **75% Intermédiaire/Standard :** Questions de syntaxe, ordre des opérations, commandes courantes (QCM et Réponses courtes).
* **25% Difficile/Expert :** Scénarios complexes, débogage de script et syntaxe précise (Composition et Réponses courtes strictes).

---

# Test Moodle : Examen Final - Administration Linux (35%)

**Consignes pour l'étudiant :**

> * Ce test évalue l'ensemble des compétences acquises durant les 15 semaines.
> * Soyez précis sur la syntaxe (espaces, casse).
> * Aucune documentation externe n'est autorisée.
> 
> 

---

### PARTIE 1 : Infrastructure et Stockage (Questions 1 à 4)

*Cette section vérifie la compréhension du cycle de vie des disques durs.*

**Question 1 (QCM - Intermédiaire)**
**Énoncé :** Vous venez de brancher un nouveau disque dur physique dans le serveur. Quelle est l'ordre chronologique **obligatoire** des opérations pour le rendre utilisable pour stocker des fichiers ?

1. Formater -> Partitionner -> Monter
2. Partitionner -> Monter -> Formater
3. Partitionner -> Formater -> Monter  **(Bonne réponse)**
4. Monter -> Partitionner -> Formater

**Question 2 (Réponse Courte - Facile)**
**Énoncé :** Vous tapez la commande `lsblk`. Vous voyez votre disque système sur `sda`. Quel sera le nom du fichier de périphérique (device file) correspondant au **deuxième** disque physique SATA branché sur la machine ? (Répondez uniquement par le chemin absolu, ex: `/dev/xyz`).

* **Réponse :** `/dev/sdb`

**Question 3 (Réponse Courte - Intermédiaire)**
**Énoncé :** Vous avez créé une partition `/dev/sdb1`. Écrivez la commande exacte pour la formater avec le système de fichiers standard de Linux (**ext4**).

* **Réponse :** `mkfs.ext4 /dev/sdb1` (Accepter aussi `sudo mkfs.ext4 /dev/sdb1`)

**Question 4 (Composition - Difficile/Expert)**
**Énoncé :** Vous devez rendre le montage permanent. Voici les informations :

* Partition : `/dev/sdb1`
* Point de montage : `/mnt/archives`
* Système de fichiers : `ext4`
* Options : par défaut
* UUID : `550e8400-e29b`
**Rédigez la ligne exacte** que vous devez ajouter dans le fichier `/etc/fstab`. Utilisez l'UUID pour identifier le disque.
* **Correction suggérée :** `UUID=550e8400-e29b /mnt/archives ext4 defaults 0 2`
* *Note au prof : Cette question sépare ceux qui comprennent la syntaxe fstab de ceux qui improvisent.*

---

### PARTIE 2 : Utilisateurs et Sécurité (Questions 5 à 8)

*Cette section vérifie la gestion des droits selon le principe du moindre privilège.*

**Question 5 (QCM - Intermédiaire)**
**Énoncé :** Vous devez ajouter l'utilisateur existant `dave` au groupe `developpeurs` sans lui retirer ses autres groupes actuels. Quelle commande est correcte ?

1. `usermod -G developpeurs dave` (Piège : ça écrase les autres groupes)
2. `usermod -aG developpeurs dave` **(Bonne réponse)**
3. `groupadd dave developpeurs`
4. `chown dave:developpeurs`

**Question 6 (Réponse Courte - Intermédiaire)**
**Énoncé :** Traduisez les permissions symboliques `rwxr-x---` en **notation octale** (chiffre).

* **Réponse :** `750`

**Question 7 (Composition - Difficile/Expert)**
**Énoncé :** Mise en situation : Vous avez un dossier `/projets/web`.

* Le propriétaire doit être `anna`.
* Le groupe propriétaire doit être `developpeurs`.
* Anna doit pouvoir tout faire.
* Les membres du groupe `developpeurs` doivent pouvoir lire et modifier les fichiers, mais pas les supprimer (pas d'exécution non plus).
* Les autres ne doivent rien voir.
**Écrivez les deux commandes nécessaires pour appliquer cette configuration.**
* **Correction suggérée :**
1. `chown anna:developpeurs /projets/web`
2. `chmod 760 /projets/web`



**Question 8 (QCM - Intermédiaire)**
**Énoncé :** Un utilisateur ne peut pas accéder à un dossier alors que les permissions semblent correctes (`777`). Quel fichier de configuration devriez-vous vérifier pour voir si son compte est verrouillé ou s'il n'a pas le bon shell ?

1. `/etc/group`
2. `/etc/shadow`
3. `/etc/passwd` **(Bonne réponse)**
4. `/etc/fstab`

---

### PARTIE 3 : Scripting et Automatisation (Questions 9 à 13)

*Cette section est la plus technique et discrimine les étudiants avancés.*

**Question 9 (Réponse Courte - Facile)**
**Énoncé :** Quelle est la toute première ligne obligatoire d'un script Bash (le Shebang) ?

* **Réponse :** `#!/bin/bash`

**Question 10 (QCM - Intermédiaire)**
**Énoncé :** Dans un script, quelle condition `if` permet de vérifier correctement si le dossier `/mnt/backup` existe ?

1. `if [ -f "/mnt/backup" ]; then`
2. `if [ -d "/mnt/backup" ]; then` **(Bonne réponse)**
3. `if [ exist "/mnt/backup" ]; then`
4. `if [ -e "/mnt/backup" ]; then` (Accepte aussi, mais -d est plus précis pour un dossier. Moodle permet de mettre 100% à la réponse 2 et 80% à la réponse 4 si désiré).

**Question 11 (Réponse Courte - Intermédiaire)**
**Énoncé :** Vous voulez créer une archive compressée du dossier `/etc`. Complétez la commande `tar` avec les bons drapeaux (flags) pour créer une archive **gzippée**.
Commande : `tar _____ backup.tar.gz /etc`

* **Réponse :** `-czf` (ou `-zcf`, `-fcz` tant que 'f' est suivi du nom).

**Question 12 (Réponse Courte - Difficile/Expert)**
**Énoncé :** Vous devez programmer une tâche Cron pour qu'elle s'exécute **tous les jours à 23h30**. Écrivez uniquement les 5 champs de temps (ne mettez pas la commande).

* **Réponse :** `30 23 * * *`

**Question 13 (Composition - Difficile/Expert)**
**Énoncé :** **Analyse de script.**
Le script ci-dessous a pour but de vérifier si l'utilisateur est `root` avant de continuer. S'il n'est pas root, il doit s'arrêter avec un code d'erreur.
Cependant, ce script contient **2 erreurs fatales** qui l'empêchent de fonctionner.
Identifiez les erreurs et réécrivez le bloc de code corrigé.

*Script buggé :*

```bash
if [ $UID -ne 0 ]
then
    echo "Erreur : Il faut être root"
    exit
fi

```

* **Correction attendue :**
1. Erreur 1 : Manque le `;` après le crochet ou le retour à la ligne avant `then` (souvent toléré par bash moderne mais mauvaise pratique), mais surtout **manque d'espaces** si l'étudiant le mentionne.
2. Erreur 2 (Critique) : `exit` tout seul renvoie le code 0 (succès). Il faut `exit 1` pour signaler une erreur.
*Réponse idéale :*


```bash
if [ "$UID" -ne 0 ]; then
    echo "Erreur : Il faut être root"
    exit 1
fi

```



---

### Résumé de la distribution

* **Facile / Intermédiaire (~75%) :**
* Q1 (Ordre partition)
* Q2 (Nom du disque sdb)
* Q3 (Formatage mkfs)
* Q5 (Ajout groupe)
* Q6 (Conversion Octale)
* Q8 (Fichier passwd)
* Q9 (Shebang)
* Q10 (Condition If -d)
* Q11 (Tar flags)


* **Difficile / Expert (~25%) :**
* Q4 (Syntaxe FSTAB complète)
* Q7 (Scénario double Chown/Chmod)
* Q12 (Syntaxe Cron précise)
* Q13 (Logique de script et code d'erreur)