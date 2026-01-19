+++
pre = 'Semaine 10 : '
title = 'Gestion de disques'
weight = 100
+++


> [!warning]

> **Cette semaine est critique** : on touche à l'infrastructure physique. C'est l'une des rares opérations où une erreur de frappe peut détruire des données de manière irréversible. Le ton doit être méthodique.


## Objectif

* Ajouter un nouveau disque dur, le préparer et l'intégrer au système de fichiers de manière permanente.

**Fichier pour les exercices (en classe)**
Utiliser le fichier **exo-semaine10.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine10.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---


# Théorie

## Linux vs Windows

Sous Windows, vous avez l'habitude des lettres de lecteurs (`C:`, `D:`, `E:`). Si vous branchez une clé USB, elle reçoit une nouvelle lettre.
Sous Linux, **ce concept n'existe pas**. Tout est intégré dans une seule et unique structure : **l'arborescence de fichiers** (la racine `/`).

Pour utiliser un disque, il faut l'attacher (le "monter") manuellement à un dossier existant.

## Le matériel : Tout est fichier

Linux communique avec le matériel via des fichiers spéciaux situés dans le dossier `/dev` (Devices).

**La nomenclature à connaître :**

| Nom | Signification |
| --- | --- |
| `/dev/sda` | **S**CSI **D**isk **A** (1er disque physique - SATA/SSD). |
| `/dev/sdb` | **S**CSI **D**isk **B** (2e disque physique). |
| `/dev/nvme0n1` | Disque moderne NVMe (souvent le disque principal sur les laptops récents). |
| `/dev/sr0` | Lecteur CD/DVD (ou l'image ISO montée). |

**Les partitions :**
Un disque est découpé en tranches. On ajoute un chiffre après le nom du disque.

* `/dev/sda1` : 1ère partition du disque A.
* `/dev/sda2` : 2e partition du disque A.

> [!primary]
> **L'outil indispensable :** `lsblk`  
> La commande `lsblk` (List Block Devices) affiche l'arbre de vos disques. C'est la première commande à taper pour ne pas se tromper de cible !


### 🟢 Exercice 1 (En classe)

Voici la sortie d'une commande `lsblk`. Répondez aux questions.

```text
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 100G  0 disk
├─sda1   8:1    0   512M  0 part /boot/efi
└─sda2   8:2    0  99.5G  0 part /
sdb      8:16   1    16G  0 disk
└─sdb1   8:17   1    16G  0 part /media/etudiant/USB_KEY
sr0     11:0    1  1024M  0 rom
```

1. Combien y a-t-il de disques physiques connectés ?
2. Lequel de ces disques est probablement une clé USB ? Pourquoi ?
3. Où se trouvent les fichiers du système d'exploitation (la racine) ?
4. Qu'est-ce que le périphérique `sr0` ?


## Le cycle de vie d'un disque

Pour rendre un disque utilisable, il faut suivre 3 étapes obligatoires.

Imaginez que vous achetez un immense entrepôt vide (Le disque dur).

1. **Partitionner (Les murs) :** Vous ne pouvez pas stocker des dossiers en vrac dans un hangar vide. Vous construisez des murs pour créer des locaux sécurisés.
> **Outil :** `fdisk`

2. **Formater (Les étagères) :** Une fois les murs posés, la pièce est vide. Il faut installer un système de rangement (étagères, armoires). Sans cela, les données sont jetées par terre. C'est le **Système de Fichiers** (*Filesystem*).
> **Outil :** `mkfs` (Make FileSystem)

3. **Monter (La porte) :** Votre pièce est prête, mais elle est murée ! Pour y entrer, vous devez percer une porte et lui donner un nom (ex: "Salle des Archives").
> **Outil :** `mount`


### Étape 1 : Partitionner (`fdisk`)

C'est l'action de découper le disque. 
 
**Attention** : `fdisk` est un outil interactif, il vous pose des questions.

```bash
# Sélectionner le disque B (attention à ne pas choisir sda !)
sudo fdisk /dev/sdb
```

**Les touches magiques dans fdisk :**

* `m` : Afficher le menu d'aide.
* `n` : Créer une **N**ouvelle partition.
* `p` : Partition **P**rimaire.
* `d` : **D**elete (supprimer) une partition.
* `w` : **W**rite (Écrire les changements sur le disque et quitter). **Rien n'est fait tant que vous ne tapez pas w !**
* `q` : **Q**uit (Quitter sans rien faire en cas de panique).

### Étape 2 : Formater (`mkfs`)

C'est l'action d'installer le système de fichiers.
Sous Linux, le standard est **ext4** (équivalent du NTFS de Windows).

```bash
# Formater la 1ère partition du disque B
sudo mkfs.ext4 /dev/sdb1
```

### Étape 3 : Monter (`mount`)

Linux ne "voit" pas le contenu du disque tant qu'il n'est pas greffé à un dossier.

```bash
# 1. On crée le point d'ancrage (un dossier vide)
sudo mkdir /mnt/disque_data

# 2. On accroche la partition sur ce dossier
sudo mount /dev/sdb1 /mnt/disque_data
```

À partir de maintenant, tout fichier copié dans `/mnt/disque_data` atterrit physiquement sur le disque B.

### 🟢 Exercice 2 (En classe)

*Au tableau/projecteur. Montrez une capture d'écran de `lsblk` avec un disque de 100 Go (`sda`) et un disque de 5 Go (`sdb`).*

**Question :** "Je veux formater le nouveau petit disque. J'écris `mkfs.ext4 /dev/sda1`. Qu'est-ce qui se passe ?"

<!--
**Réponse attendue :** "Vous effacez votre système d'exploitation (Windows/Linux) et vous perdez tout."
**Morale :** Toujours vérifier 3 fois : est-ce `sda` ou `sdb` ?
-->


## La persistance (Le danger du fstab)

La commande `mount` est **temporaire**. Au redémarrage, Linux "oublie" le montage. Pour le rendre permanent, il faut l'inscrire dans le fichier de configuration `/etc/fstab` (File System Table).

### 1. Pourquoi utiliser l'UUID ?

Les noms `/dev/sdb1` changent si vous changez le branchement des câbles.
L'**UUID** (Universally Unique IDentifier) est l'empreinte digitale du disque. Elle ne change jamais.

```bash
# Récupérer l'UUID
sudo blkid

```

> [!warning]

> Le fichier `/etc/fstab` est lu au démarrage du noyau.
>
> * Si vous faites une faute de frappe dans ce fichier...
> * Le système ne pourra pas monter le disque...
> * Et **Linux refusera de démarrer** (Emergency Mode).


**Règle de survie :**
Après avoir modifié `/etc/fstab`, ne redémarrez JAMAIS tout de suite. Testez votre configuration :

```bash
sudo mount -a
```

* Si la commande ne dit rien : **C'est bon.**
* Si la commande affiche une erreur : **Corrigez le fichier immédiatement !**


### 🟢 Exercice 3 (En classe)

Un étudiant veut monter son disque de sauvegarde automatiquement dans `/mnt/backup`. Il a écrit cette ligne dans `/etc/fstab`.   
Trouvez les **3 erreurs** qui vont empêcher le système de démarrer.

```text
# Fichier /etc/fstab
UUID=a1b2-c3d4   /mnt/backup   ntfs   default   0  0
```

*(**Indices** : Le système de fichiers Linux standard n'est pas ntfs, il manque un 's' quelque part, et la syntaxe des options est souvent au pluriel).*



---

# Laboratoire

Utiliser le fichier **labo10.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo10.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

Votre serveur manque d'espace. Votre patron a inséré physiquement un nouveau disque dur de 5 Go. Votre mission est de le rendre opérationnel pour que le département comptabilité puisse y stocker des archives.

**Prérequis VM :**
*Assurez-vous d'avoir ajouté un second Disque Dur Virtuel (VDI) de 2 Go ou 5 Go dans les paramètres de votre machine virtuelle (Storage > Controller: SATA > Add Hard Disk).*

### Étape 1 : Reconnaissance

1. Allumez votre VM.
2. Ouvrez un terminal et identifiez le nouveau disque :
```bash
lsblk
```


3. Repérez le disque qui n'a pas de partitions (probablement `sdb`). Quelle est sa taille exacte ?

### Étape 2 : Construction des murs (Partitionnement)

Nous allons créer une partition unique qui occupe tout le disque.

1. Lancez l'utilitaire : `sudo fdisk /dev/sdb`
2. Tapez `n` (Nouvelle partition).
3. Tapez `p` (Primaire).
4. Numéro de partition : Appuyez sur **Entrée** (Par défaut : 1).
5. Premier secteur : Appuyez sur **Entrée** (Par défaut : début du disque).
6. Dernier secteur : Appuyez sur **Entrée** (Par défaut : fin du disque).
7. Vérifiez votre travail en tapant `p` (Print). Vous devriez voir `/dev/sdb1`.
8. Sauvegardez en tapant `w` (Write).

### Étape 3 : Décoration (Formatage)

Nous allons formater cette nouvelle partition en **ext4**.

1. Lancez la commande :
```bash
sudo mkfs.ext4 /dev/sdb1
```

2. Notez l'UUID qui s'affiche à l'écran (ou retrouvez-le plus tard avec `blkid`).

### Étape 4 : Emménagement (Montage manuel)

1. Créez le dossier d'accueil :
```bash
sudo mkdir /mnt/archives
```

2. Montez le disque :
```bash
sudo mount /dev/sdb1 /mnt/archives
```

3. Vérifiez que le disque est bien là avec la commande `df -h` (Disk Free -human readable). Vous devriez voir votre disque associé à `/mnt/archives`.


4. Créez un fichier test pour prouver que ça marche :
```bash
sudo touch /mnt/archives/test.txt
```


### Étape 5 : Ancrage définitif (Configuration fstab)

C'est le moment critique.

1. Récupérez l'UUID de `sdb1` :
```bash
sudo blkid
```
*(Copiez la suite de caractères sans les guillemets).*

2. Ouvrez le fichier fstab :
```bash
sudo nano /etc/fstab
```

3. Allez tout en bas du fichier et ajoutez cette ligne (remplacez les X par votre UUID) :
```text
UUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX   /mnt/archives   ext4   defaults   0   0
```

*Utilisez la touche Tabulation pour espacer les colonnes.*
4. Sauvegardez (`Ctrl+O`, `Entrée`) et quittez (`Ctrl+X`).

5. **TEST DE SURVIE :**
Tapez : `sudo mount -a`
* Si aucun message ne s'affiche : **Bravo !** Vous pouvez redémarrer.
* Si un message d'erreur apparaît : **STOP !** Rouvrez le fichier et corrigez l'erreur.


### Étape 6 : Validation finale

Redémarrez votre VM (`sudo reboot`).
Une fois reconnecté, tapez `df -h`. Si `/mnt/archives` est présent, félicitations : vous êtes un administrateur système capable de gérer le stockage.

===============================
---

# Théorie

*« Aujourd'hui, on agrandit la maison. On ne va pas juste copier des fichiers, on va construire l'étagère sur laquelle ils seront posés. »*

### 1. Le matériel sous Linux : Tout est fichier

Contrairement à Windows, il n'y a pas de "Poste de travail" avec des icônes de disques. Les périphériques sont des fichiers dans le dossier `/dev` (Devices) .

* **Nomenclature SATA/SCSI (La norme actuelle) :**
   * `/dev/sda` : **S**CSI **D**isk **A** (1er disque physique).
   * `/dev/sdb` : **S**CSI **D**isk **B** (2e disque physique).


* **Les partitions (Les tranches du gâteau) :**
   * `/dev/sda1` : 1ère partition du 1er disque.
   * `/dev/sdb1` : 1ère partition du 2e disque.


* **La commande de visualisation :**
   * `lsblk` (*LiSt BLock devices*) : Affiche l'arbre des disques. C'est votre meilleur ami pour ne pas formater le mauvais disque.


### 2. Le cycle de vie : Partitionner, formater, monter

Pour qu'un disque neuf soit utilisable, il faut obligatoirement passer par 3 étapes :

1. **Partitionner (`fdisk` ou `gdisk`) :**
   * C'est tracer les frontières.
   * **MBR (Master Boot Record) :** L'ancien standard (max 2 To, max 4 partitions primaires). Utilisé par `fdisk`.
   * **GPT (GUID Partition Table) :** Le standard moderne (Disques immenses, illimité). Utilisé par `gdisk`.

2. **Formater (`mkfs`) :**
   * C'est peindre les cases pour que l'OS puisse écrire dedans. On crée le **Système de Fichiers** (Filesystem) .
   * **ext4 :** Le standard Linux (robuste, journalisé).
   * **xfs :** Très performant pour les gros serveurs (RedHat).
   * **swap :** Mémoire virtuelle (RAM de secours sur disque).

3. **Monter (`mount`) :**
   * C'est accrocher le disque à l'arborescence.
   * On crée un dossier vide (ex: `/mnt/data`) et on "colle" le disque dessus.


### 3. La persistance (`/etc/fstab`)

La commande `mount` est temporaire. Au redémarrage, Linux "oublie" tout ce qui n'est pas écrit dans le fichier de configuration `/etc/fstab` (File System TABle).

* **Le danger des noms (`/dev/sdb`) :** Si vous changez les câbles, `sdb` peut devenir `sdc`. Le système ne démarrera plus.
* **La solution UUID (Universally Unique IDentifier) :** Une empreinte digitale unique pour chaque partition (ex: `a1b2-c3d4...`). C'est ce qu'on utilise dans `fstab` pour être sûr.




---

# LABORATOIRE

**Objectif :** Ajouter un disque de 5 Go à la VM pour simuler un disque de sauvegarde.

### Étape 0 : Ajout du matériel (Hors Linux) (15 min)

1. **Éteignez** proprement votre VM (`poweroff`).
2. Dans VirtualBox : Configuration -> Stockage -> Contrôleur SATA -> Icône "Ajouter un disque dur".
3. Créer -> VDI -> Dynamique -> Taille : **5,00 Go**.
4. Rallumez la VM.

### Étape 1 : Identification et partitionnement (30 min)

1. Ouvrez un terminal et tapez `lsblk`. Repérez le disque de 5 Go (probablement `sdb`).
2. Lancez l'outil de partitionnement : `sudo fdisk /dev/sdb` (Attention à la lettre !).
3. Dans l'interface interactive de fdisk :
   * Tapez `n` (New partition).
   * Tapez `p` (Primary).
   * Tapez `1` (Numéro 1).
   * Entrée (Premier secteur par défaut).
   * Entrée (Dernier secteur par défaut = tout le disque).
   * Tapez `w` (Write) pour sauvegarder et quitter.


4. Refaites `lsblk`. Vous devriez voir `sdb1` apparaître sous `sdb`.

### Étape 2 : Formatage (Construction du système de fichiers) (30 min)

1. Appliquez le système de fichiers ext4 sur la partition :
`sudo mkfs.ext4 /dev/sdb1`
*(Cela prend quelques secondes).*

### Étape 3 : Montage manuel (Test) (15 min)

1. Créez le point d'ancrage (le dossier) :
`sudo mkdir /mnt/backup`
2. Montez le disque :
`sudo mount /dev/sdb1 /mnt/backup`
3. Vérifiez l'espace disque disponible :
`df -h`
*(Vous devez voir une ligne `/dev/sdb1` avec une taille d'environ 4.9G).*
4. Testez l'écriture :
`sudo touch /mnt/backup/test.txt`

### Étape 4 : Automatisation (Persistance) (30 min)

*Le moment de vérité.*

1. Trouvez l'UUID de votre nouvelle partition :
`sudo blkid`
*(Copiez la longue chaîne de caractères sans les guillemets pour `/dev/sdb1`).*
2. Éditez le fichier fstab :
`sudo nano /etc/fstab`
3. Ajoutez une ligne à la fin (Attention à la syntaxe !) :
`UUID=votre-uuid-ici   /mnt/backup   ext4   defaults   0   2`
4. Sauvegardez (`Ctrl+X`, `Y`).
5. **TEST DE SÉCURITÉ :** Tapez `sudo mount -a`.
   * Si rien ne s'affiche : C'est parfait, pas d'erreurs.
   * Si un message d'erreur s'affiche : **CORRIGEZ TOUT DE SUITE.** Si vous redémarrez avec une erreur dans fstab, la VM ne démarrera plus (Mode Urgence).



### Étape 5 : Le redémarrage (15 min)

1. Redémarrez la VM (`reboot`).
2. Une fois revenu, tapez `df -h`.
3. Si `/mnt/backup` est là, félicitations : vous êtes un admin système capable de gérer le stockage.

---

## Corrigé du laboratoire

> À venir (samedi ou dimanche)



### 💡 Astuces de la semaine


> **⚠️ ATTENTION : La zone de danger FSTAB**
> Le fichier `/etc/fstab` est lu au démarrage du noyau. S'il contient une faute de frappe, le démarrage échoue.
> **La Règle d'Or :**
> Après avoir modifié ce fichier, lancez **toujours** la commande `sudo mount -a` avant d'éteindre l'ordinateur.
> * Pas de nouvelle = Bonne nouvelle (Tout va bien).
> * Message d'erreur = Ne redémarrez pas ! Rouvrez le fichier et corrigez.
> 
> 
> **Structure de la ligne fstab :**
> `QUOI` (UUID) `OÙ` (/dossier) `COMMENT` (ext4) `OPTIONS` (defaults) `DUMP` (0) `PASS` (2)
