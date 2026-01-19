+++
pre = 'Semaine 14 : '
title = 'Boucles & planification (Cron)'
weight = 140
+++


## Objectif de la semaine

* Traiter des centaines de fichiers en une seconde et programmer des tâches répétitives dans le temps.

**Fichier pour les exercices (en classe)**  
Utiliser le fichier **exo-semaine14.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine14.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

## L'automatisation et les boucles

La paresse est la première qualité de l'administrateur système. Si vous devez répéter une tâche plus de trois fois, **ne le faites pas à la main**.

L'automatisation repose sur le concept de **Boucle** (Loop). Une boucle permet de répéter un bloc de commandes sans avoir à les réécrire.

### 1. La boucle `for`

La boucle `for` est idéale quand **vous connaissez à l'avance** la liste des éléments à traiter (une liste de fichiers, une liste de prénoms, une séquence de nombres).


#### Syntaxe et fonctionnement

```bash
for VARIABLE in LISTE ...; do COMMANDES; done
```

La structure se lit : *"Pour chaque `VARIABLE` trouvée dans la `LISTE`, faire les `COMMANDES` suivantes."*

**Exemple** :

```bash
#  1. variable 2. Liste
#       ↓          ↓
for fichier in *.txt
do
    # 3. L'action (qui utilise le contenant)
    echo "Je traite le fichier : $fichier"
done
```

**Ce qui se passe sous le capot :**

1. Bash regarde la liste (tous les fichiers `.txt`).
2. Il prend le **premier** fichier et met son nom dans la variable `$fichier`.
3. Il exécute les commandes entre `do` et `done`.
4. Il remonte, prend le **deuxième** fichier, met son nom dans `$fichier` (l'ancien est oublié).
5. Il recommence jusqu'à ce que la liste soit vide.


### 🟢 Exercice 1 (En classe)

**Objectif :** Créer plusieurs dossiers d'un seul coup sans taper `mkdir` 3 fois.

1. Ouvrez votre terminal.
2. Écrivez (ou copiez) la commande suivante et validez :
```bash
for dossier in Projet_A Projet_B Projet_C
do
    echo "Création du dossier $dossier..."
    mkdir "$dossier"
done
```
3. Vérifiez le résultat avec la commande `ls`. Vous devriez voir vos trois dossiers créés.



### 2. La boucle `while`

La boucle `while` (Tant que) est utilisée quand **on ne sait pas combien de temps** cela va durer. Elle continue tant qu'une condition précise reste **VRAIE**.


#### Syntaxe et fonctionnement

```bash
# Tant que la condition entre crochets est VRAIE
while [ condition ]; do
    # Exécuter ces commandes
    commande_1
    commande_2
done
```

#### Exemple : Le compte à rebours

Pour qu'une boucle `while` s'arrête, il faut absolument que quelque chose change à l'intérieur de la boucle (sinon, la condition restera toujours vraie !).

```bash
compteur=5

while [ $compteur -gt 0 ]; do     # Tant que compteur > 0
    echo "Explosion dans $compteur..."
    sleep 1                       # Attendre 1 seconde
    ((compteur--))                # Enlever 1 au compteur
done

echo "BOUM !"
```

> [!primary]
> ***Note** : `((...))` est une syntaxe spéciale pour faire des calculs mathématiques rapides en Bash.*



### 🟢 Exercice 2 (En classe)

**Objectif :** Créer un script interactif qui 

1. Créez un petit script nommé `voyage.sh` (avec `vim voyage.sh`).
2. Le script doit  :
   * demander à l'utilisateur de répondre à la question "Sommes nous arrivés ?", tant qu'il n'entre pas "oui".
3. Lancez le script.
4. Testez le avec les réponses: "non", "bientôt", "jamais". Le script devrait s'arrêter que si vous tapez "oui".

<!--
SOLUTION

```bash
#!/bin/bash
reponse="non"

# Tant que la réponse n'est pas "oui" (!= signifie "n'est pas égal à")
while [ "$reponse" != "oui" ]; do
    echo "Sommes nous arrivés ?"
    read reponse
done

echo "Enfin ! On est arrivés."
```
-->



### 3. La boucle infinie

Que se passe-t-il si la condition reste toujours **VRAIE** ? Le script ne s'arrêtera jamais. Le processeur va continuer à travailler indéfiniment.

C'est souvent une erreur de programmation (oublier de modifier la variable), mais c'est parfois voulu (pour un serveur web qui doit écouter 24h/24).

> [!warning]

> Ne lancez pas ce script sans lire la suite !
```bash
# Exemple de boucle infinie 
while true; do
    echo "Je tourne en rond..."
    sleep 1
done
```

> [!primary]
> **KIT DE SURVIE** (À mémoriser)
> Si vous lancez un script et qu'il part en boucle infinie (il défile sans s'arrêter) :
> Appuyez simultanément sur : **`Ctrl` + `C**`
> Cette combinaison envoie le signal `SIGINT` (Signal Interrupt) qui ordonne au programme de s'arrêter immédiatement.



