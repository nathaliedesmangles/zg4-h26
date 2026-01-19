+++
pre = 'Semaine 11 : '
title = 'Réseau et services & Examen #3'
weight = 110
+++


## Objectif de la semaine

* Comprendre comment une machine Linux communique, comment gérer les programmes qui tournent en arrière-plan (services) et comment sécuriser ces échanges.

**Fichier pour les exercices (en classe)**
Utiliser le fichier **exo-semaine11.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Exercices/exo-semaine11.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

---

> [!warning]

> **Cette semaine est critique** : on touche à l'infrastructure physique. C'est l'une des rares opérations où une erreur de frappe peut détruire des données de manière irréversible. Le ton doit être méthodique.

---

# Théorie

## Interfaces et connexions

**Analogie** : Avant de passer un appel, il faut vérifier si le téléphone est branché et connaître son propre numéro.

La commande `ifconfig` est obsolète. Aujourd'hui, nous utilisons la suite **iproute2** (commandes commençant par `ip`).

### 1. Identifier ses cartes (Le matériel)

Chaque carte réseau possède un nom unique.
Commande : `ip link show`

* **`lo` (Loopback) :** L'adresse de bouclage (127.0.0.1). C'est l'ordinateur qui se parle à lui-même. *Règle d'or : On ne touche jamais à `lo`.*
* **`eth0` / `enp3s0` :** La carte Ethernet (câble).
* **`wlan0` / `wlp2s0` :** La carte Wi-Fi.

### 2. Voir les adresses (L'identité)

Pour savoir "qui" vous êtes sur le réseau.
Commande : `ip addr show`

* Cherchez la ligne `inet` pour voir votre IPv4 (ex: `192.168.1.50`).
* Cherchez le mot `UP` dans la première ligne pour savoir si la carte est active.

### 3. Éteindre et allumer

Parfois, pour relancer une connexion buggée, il faut virtuellement "débrancher le câble".

```bash
sudo ip link set enp3s0 down  # Éteindre
sudo ip link set enp3s0 up    # Rallumer
```

### 🟢 Exercice #1 (En classe)

1. Lancez `ip addr show`.
2. Quelle est votre adresse IP (inet) ?
3. Quel est le nom de votre interface principale (eth0, enp..., wlan...) ?
4. Votre interface est-elle marquée comme `UP` ou `DOWN` ?


## Le carnet d'adresses : DNS et Hosts

Les ordinateurs préfèrent les chiffres (IP), les humains préfèrent les noms (google.com).

### 1. `/etc/hosts` : Le post-it local

C'est un fichier texte simple qui force une correspondance Nom <-> IP. Linux regarde ce fichier **avant** de demander à Internet.

* *Utilité :* Bloquer des sites ou nommer vos propres serveurs locaux.

### 2. `/etc/resolv.conf` : L'annuaire (DNS)

C'est ici que votre ordinateur sait à qui demander le chemin quand il ne connaît pas une adresse. Il contient les adresses des serveurs DNS (ex: `8.8.8.8` pour Google).



## Le gestionnaire : NetworkManager

Sur les distributions modernes (comme Mint ou Ubuntu), on ne configure plus tout à la main. On utilise un "chef de chantier" : **NetworkManager**.

* **L'outil en ligne de commande :** `nmcli`
   * `nmcli general status` : Tout va bien ?
   * `nmcli connection show` : Quelles sont mes connexions ?


* **L'outil visuel :** `nmtui`
   * Tapez `nmtui` dans le terminal.
   * Vous obtenez une interface graphique... en texte ! Idéal pour configurer le Wi-Fi sans souris.



## Le chef d'orchestre : Systemd

Un serveur Web (Apache) ou SSH tourne en arrière-plan sans fenêtre ouverte. On appelle cela un **Service** (ou Démon). Le programme qui les gère s'appelle **systemd**.

**Analogie : Le concierge d'hôtel**
Vous ne parlez pas aux employés, vous donnez vos ordres au concierge (Systemd).

