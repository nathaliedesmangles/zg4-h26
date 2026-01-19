+++
pre = 'Semaine 7 : '
title = 'Gestion des processus'
weight = 70
+++

## Objectif de la semaine

* Comprendre comment Linux gère les programmes, apprendre à les surveiller, les mettre en pause, et les arrêter proprement (ou brutalement)


**Fichier pour les exercices (en classe)**
Utiliser le fichier **exo-semaine7.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine7.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---


# Théorie


## Qu'est-ce qu'un processus ?

Imaginez un livre de cuisine.

* Le **Programme** (fichier sur le disque), c'est la **recette** écrite dans le livre. Elle est inerte.
* Le **Processus**, c'est l'action de **cuisiner** cette recette. C'est vivant, ça utilise des ingrédients (RAM) et du temps de chef (CPU).

Dès que vous lancez un programme, Linux lui attribue un badge d'identification unique : le **PID** (Process ID).

> **Note importante :** Si un programme plante, Linux continue de tourner. Il suffit de retirer le badge (tuer le PID) pour nettoyer la situation.


## Surveillance : L'art de savoir ce qui tourne

Pour agir, il faut d'abord voir. Linux nous offre trois niveaux de vision.

### 1. L'arbre généalogique : `pstree`

Les processus ne naissent pas de nulle part ; ils sont lancés par des "parents".

* **Commande :** `pstree`
* **Ce qu'on voit :** L'arbre généalogique. Vous verrez que `systemd` (PID 1) est l'ancêtre commun de tous.

### 2. La photo : `ps`

C'est une "photo instantanée" du système.

