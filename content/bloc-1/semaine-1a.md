+++
pre = 'Semaine 1.1: '
title = 'Présentation du cours et introduction à Linux'
weight = 11
+++


### Objectif de la séance

* Connaitre les objectifs du cours et les règlements à respecter.
* Comprendre ce qu'est Linux et pourquoi on l'utilise.


---

# Présentation du cours

> [!primary]
> 1. Voir le plan de cours (sur Moodle et/ou ColNet)  
> 2. Voir les règlements [**ICI**](../regles/)

---

# Théorie


## Qu'est-ce que Linux ?

Ce n'est pas juste "un autre Windows". C'est l'OS qui fait tourner Internet, le Cloud, Android et les supercalculateurs. Votre routeur Wi-Fi, c'est Linux. Si vous voulez travailler en TI, vous ne pouvez pas éviter ce pingouin.

> En date de décembre 2025, **"96,3 %** du top 1 million des serveurs web tournent sous Linux. 


* **Philosophie Open Source :**
* **Windows/macOS :** Code fermé (propriétaire). Vous louez le logiciel, vous ne pouvez pas voir comment il est fait.
* **Linux :** Code ouvert. C'est comme une recette de cuisine publique : tout le monde peut la copier, la modifier et la redistribuer .
* **Analogie :** Windows, c'est un restaurant (on mange, on paie, on ne va pas en cuisine). Linux, c'est un "Potluck" communautaire (on apporte, on modifie, on partage).



## Architecture : Le moteur et la carrosserie

Il faut distinguer le **Noyau** du **Système d'exploitation** .

* **Le Kernel (Noyau) :** C'est le chef d'orchestre. Il parle directement au matériel (CPU, RAM, Disque). C'est le "moteur".
* **Le Shell (Coquille) :** L'interface textuelle qui permet à l'humain de parler au Noyau. C'est le "volant".
* **La Distribution (Distro) :** C'est le package complet (Moteur + Volant + Sièges en cuir + Climatisation).
	* **Ubuntu** (Grand public), 
	* **RedHat** (Entreprise), 
	* **Kali** (Sécurité), 
	* **Mint** (stable avec une interface proche de Windows).
	* **AlmaLinux** (<span style="color:blue;"><b>Notre choix</b></span>).




## La virtualisation (cours 420-ZE5-MO)

Pourquoi on n'installe pas Linux directement sur vos portables aujourd'hui ? Pour éviter la catastrophe.

* **Concept :** Un ordinateur "fantôme" qui tourne à l'intérieur de votre vrai ordinateur .
* **Vocabulaire technique :**
	* **Hôte (Host) :** Votre machine physique (Windows/Mac).
	* **Invité (Guest) :** La machine virtuelle (Linux Mint).
	* **Hyperviseur :** Le logiciel qui fait le pont (VirtualBox).
* **Avantage majeur :** Le "Sandbox" (Bac à sable). Si vous brisez Linux, vous supprimez juste un fichier. Votre système natif reste intact.