### 🟢 Exercice 3 (En classe)

**Objectif :** Apprendre à tuer un processus fou.

1. Dans le terminal, tapez cette commande qui ne s'arrêtera jamais :
```bash
while true; do echo "Au secours !"; sleep 1; done
```
2. Regardez le texte défiler pendant quelques secondes.
3. Exécutez la combinaison de touches **`Ctrl` + `C**` pour reprendre le contrôle de votre terminal.



## Planification de tâches (Cron)

Imaginez que vous ayez un assistant personnel virtuel capable d'effectuer des tâches répétitives pour vous, même lorsque vous dormez. Sous Linux, cet assistant s'appelle le démon **Cron**.

**À quoi ça sert ?**

* Lancer des sauvegardes (backups) la nuit.
* Envoyer des rapports par courriel tous les lundis matin.
* Vider les fichiers temporaires toutes les heures.

### 1. L'édition de votre agenda (`crontab`)

Chaque utilisateur possède son propre fichier de planification, appelé la « crontab » (Cron Table).

Pour modifier votre agenda, utilisez la commande suivante :

```bash
crontab -e
```

*(Le `e` signifie "edit").*

> **Note importante :** Si c'est la première fois, Linux vous demandera de choisir un éditeur de texte. Choisissez **Vim**.



### 2. La syntaxe des 5 étoiles

C'est le cœur du système. Une tâche Cron est définie par une ligne unique composée de **5 champs temporels** suivis de la **commande**.

Il faut lire la ligne de gauche à droite. Pour qu'une commande se lance, **toutes** les conditions de temps doivent être validées.

| Position | Champ | Valeurs possibles | Ce que ça signifie |
| --- | --- | --- | --- |
| **1** | Minutes | `0` - `59` | À quelle minute ? |
| **2** | Heures | `0` - `23` | À quelle heure ? |
| **3** | Jour du mois | `1` - `31` | Le combien du mois ? |
| **4** | Mois | `1` - `12` | En quel mois ? |
| **5** | Jour semaine | `0` - `6` | Quel jour ? (0 = Dimanche) |

#### Les caractères spéciaux à connaître

