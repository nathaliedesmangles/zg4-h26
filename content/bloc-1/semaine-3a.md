+++
pre = 'Semaine 3.1 : '
title = 'Gestion des Utilisateurs et des groupes'
weight = 14
+++



## Objectifs de la séance

* **Comprendre** l'identité numérique sous Linux (UID, GID) et le concept de groupe primaire vs secondaire.
* **Auditer** les fichiers système (`/etc/passwd`, `/etc/shadow` et `/etc/group`).
* **Gérer** le cycle de vie des comptes (création, modification, suppression, verrouillage).


---

# Théorie

Linux est un système multi-utilisateurs strict. Pour le noyau, vous n'êtes pas un nom, mais une série de numéros.

## Les identifiants (UID & GID)

Chaque utilisateur possède un **UID** (*User ID*) et appartient à un groupe principal identifié par un **GID** (*Group ID*).

| Plage d'UID | Rôle | Description |
| --- | --- | --- |
| **0** | **Root** | L'administrateur suprême. Accès total et illimité. |
| **1 à 999** | **Système** | Comptes pour les services (ex: `www-data` pour le web). Pas de connexion humaine. |
| **1000+** | **Humains** | Utilisateurs réels. Le premier compte créé reçoit souvent l'UID 1000. |



## Groupes primaires vs secondaires

C'est une distinction cruciale pour la gestion des droits sous Linux.

### 1. Le groupe primaire (Identité de base)

* **Définition :** C'est le groupe inscrit directement dans le fichier `/etc/passwd`.
* **Rôle :** Lorsqu'un utilisateur crée un fichier, ce fichier appartient automatiquement à son groupe primaire.
* **Règle :** Un utilisateur a **exactement un seul** groupe primaire.
* **Usage :** Sous les distributions modernes (Ubuntu, Debian, CentOS), Linux crée par défaut un groupe portant le même nom que l'utilisateur (ex: l'utilisateur `albert` a pour groupe primaire `albert`).

### 2. Les groupes secondaires (Les permissions)

* **Définition :** Ce sont tous les autres groupes auxquels appartient l'utilisateur (définis dans `/etc/group`).
* **Rôle :** Ils servent à donner des accès supplémentaires (ex: accès au groupe `sudo` pour administrer, `docker` pour gérer des conteneurs, ou `comptabilite` pour lire des rapports).
* **Règle :** Un utilisateur peut appartenir à **une infinité** de groupes secondaires (ou aucun).

> [!primary]
> Les groupes peuvent être spécifiés par leur **GID** ou leur **nom**.

#### Le groupe *wheel*

* Le groupe wheel permet à ses membres d’utiliser des commandes avec des privilèges administratifs grâce à `sudo`.


## Les fichiers de configuration

Linux utilise trois fichiers texte simples pour gérer la sécurité :

### 1. Le fichier ***/etc/passwd*** (L'annuaire)

Tout le monde peut le lire. Il définit les comptes.

* **Format :** `nom:x:UID:GID:Commentaire:Home:Shell`
* **Exemple :** `etudiant:x:1000:1000:Etudiant,,,:/home/etudiant:/bin/bash`
* *Le 4ème champ (`1000`) est le GID du groupe primaire.*



### 2. Le fichier ***/etc/shadow*** (Les secrets)

Contient les mots de passe chiffrés (*hashs*). Seul **root** peut y accéder.

> [!primary]
> Si vous voyez `!` ou `*` à la place du hash, le compte est verrouillé.

### 3. Le fichier ***/etc/group*** (Les clubs)

Définit les noms des groupes et liste les membres **secondaires**.

* **Format :** `nom_groupe:x:GID:liste_membres_secondaires`
* **Exemple :** `sudo:x:27:albert,nancy` (Albert et Nancy sont membres secondaires du groupe sudo).



## Les commandes de gestion

### 1. Création (`useradd`)

La syntaxe `useradd [options] [nom_utilisateur]`

La commande `useradd` est précise mais minimaliste. On doit alors l'utiliser avec des **options**. Voici les plus courantes.

```bash
# -u : Spécifie un UID.
# -c : Ajoute un commentaire pour les archives.
# -m : Crée le dossier personnel.
# -g : Définit le groupe PRIMAIRE (par nom ou GID)
# -G : Définit les groupes SECONDAIRES (séparés par des virgules)
# -s : Définit le Shell par défaut (ex: `/bin/bash`)
# -e : Définit une date d'expiration.

sudo useradd -m -g users -G sudo,devs -s /bin/bash alfred
```

