
# **PROJET : AUDIT DE ROBUSTESSE DES MOTS DE PASSE**

# Sommaire
- [**Présentation du projet**](#le-projet)
- [**John The Ripper et HashCat c'est quoi ?**](#john-the-ripper-et-hashcat-cest-quoi-)
- [**Les membres du groupes**](#les-membres-du-groupe-et-leurs-rôles)
- [**Choix Techniques**](#choix-techniques)
- [**Logiciels**](#logiciels)
- [**Difficultés et solutions rencontrées**](#difficultés-et-solutions-rencontrées)
- [**Améliorations possibles**](#améliorations-possibles)

# Le projet

Le projet réalisé permet de tester la vulnerabilitée des mots de passe grâce aux logiciels [_**John The Ripper**_](https://www.openwall.com/john/) et [_**HashCat**_](https://hashcat.net/hashcat/). L'attaque avec **John The Ripper** s'effectuera d'une machine Linux à une machine Windows 11. Tandis que l'attaque avec **HashCat** s'effectuera d'un serveur Debian à un serveur Windows 2025.

Les attaques seront commises par *dictionnaire* et par *force brut* avec **John The Ripper**.
Concernant **HashCat** , nous utiliserons des masques d'attaque.

L'objectif final est de réaliser un audit sur la robustesse des mots de passe de nos fichiers .zip.

Voici le schéma du lab sur lequel nous travaillerons. 

![LAB](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Projet1-G1.drawio%20(1).png)

# John The Ripper et HashCat c'est quoi ?

- **Utilité**

Ce sont des logiciels de cassage de mot de passe, afin de réaliser des audits en testant l'efficacité de ceux-ci.

- **Possibilités**

Pour casser les mots de passe, plusieurs possibilités s'offrent à nous. 
Le mode simple, permet d'essayer des mots de passes en détournant le nom utilisateur.
Le mode dictionnaire, utilise une **wordlist** téléchargeable , puis y ajoute le nom utilisateur pour plus de possibilités.
Le mode force brute, essaye toutes les combinaisons de caractères possible jusqu'à atteindre son but.


# Les membres du groupe et leurs rôles


|             |                                                                        **Sprint 1**                                                                        |                                 **Sprint 2**                                 |
| :---------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------: |
| **Jeremy**  |                                   Recherche sur HashCat. Préparation de l'installation du programme et de l'utilisation.                                   |   _Product owner_: Maintenir la direction du projet. Livraison de l'audit.   |
| **Zishan**  | _Product Owner_:  Maintenir la direction du projet. Remplir le backlog. Installation des logiciels, prise de screens, test. Recherche sur John The Ripper. |                  Rédaction de INSTALL.md avec les screens.                   |
|  **Brice**  |   _Scrum Master_: Assurer la coordination et les bonnes conditions de travail. Rédaction du README.md suivant l'avancé du projet. Recherche sur HashCat.   |                          Rédaction de USERGUID.md.                           |
| **Gregory** |                                     Installation des logiciels, prise de screens, test. Recherche sur John The Ripper.                                     | _Scrum Master_: Assurer la coordination et les bonnes conditions de travail. |
| **Commun**  |                                     Installation de toutes les VM et des logiciels pour pouvoir tester en même temps.                                      |                       Test du cassage de mot de passe                        |


# Choix techniques 

Pour mener à bien notre projet, nous avons utilisé :

**Machines clients** : 

_______________________________________
- *VM 1* : WIN01 - CLIENT WINDOWS 11 
- *OS* : Windows 11
- *Compte & Mot de passe* : Wilder, Azerty1*
- *Adresse Ip* : 172.16.10.10
- *Masque* : 255.255.255.0
_______________________________________
- *VM 2* :  UBU01 - CLIENT UBUNTU 
- *OS* : Ubuntu 24.04
- *Compte & Mot de passe* : wilder, Azerty1*
- *Adresse Ip* : 172.16.10.20
- *Masque* : 255.255.255.0
_______________________________________
- *VM 3* :  SRVWIN01 - WindowsServer
- *OS* : Windows Server 2025 GUI
- *Compte & Mot de passe* : Administrator, Azerty1*
- *Adresse Ip* : 172.16.10.5
- *Masque* : 255.255.255.0
_______________________________________
- *VM 4* : SRVLX01 - DebianServer 
- *OS* : Debian 13 CLI
- *Compte & Mot de passe* : Root, Azerty1*
- *Adresse Ip* : 172.16.10.6
- *Masque* : 255.255.255.0
_______________________________________
# Logiciels

- *John The Ripper v 1.9.0* : [Lien de Téléchargement](https://www.openwall.com/john/) , [Lien de documentation](https://www.openwall.com/john/doc/)
- *HashCat v 7.1.2* : [Lien de Téléchargement](https://hashcat.net/hashcat/) , [Lien de documentation](https://github.com/hashcat/hashcat-utils/tree/master/docs)


## **Difficultés et solutions rencontrées:**

| **Difficultées**                                           |                                                       **Solutions** |
| :--------------------------------------------------------- | ------------------------------------------------------------------: |
| Logiciel peu efficace avec le paquet apt                   |                                    Installation du paquet avec snap |
| Capacité  de base limitée en liste de mots                 |            Installation d'un paquet de liste supplémentaire openssl |
| Erreur "Signature Unmatched" lors du décryptage du hash.   |    Remise au propre du hash grâce a la commande " cut -d ':' -f2 ". |
| Accéder aux dossier du serveur par la machine qui attaque. | Utilisation des protocoles CIPS pour faciliter l'accès au dossier . |




# Améliorations possibles

**N'utilisez pas ces logiciels de façon malveillante !** 