* `*` (L'astérisque) : Signifie **"TOUS"** (ex: toutes les heures, tous les jours).
* `/` (Le slash) : Signifie **"CHAQUE"** (ex: `*/5` dans les minutes = toutes les 5 minutes).
* `,` (La virgule) : Pour faire une liste (ex: `1,15` = le 1er et le 15 du mois).



### 3. Exemples traduits en langage humain

Analysons ensemble ces exemples pour comprendre la logique :

* **L'événement quotidien :**
```bash
0 3 * * * /home/etudiant/backup.sh
```

> *« À la minute 0, de la 3ème heure, peu importe le jour du mois, peu importe le mois, peu importe le jour de la semaine. »*
> ➡ **Tous les jours à 03h00 du matin.**


* **Le début de semaine :**
```bash
0 8 * * 1 /home/etudiant/script.sh
```

> *« À 08h00 pile, n'importe quel jour du mois, mais OBLIGATOIREMENT si c'est le jour 1 (Lundi). »*
> ➡ **Tous les lundis à 08h00.**


* **La répétition fréquente :**
```bash
*/5 * * * * /home/etudiant/check_ping.sh
```

> *« Toutes les tranches de 5 minutes, à toutes les heures... »*
> ➡ **Toutes les 5 minutes (00:05, 00:10, 00:15, etc.).**




### 4. Les chemins absolus

C'est l'erreur qui fait échouer 99% des scripts débutants.

Lorsque Cron lance votre script, il ne connaît pas votre dossier actuel, ni vos raccourcis. Il est "aveugle".

* ❌ `python script.py` (Cron ne sait pas où est `script.py` ni où est `python`).
* ✅ `/usr/bin/python3 /home/etudiant/projets/script.py` (**Chemins absolus**).

> **Règle d'or :** Utilisez toujours le chemin complet, depuis la racine `/`. Aidez-vous de la commande `pwd` pour connaître le chemin de vos fichiers.



### 5. Où va la sortie ? (Les Logs)

Si votre script affiche un message (`print` ou `echo`), vous ne le verrez jamais car le script tourne en arrière-plan. Pour garder une trace ("Log"), on utilise les redirections.

```bash
# Syntaxe complète recommandée
* * * * * /home/user/mon_script.sh >> /home/user/journal.log 2>&1
```

* `>>` : Ajoute le résultat à la fin du fichier `journal.log` sans l'écraser.
* `2>&1` : Redirige aussi les **messages d'erreurs** (2) vers la même sortie standard (1). Très utile pour comprendre pourquoi un script a planté !



### 🟢 Exercice 1 (En classe)

Sans utiliser le terminal pour l'instant, écrivez la ligne Cron correspondant aux demandes suivantes :

1. Exécuter `script.sh` tous les mercredis à 14h30.
2. Exécuter `backup.sh` le 1er de chaque mois à minuit.
3. Exécuter `ping.sh` toutes les 15 minutes.

<!--
*(Correction rapide avec le professeur).* -->

### 🟢 Exercice 2 (En classe)

Nous allons créer une tâche qui laisse une trace chaque minute.

1. Ouvrez votre crontab : `crontab -e`
2. Ajoutez la ligne suivante à la fin du fichier (attention aux chemins !) :
```bash
* * * * * echo "Cron fonctionne : $(date)" >> /home/votre_nom/cron_test.txt
```


*(Remplacez `votre_nom` par votre vrai dossier utilisateur).*
3. Sauvegardez et quittez (Ctrl+X, Y, Enter).
4. Attendez une minute, puis vérifiez si le fichier se crée :
```bash
cat /home/votre_nom/cron_test.txt
```



### 🟢 Exercice 3 (En classe)

1. Créez un petit script `bonjour.sh` dans votre dossier personnel :
```bash
#!/bin/bash
echo "Bonjour, je suis un script !"
```


2. Rendez-le exécutable (`chmod +x bonjour.sh`).
3. Programmez-le dans Cron pour qu'il s'exécute dans 2 minutes (regardez l'heure actuelle avec `date`).
4. Redirigez la sortie vers un fichier `resultat.log`.
5. **Le piège :** Si le fichier `resultat.log` est vide ou n'existe pas, c'est probablement un problème de chemin. Déboguez !


<!--
SOLUTION

Voici le corrigé détaillé des exercices, accompagné de quelques **notes pédagogiques** pour vous aider à anticiper les questions ou les erreurs des étudiants.


### 🟢 Corrigé Exercice 1 : Le traducteur

L'objectif est de vérifier la compréhension de l'ordre des champs.

**1. Exécuter `script.sh` tous les mercredis à 14h30.**
`30 14 * * 3 /chemin/script.sh`

> **Note :** Rappelez-leur que Dimanche = 0, donc Lundi = 1, Mardi = 2, Mercredi = 3.

**2. Exécuter `backup.sh` le 1er de chaque mois à minuit.**
`0 0 1 * * /chemin/backup.sh`

> **Erreur fréquente :** Mettre des `*` partout. Insistez sur le fait que "Minuit" s'écrit `0 0` (0 minute, 0 heure).

**3. Exécuter `ping.sh` toutes les 15 minutes.**
`*/15 * * * * /chemin/ping.sh`

> **Alternative acceptée :** `0,15,30,45 * * * *` (C'est correct, mais plus long à écrire).
> **Erreur fréquente :** Écrire `15 * * * *`. Expliquez que cela signifie "À la 15ème minute de chaque heure" (donc une fois par heure), et non "toutes les 15 minutes".

---

### 🟡 Corrigé Exercice 2 : "Hello World" automatisé

Ici, il n'y a pas de "réponse" unique, mais un résultat attendu.

**Vérification de la réussite :**
L'étudiant a réussi si, après une minute, il tape la commande `ls -l` et voit le fichier `cron_test.txt` apparaître.
Ensuite, `cat cron_test.txt` doit afficher quelque chose comme :

```text
Cron fonctionne : Mer  7 jan 2026 10:15:01 EST
```

**Si ça ne marche pas :**

1. Le démon cron ne tourne pas (rare sous Linux Mint, mais possible).
2. L'étudiant a fait une faute de frappe dans le chemin du fichier de sortie (`>>`).
3. Il a oublié les astérisques `* * * * *` au début.



### 🔴 Corrigé Exercice 3 : Le défi du chemin absolu

C'est l'exercice qui discrimine ceux qui ont compris le concept de chemin absolu.

Supposons que l'étudiant s'appelle `martin` et que son script est dans `/home/martin/`.

**1. Le script `bonjour.sh**`

```bash
#!/bin/bash
echo "Bonjour, je suis un script !"

```

**2. Les permissions**
L'étudiant **DOIT** avoir fait :

```bash
chmod +x /home/martin/bonjour.sh

```

*Sans cela, Cron aura une erreur "Permission denied".*

**3. La ligne Crontab correcte**
Si il est 10h00, il programme pour 10h02 :

```bash
2 10 * * * /home/martin/bonjour.sh >> /home/martin/resultat.log 2>&1

```

**Les points de vigilance (pour la correction) :**

* ❌ `/home/martin/bonjour.sh > resultat.log` : **Erreur.** Cron ne sait pas où mettre `resultat.log`. Il va essayer de le mettre à la racine ou dans le dossier home de root, et échouer. Il faut le chemin complet pour le log aussi.
* ❌ `bonjour.sh` (sans `/home/...`) : **Erreur.** Cron ne trouvera pas le fichier.

-->

<!--

### 🟢 **Exercice (en classe)** : "Le décodeur cron"

*Au tableau, écrivez ces lignes et demandez quand elles s'exécutent :*

1. `30 8 * * *` -> Tous les jours à 8h30.
2. `0 0 1 * *` -> Le 1er de chaque mois à minuit.
3. `*/10 * * * *` -> Toutes les 10 minutes (0, 10, 20...).
4. `0 17 * * 5` -> Tous les vendredis à 17h00 (Le "Happy Hour" script).
-->

---

# Laboratoire

**Objectifs** :

1. Utiliser la boucle **`for`** pour traiter des fichiers en lot.
2. Utiliser la boucle **`while`** pour créer un script de surveillance continue.
3. Planifier l'exécution automatique de scripts avec **Cron**.

Utiliser le fichier **labo14.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo14.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}


## Partie 0 : Mise en place de l'environnement

Avant de commencer l'automatisation, nous devons créer un environnement de travail qui simule des fichiers de logs (journaux) générés par une application.

1. Ouvrez votre terminal.
2. Créez un dossier nommé `labo_automation` dans votre dossier personnel.
3. À l'intérieur, créez deux sous-dossiers : `logs` et `archives`.
4. Dans le dossier `logs`, créez 5 fichiers vides nommés `app1.log`, `app2.log`, `app3.log`, `system.log`, `error.log`.
* **Astuce** : Utilisez une commande unique avec `touch` et les accolades `{}`.*



## Partie 1 : Archivage avec la boucle `for`

Les fichiers de logs s'accumulent. Votre responsable vous demande un script pour les déplacer vers le dossier d'archives en leur ajoutant la date du jour, afin de libérer le dossier de logs actifs.


1. Créez un script nommé `archiveur.sh` à la racine de `labo_automation`.
2. N'oubliez pas le **Shebang** (`#!/bin/bash`) au début.
3. Le script doit :
   * Afficher le message "Début de l'archivage...".
   * Utiliser une boucle **`for`** pour parcourir tous les fichiers finissant par `.log` dans le dossier `logs/`.
   * Pour chaque fichier trouvé :
   * Déterminer le nom de base du fichier (ex: `app1.log`).
   * Déplacer ce fichier vers le dossier `archives/` en le renommant avec le format : `nom_original-DATE.bak`.
   * *Format de la date :* Utilisez `$(date +%F)` (Année-Mois-Jour).
   * Afficher un message : "Archivage de [fichier] terminé."
4. Rendez le script exécutable.
5. Testez le script. Vérifiez que le dossier `logs` est vide et que `archives` contient les fichiers renommés.



## Partie 2 : Surveillance avec la boucle `while` (30 min)

Vous devez surveiller la création de nouveaux fichiers dans le dossier `logs` en temps réel pendant une intervention de maintenance.


1. Créez un script nommé `moniteur.sh`.
2. Ce script doit utiliser une boucle **`while`** infinie (`true`).
3. À l'intérieur de la boucle :
   * Comptez le nombre de fichiers présents dans le dossier `logs`.
   * Affichez : "Surveillance active : X fichiers présents dans logs" (où X est le nombre).
   * Si le nombre de fichiers est égal à 0, affichez "Tout est propre.".
   * Si le nombre est supérieur à 0, affichez "Attention : Nouveaux logs détectés !".
   * Mettez le script en pause pendant **5 secondes** avant de recommencer la boucle (commande `sleep`).
4. Lancez ce script dans un terminal.
5. **Test :** Ouvrez un *deuxième terminal*. Créez un nouveau fichier dans `logs` (`touch logs/test.log`) et observez la réaction de votre script `moniteur.sh`.
6. Arrêtez le script avec `Ctrl + C`.



## Partie 3 : Automatisation avec Cron (20 min)

Votre script d'archivage fonctionne bien, mais vous ne voulez pas le lancer manuellement tous les jours. Vous allez le planifier.

1. Assurez-vous d'avoir remis quelques fichiers `.log` dans le dossier `logs` pour le test.
2. Ouvrez l'éditeur de tâches cron (`crontab -e`).
3. Ajoutez une tâche planifiée qui exécute votre script `archiveur.sh` **toutes les minutes**.
   * *Note importante :* Cron ne voit pas votre écran. Pour vérifier que cela fonctionne, vous devez rediriger la sortie du script vers un fichier journal, par exemple : `>> /home/votre_user/labo_automation/cron_history.txt 2>&1`.
4. Sauvegardez et quittez.
5. Attendez 2 minutes.
6. Vérifiez :
   * Si les fichiers dans `logs` ont disparu.
   * Si le fichier `cron_history.txt` contient bien les messages "Début de l'archivage...".



## Partie 4 : Nettoyage

Après avoir pris **toutes** vos captures d'écran :

1. Modifiez le `crontab` pour supprimer la tâche planifiée (pour éviter de remplir votre disque dur inutilement après le cours !).
2. Modifiez le script `archiveur.sh` pour qu'il vérifie d'abord si le dossier `logs` est vide. S'il est vide, il doit afficher "Rien à archiver" et s'arrêter.

## Corrigé du laboratoire

> À venir (samedi ou dimanche)


<!--

# Solution 

Voici les commandes et scripts attendus pour la correction.

### Partie 0 : Mise en place

```bash
mkdir -p ~/labo_automation/logs ~/labo_automation/archives
cd ~/labo_automation
touch logs/{app1,app2,app3,system,error}.log
```

### Partie 1 : Le script `archiveur.sh`

```bash
#!/bin/bash

# Définition des dossiers (Chemin absolu recommandé pour cron, mais relatif ok pour test)
# On utilise $HOME pour que cela fonctionne peu importe l'utilisateur
SOURCE="$HOME/labo_automation/logs"
DEST="$HOME/labo_automation/archives"
DATE_JOUR=$(date +%F-%H%M) # Ajout heure/minute pour bien voir les différences en test

echo "--- Début de l'archivage : $(date) ---"

# Vérification si des fichiers existent (Bonus Partie 4 inclus ici pour robustesse)
# On regarde si ls trouve des fichiers .log
if ! ls $SOURCE/*.log >/dev/null 2>&1; then
    echo "Aucun fichier .log trouvé dans $SOURCE."
    exit 0
fi

# La boucle FOR
for fichier in "$SOURCE"/*.log; do
    # Extraction du nom de fichier sans le chemin
    nom_base=$(basename "$fichier")
    
    # Déplacement et renommage
    mv "$fichier" "$DEST/$nom_base-$DATE_JOUR.bak"
    
    echo "Archivage de $nom_base terminé."
done

echo "Opération terminée."
```

**Pour tester :**

```bash
chmod +x archiveur.sh
./archiveur.sh
ls archives/
```

### Partie 2 : Le script `moniteur.sh`

```bash
#!/bin/bash

DOSSIER="$HOME/labo_automation/logs"

echo "Démarrage de la surveillance sur $DOSSIER..."
echo "Appuyez sur Ctrl+C pour arrêter."

# La boucle WHILE infinie
while true; do
    # Compter les fichiers (ls | wc -l)
    # On vérifie si le dossier existe pour éviter les erreurs
    if [ -d "$DOSSIER" ]; then
        nombre=$(ls -1 "$DOSSIER" | wc -l)
        
        echo "---------------------------------"
        echo "Heure : $(date +%H:%M:%S)"
        echo "Surveillance active : $nombre fichiers présents."
        
        if [ "$nombre" -eq 0 ]; then
            echo "Statut : Tout est propre."
        else
            echo "ALERTE : Nouveaux logs détectés !"
        fi
    else
        echo "Erreur : Le dossier $DOSSIER n'existe pas."
    fi

    # Pause de 5 secondes
    sleep 5
done
```

**Pour tester :**

```bash
chmod +x moniteur.sh
./moniteur.sh
# Dans un autre terminal : touch ~/labo_automation/logs/test.log
```

### Partie 3 : La planification Cron

Commande pour éditer :

```bash
crontab -e
```

Ligne à ajouter (remplacez `votre_user` par votre vrai nom d'utilisateur, ex: `etudiant`) :

```cron
# M h  dom mon dow   command
* * * * * /home/votre_user/labo_automation/archiveur.sh >> /home/votre_user/labo_automation/cron_history.txt 2>&1

```

*Explication de la syntaxe :* `* * * * *` signifie "Chaque minute".
*Explication de la redirection :* `>>` ajoute la sortie à la fin du fichier texte. `2>&1` redirige aussi les erreurs vers ce même fichier.

### Nettoyage final

Pour arrêter le cron, tapez `crontab -e` et supprimez la ligne ou ajoutez un `#` au début pour la commenter.

```bash
# * * * * * /home/... (Ligne désactivée)
```
-->