> [!primary]
> L'utilisateur sera ajouté à la **dernière ligne** du fichier `etc/passwd`. Vous pouvez ensuite vérifier l’utilisateur ajouté dans le fichier:

```bash
tail -1 /etc/passwd
```

> [!primary]
> **Création d'un utilisateur avec `adduser`**  
> En réalité, `adduser` est l'implémentation standard sur les systèmes Debian et Ubuntu qui agit comme un "wrapper" (emballage) autour de l'outil de bas niveau `useradd` pour offrir cette interface interactive et simplifiée. 

### 2. Modification (`usermod`)

C'est ici que l'on gère les promotions ou changements de service.

> [!primary]
> **Le piège classique :** Si vous faites `usermod -G projet_a alfred`, vous **écrasez** tous ses anciens groupes secondaires. S'il était admin, il ne l'est plus !  
> **La règle d'or :** Toujours utiliser `-a` (Append) avec `-G`.

```bash
# ✅ BONNE PRATIQUE : Ajoute le groupe sans supprimer les anciens
sudo usermod -aG comptabilite alfred

# Modifier le groupe primaire (rarement utilisé)
sudo usermod -g nouveaux_membres alfred
```

### 3. Vérification (`id` et `groups`)

Pour savoir qui appartient à quoi :

```bash
id alfred
# Affiche : uid=1001(alfred) gid=100(users) groupes=100(users),27(sudo),1005(comptabilite)
```

### 🟢 Exercice (en classe)

1. Créer un groupe nommé `direction`.
2. Créer un utilisateur `pdg` avec `/bin/bash` et son dossier personnel.
3. Assigner `pdg` au groupe `direction` en tant que **groupe primaire** (utilisez `-g`).
4. Ajouter `pdg` au groupe `sudo` en tant que **groupe secondaire** (utilisez `-aG`).
5. Vérifier avec `id pdg`. Que remarquez-vous sur la position du groupe `direction` ?

<!--
SOLUTION

```bash
# 1. Création du groupe direction
sudo groupadd direction

# 2 & 3. Création de l'utilisateur avec son groupe primaire (-g)
# Note : On combine la création du home (-m) et du shell (-s)
sudo useradd -m -s /bin/bash -g direction pdg

# 4. Ajout au groupe secondaire sudo (-aG)
sudo usermod -aG sudo pdg

# 5. Vérification de l'identité
id pdg
```

### Analyse du résultat de la commande `id`

Le résultat devrait ressembler à ceci :
`uid=1001(pdg) gid=1002(direction) groupes=1002(direction),27(sudo)`

**Ce que l'on remarque sur la position du groupe `direction` :**

1. **Champ `gid=` :** Le groupe `direction` apparaît en premier juste après l'UID. C'est ici qu'est défini le **groupe primaire**. Tous les fichiers créés par le PDG appartiendront par défaut à ce groupe.
2. **Champ `groupes=` :** On remarque que `direction` est listé à nouveau dans la liste globale des groupes, suivi de `sudo`.
3. **Absence du groupe "pdg" :** Contrairement à une création standard, Linux n'a pas créé de groupe nommé `pdg` car nous avons explicitement forcé l'utilisation de `direction` comme groupe principal avec l'option `-g`.

-->



## 4. Verrouillage et suppression

### 1. Le piège du Shell vide

Si votre utilisateur voit un `$` au lieu de `utilisateur@machine:~$` et que les flèches du clavier ne fonctionnent pas, c'est qu'il est sur le shell `sh` (basique).  
**Correction :** `sudo usermod -s /bin/bash nom_utilisateur`

### 2. Verrouiller sans supprimer

En cas de départ temporaire ou enquête :

```bash
sudo passwd -l alfred  # Lock (verrouille)
sudo passwd -u alfred  # Unlock (déverrouille)
```

### 3. Suppression propre

```bash
sudo userdel -r alfred # -r supprime aussi le dossier /home/alfred
```

<!--
---

# Laboratoire

**Objectif :** Simuler l'arrivée de nouveaux employés dans une entreprise.

Utiliser le fichier **labo5.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo5.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

Vous êtes administrateur système pour la start-up "Momo420".

1. **Embauche :** Créer les comptes pour les nouveaux développeurs (avec dossier home).
2. **Sécurité :** Forcer le changement de mot de passe à la première connexion.
3. **Groupes :** Créer un groupe `devs` et y ajouter les utilisateurs.
4. **Départ :** Un stagiaire quitte l'entreprise. Verrouillez son compte sans supprimer ses fichiers.