1. **Immédiat ("Fais-le maintenant") :**  
`sudo systemctl start apache2` (Démarre le service)  
`sudo systemctl stop apache2` (Arrête le service)  
2. **Futur ("Fais-le à chaque démarrage") :**  
`sudo systemctl enable apache2` (Lance le service automatiquement au boot)  
`sudo systemctl disable apache2` (Empêche le lancement automatique)  
3. **L'inspection :**  
`sudo systemctl status apache2` (Le service est-il actif ? A-t-il planté ?)  


### 🟢 Exercice #2 (En classe)

Le service `cron` gère les tâches planifiées et est présent sur tous les Linux.
1. Vérifiez le statut de cron : `systemctl status cron`.
2. Est-il `active (running)` ?
3. Regardez la ligne "Loaded" : est-il `enabled` (activé au démarrage) ?
 


## Sécurité : SSH et Pare-feu (UFW)

### 1. SSH (Secure Shell)

Le cordon ombilical pour administrer un serveur à distance.
`ssh utilisateur@192.168.1.50`

### 2. UFW (Uncomplicated Firewall)

Le pare-feu est votre portier. Par défaut, il devrait refuser tout le monde, sauf ceux qui sont sur la liste d'invités.

```bash
sudo ufw status verbose # Voir l'état
sudo ufw enable         # Activer le douanier
sudo ufw allow ssh      # Ouvrir le port 22 (IMPORTANT sinon vous êtes bloqué dehors !)
sudo ufw allow http     # Ouvrir le port 80 (Web)
```

### 🟢 Exercice #3 (En classe)

1. Tapez `sudo ufw status`.
2. Si c'est "inactif", ne l'activez pas encore.
3. Listez les applications connues par le pare-feu avec : `sudo ufw app list`. Voyez-vous OpenSSH ?

---

# Laboratoire 

Utiliser le fichier **labo11.docx** pour y mettre vos réponses et captures d'écran.  
{{% button href="/docs/Labos/labo11.docx" icon="download" %}}Télécharger le fichier docx{{% /button %}}

Vous êtes administrateur système. On vous demande de préparer un petit serveur web qui doit démarrer automatiquement et être protégé par un pare-feu.

### Étape 1 : Reconnaissance réseau

1. Identifiez l'adresse IP de votre machine. Notez-la.
2. Utilisez `ping` pour vérifier que vous avez accès à internet (ex: `ping -c 3 google.com`).

### Étape 2 : Manipulation de l'annuaire local

1. Ouvrez le fichier `/etc/hosts` avec `nano` (en sudo).
2. Ajoutez une ligne à la fin : `127.0.0.1 monsite.local`.
3. Enregistrez et quittez.
4. Testez avec la commande `ping -c 3 monsite.local`. (Cela doit répondre depuis 127.0.0.1).

### Étape 3 : Gestion du service Web

*Note : Si Apache n'est pas installé, installez-le : `sudo apt install apache2 -y*`

1. Demandez au "Concierge" (`systemctl`) si Apache est démarré.
2. S'il est éteint, démarrez-le.
3. Assurez-vous qu'il redémarrera automatiquement si on reboote le serveur (`enable`).
4. Vérifiez que le serveur fonctionne : ouvrez votre navigateur et tapez votre adresse IP (trouvée à l'étape 1) ou `localhost`. Vous devriez voir la page "It Works!".

### Étape 4 : Sécurisation 

1. Vérifiez le statut actuel de UFW.
2. Ajoutez une règle pour autoriser le trafic Web (Port 80 ou "Apache").
3. Ajoutez une règle pour autoriser le SSH (Port 22 ou "OpenSSH") *très important*.
4. Activez le pare-feu (`enable`).
5. Affichez le statut final numéroté (`sudo ufw status numbered`).

### Validation

Appelez le professeur pour montrer :

1. Votre page web qui s'affiche.
2. Le résultat de la commande `systemctl status apache2`.
3. Le résultat de `sudo ufw status` montrant que le pare-feu est actif et filtre les ports.

---

## Corrigé du laboratoire

> À venir (samedi ou dimanche)