* **La commande reine :** `ps aux`
* `a` : Tous les utilisateurs (pas juste vous).
* `u` : Affiche les détails utiles (Utilisateur, CPU%, RAM%).
* `x` : Affiche aussi les processus "invisibles" (services d'arrière-plan).


* **L'astuce du détective :**
Souvent, la liste est trop longue. On utilise `grep` pour filtrer :
```bash
ps aux | grep firefox
```

### 3. En temps réel (Vidéo) : `htop`

C'est le "Gestionnaire des tâches" dynamique.

* **Commande :** `htop`
* **Navigation :**
* Utilisez les flèches pour choisir un processus.
* `F9` pour tuer, `F10` (ou `q`) pour quitter.


### 🟢 Exercice 1 (En classe)

**Objectif :** Comprendre les colonnes de `ps aux`.

Soit cette fausse ligne de résultat `ps`:

```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 168664 13084 ?        Ss   09:21   0:04 /sbin/init
etudiant  4021 95.0  2.5 500200 50000 pts/0    R+   10:00   5:00 ./minage_crypto
```

1. Quel processus est en train de faire surchauffer le processeur ?
2. Est-ce que ce processus appartient à l'administrateur ?
3. Le processus PID 1 a-t-il un terminal attaché (TTY) ?

<!--
**Réponses attendues :**

1. `./minage_crypto` (car %CPU est à 95.0).
2. Non, il appartient à l'utilisateur `etudiant`.
3. Non, il y a un `?` sous TTY (c'est un processus système/démon lancé au démarrage).
-->

## Arrêter des processus

Quand un processus ne répond plus, on ne redémarre pas le PC. On lui envoie un **Signal**.

### 1. Les deux armes principales

| Commande | Analogie | Description |
| --- | --- | --- |
| **`kill [PID]`** | Le Sniper | Vise un processus précis par son numéro. <br>

<br>Ex: `kill 4512` |
| **`killall [Nom]`** | La Grenade | Vise tous les processus ayant ce nom. <br>

<br>Ex: `killall firefox` (Ferme toutes les fenêtres). |

### 2. La force du signal

Par défaut, `kill` est gentil. Mais on peut être méchant.

a. **Niveau 1 : La demande polie (SIGTERM -15)**
* C'est le défaut (`kill 1234`).
* *Signification :* "S'il te plaît, sauvegarde ton travail et ferme-toi."
* Le processus peut refuser s'il est planté.


b. **Niveau 2 : L'arrêt forcé (SIGKILL -9)**
* Commande : `kill -9 1234`
* *Signification :* "La police t'expulse immédiatement."
* Le processus disparaît instantanément sans sauvegarder. **À utiliser en dernier recours.**

### 🟢 Exercice 2 (En classe)

**Objectif :** Choisir le bon signal (`SIGTERM` vs `SIGKILL`).

> "Votre navigateur Firefox (PID 345) est gelé. Il consomme trop de RAM, mais vous aviez des onglets importants ouverts."

1. Quelle est la **première** commande à tenter ?
2. Si elle ne fonctionne pas après 10 secondes, quelle est la commande de la **dernière chance** ?

<!--
**Réponse attendue :**

1. `kill 345` (ou `kill -15 345`). *Pourquoi ? Pour lui laisser une chance de sauvegarder l'historique.*
2. `kill -9 345`. *Pourquoi ? C'est l'arrêt brutal, on perd les données non sauvegardées.*
-->


## Processus en avant-plan et en arrière-plan (Multitâche)

Par défaut, si vous lancez une commande longue (ex: une copie de gros fichiers), votre terminal est bloqué. Vous ne pouvez plus rien taper tant que ce n'est pas fini.

### 1. La solution : processus en background (`&`)

Pour lancer une commande directement en arrière-plan, ajoutez une esperluette `&` à la fin.

```bash
sleep 1000 &
# Le terminal vous rend la main tout de suite !
```

### 2. La méthode "Oups, j'ai oublié" (Gestion des jobs)

Vous avez lancé une commande et elle bloque votre terminal ? Pas de panique.

1. **Mettre en pause :** Faites `Ctrl + Z`. Le processus est figé (Stopped).
2. **Envoyer au fond :** Tapez `bg` (Background). Le processus reprend son travail, mais en arrière-plan.
3. **Vérifier :** Tapez `jobs` pour voir vos tâches de fond.
4. **Récupérer :** Tapez `fg` (Foreground) pour ramener le processus devant vous.



## La persistance des processus (Survivre à la fermeture)

Si vous fermez votre terminal (ou si votre connexion SSH coupe), tous vos processus enfants meurent. C'est la chaîne de commandement : si le chef part, l'équipe est dissoute.

**La solution préventive : `nohup**`
`nohup` (*No Hang Up*) immunise le processus contre la fermeture du terminal.

```bash
nohup python mon_script_long.py &
```

* Le script continuera de tourner même si vous fermez la fenêtre.
* Les messages (logs) seront écrits dans un fichier `nohup.out`.

### 🟢 Exercice 3 (En classe)

**Objectif :** Mémoriser la séquence de mise en arrière-plan.

> "Vous lancez la commande `gedit mon_fichier.txt` et zut ! Le terminal est bloqué. Vous ne pouvez plus taper de commandes."

Donnez moi la combinaison de touches et la commande pour récupérer le terminal **sans fermer** l'éditeur de texte.

<!--
**Réponse attendue :**

1. **`Ctrl + Z`** (Pour mettre en pause/stop).
2. **`bg`** (Pour relancer en background).
-->

---

# Laboratoire

Utiliser le fichier **labo7.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo7.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}


Vous êtes administrateur système. Vous devez gérer plusieurs tâches simultanées et nettoyer des processus récalcitrants sans redémarrer la machine.

### Exercice 1 : Gestion des jobs

*Objectif : Maîtriser le Ctrl+Z, bg et fg.*

1. Lancez une commande qui ne fait rien pendant 500 secondes :
```bash
sleep 500
```


*(Constatez que votre terminal est bloqué).*
2. Utilisez le raccourci clavier pour mettre le processus en **Pause**.
3. Vérifiez l'état avec la commande `jobs`.
4. Relancez le processus en **Arrière-plan** (sans bloquer le terminal).
5. Ramenez le processus au **Premier plan**, puis annulez-le définitivement avec `Ctrl + C`.


### Exercice 2 : La surveillance

*Objectif : Trouver un PID spécifique.*

1. Lancez un éditeur de texte en arrière-plan (pour qu'il ne bloque pas le terminal) :
```bash
nano &
```

*(Note : nano va se mettre en pause car il attend une saisie, c'est normal).*
2. Utilisez `ps aux` combiné avec un "pipe" `|` et `grep` pour trouver la ligne correspondant à `nano`.
3. Notez le **PID** de nano (le numéro dans la 2ème colonne).


### Exercice 3 : Les signaux

*Objectif : Tuer proprement puis salement.*

1. Relancez trois processus "sommeil" en arrière-plan :
```bash
sleep 2000 &
sleep 3000 &
sleep 4000 &
```

2. Affichez la liste avec `jobs`.
3. Tuez le dernier `sleep` en utilisant `kill` et son **PID** (trouvé via `ps` ou affiché lors du lancement).
4. Utilisez la méthode "Grenade" pour tuer tous les processus `sleep` restants d'un seul coup :
```bash
killall sleep
```

5. Vérifiez avec `jobs` qu'il ne reste plus rien.


### Exercice 4 : La persistance

*Objectif : Lancer un processus qui survit à la fermeture.*

1. Lancez un processus avec `nohup` :
```bash
nohup sleep 500 &
```

2. Fermez complètement votre terminal (ou déconnectez vous).
3. Rouvrez un nouveau terminal.
4. Vérifiez si le processus tourne toujours. Comme ce n'est plus votre enfant "direct" (votre ancien terminal est mort), `jobs` ne le verra pas. Il faut utiliser le radar global :
```bash
ps aux | grep sleep
```

5. Bravo ! Vous avez créé un processus indépendant. Tuez-le pour nettoyer le système.

---

## Corrigé du laboratoire

> À venir (samedi ou dimanche)