### Étape 1 : Audit 

1. Utilisez `grep` pour trouver votre utilisateur dans `/etc/passwd`.
2. Essayez de lire `/etc/shadow`. Que se passe-t-il ?.
3. Utilisez `sudo cat /etc/shadow` pour voir la différence.


### Étape 2 : Recrutement massif 

*Vous êtes l'admin système.*

1. Créez deux groupes:

   * `comptabilite`
   * `informatique`

2. Créez 3 utilisateurs (avec `useradd` ou `adduser`):

   * `andre` (dans le groupe `comptabilite`).
   * `sylvie` (dans le groupe `informatique`).
   * `charles` (dans le groupe `informatique` aussi).

3. Assignez leur le mot de passe : `Cegep`.


### Étape 3 : Gestion des groupes 

1. `andre` a obtenu une promotion. Ajoutez-le au groupe `sudo` (ou `wheel`) pour qu'il devienne administrateur.

**Commande** : `usermod -aG sudo andre` (Attention au `-a` pour Append, sinon on écrase ses autres groupes !).


2. Vérifiez les groupes de Andre avec la commande `groups andre`.

-->


<!--
## Corrigé du laboratoire

> À venir (samedi ou dimanche)


## Solution du laboratoire

### Étape 1 : Audit

1. **Trouver son utilisateur :**
```bash
grep "$USER" /etc/passwd
```


2. **Lire `/etc/shadow` sans privilèges :**
* *Résultat :* "Permission non accordée". C'est normal, ce fichier contient les empreintes des mots de passe et seul l'utilisateur **root** y a accès pour garantir la sécurité du système.


3. **Lire avec `sudo` :**
```bash
sudo cat /etc/shadow
```


* *Observation :* Vous voyez maintenant les hashs (caractères cryptiques entre les `:`).


### Étape 2 : Recrutement massif

**1. Création des groupes :**

```bash
sudo groupadd comptabilite
sudo groupadd informatique
```

**2. Création des utilisateurs :**
Ici, nous utilisons `useradd` avec les options nécessaires pour que les comptes soient fonctionnels immédiatement.

```bash
# André (Comptabilité)
sudo useradd -m -s /bin/bash -g comptabilite andre

# Sylvie (Informatique)
sudo useradd -m -s /bin/bash -g informatique sylvie

# Charles (Informatique)
sudo useradd -m -s /bin/bash -g informatique charles
```

**3. Assignation des mots de passe :**

```bash
echo "andre:Cegep" | sudo passwd andre
echo "sylvie:Cegep" | sudo passwd sylvie
echo "charles:Cegep" | sudo passwd charles
```

*(Note : `chpasswd` est plus rapide pour les créations en lot, mais `sudo passwd andre` fonctionne aussi individuellement).*



### Étape 3 : Gestion des groupes et Sécurité

**1. Promotion d'André :**
André garde son groupe primaire `comptabilite` mais reçoit les pouvoirs d'administration en devenant membre secondaire de `sudo`.

```bash
sudo usermod -aG sudo andre
```

**2. Vérification :**

```bash
groups andre
# Sortie attendue : andre : comptabilite sudo
```

**3. Sécurité : Forcer le changement de mot de passe**
Pour l'objectif de sécurité mentionné en début de labo, on utilise la commande `chage` (Change Age) :

```bash
# Force le changement à la prochaine connexion pour tous
sudo chage -d 0 andre
sudo chage -d 0 sylvie
sudo chage -d 0 charles
```



### Étape 4 : Départ (Gestion du cycle de vie)

Un stagiaire (appelons-le `stagiaire1`) quitte l'entreprise. On veut empêcher toute connexion sans supprimer ses données pour pouvoir les auditer plus tard.

```bash
# Verrouiller le mot de passe (met un ! devant le hash dans /etc/shadow)
sudo passwd -l stagiaire1

# Alternative : Expire le compte complètement
sudo usermod -e 1 stagiaire1
```

---

### 💡 Synthèse pour l'examen

| Action | Commande clé | Pourquoi ? |
| --- | --- | --- |
| **Créer** | `useradd -m -g [G_PRIMAIRE]` | Le `-m` crée la maison de l'utilisateur. |
| **Ajouter un droit** | `usermod -aG [G_SECONDAIRE]` | Le `-a` évite de supprimer les autres accès. |
| **Vérifier** | `id [USER]` | Donne l'UID et TOUS les groupes d'un coup. |
| **Désactiver** | `passwd -l [USER]` | Sécurise le départ d'un employé sans perte de données. |

-->
