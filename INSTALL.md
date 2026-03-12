# Guide d'installation des logiciel John The Ripper et HashCat

Vous êtes en possession d'un dossier crypté dont vous avez oublié le mot de passe ou vous êtes en entreprise et devez tester la force des mots de passe de votre service ?
Les logiciels John The Ripper et HashCat sont des logiciels open source qui permettent de réaliser vos tests de façon rapide et simple afin d'assurer votre sécurité et celle de vos collaborateurs. 

# Prérequis technique :


Ces deux logiciels fonctionnent de façon similaire sur différents OS. Dans ce guide nous verrons les étapes d'installations des logiciels sur un système **Debian 13** et **Ubuntu 24.04.4**.
Il n'y a aucune contrainte technique concernant l'utilisation des logiciels, mise à part la connaissance et le terminal des systèmes Ubuntu et Debian.

# Installation d'un dossier partagé sur Windows et Windows serveur.

Les machines et les serveurs étant sur le même réseau, nous allons devoir créer un dossier partagé sur notre machine Windows et sur notre Windows serveur.

## Windows Serveur.

- Créez depuis la session Administrator un nouvel utilisateur et le mettre dans le groupe admin .
- Quittez la session Administrator et connectez vous à la session du nouvel utilisateur, ici *Wilder*.
- Créez un dossier , ici *audit* , dans le disque C:/
- Créez un fichier dans ce dossier, ici *fichier2* , le compresser en zip et mettre un mot de passe. 
- Allez dans les propriétés du dossier pour le partager *à tout le monde*.
- Pour la suite, il faut retenir : Le **nom d'utilisateur** , le **mot de passe utilisateur**, le **nom du dossier partagé** et l'**adresse IP** du serveur. 
## Windows 10/11

- Créez un dossier , ici *audit* , dans le disque C:/
- Créez un fichier dans ce dossier, ici *fichier1* , le compresser en zip et mettre un mot de passe. 
- Allez dans les propriétés du dossier pour le partager *à tout le monde*.
- Pour la suite, il faut retenir : Le **nom d'utilisateur** , le **mot de passe utilisateur**, le **nom du dossier partagé** et l'**adresse IP** du serveur. 

## Installation de John The Ripper sur Ubuntu.

**Installer les protocoles cifs**

``` bash 
sudo apt install cifs-utils -y
```

![Screen cifs](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Protocole_cifs.png)

John The Ripper peut être installer directement un utilisant apt ou snap.
Nous avons rencontré plusieurs dysfonctionnements en l'installant avec la commande apt.
Nous vous recommandons donc de procéder avec l'installation snap. Le format snap permet l'installation de logiciels séparés du reste du système d'exploitation grâce a des mécanismes de sécurité.

**Installer Snap**

``` bash
sudo apt update
sudo apt install snap
```

![Screen Snap](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Install_Snap.png)

**Installer John The Ripper**

``` bash 
sudo snap install john-the-ripper
```

![Screen JTR](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Install_JohnTheRipper.png)

Tapez maintenant John dans votre terminal, il vous affichera ceci :

![Screen John](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/John.png)

Nous pouvons voir la version du logiciel (ici 1.9.0). La ligne **usage** montre que nous pouvons fournir à John un fichier contenant une liste de mots de passe ainsi que choisir l'option avec laquelle il va tenter de déchiffrer les fichiers que nous lui indiquerons.

Voici quelques exemples d'options envisageables pour John :

1. **--single** : mode par défaut, John tente des combinaisons variables en fonction du nom de l'utilisateur.
2. **--wordlist** : l'attaque par dictionnaire. John utilise une liste de mots de passe fournis afin de craquer le mot de passe du fichier désigné.
3. **--incremental** : brut force, John teste toutes les combinaisons de caractères jusqu’à trouver le mot de passe. Infaillible en théorie mais peut prendre du temps en fonction de la force du mot de passe du fichier sélectionné.


## Installation de HashCat sur Debian

**Installer les protocoles cifs**

``` bash
apt install cifs-utils -y
```

![Protocole Cifs](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Protocole_cifs_debian1.png)

**Installer HashCat**

``` bash
apt install hashcat
```

Plusieurs lignes de commande s'affichent, pour vérifier la version tapez :

``` bash
hashcat --version
```

![Version HashCat](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Hashcat1.png)

**Installer John The Ripper pour avoir zip2john**

Sur notre serveur Debian, l'installation par **snap** ne fonctionnera pas. Pour installer John The Ripper , vous aurez besoin d'installer quelques dépendances : 

``` bash
apt install git build-essential libssl-dev zlib1g-dev yasm pkg-config
```

Lorsque tout est bien installé, nous allons cloner sur GitHub le logiciel John The Ripper.

``` bash
git clone https://github.com/openwall/john.git
```

![JTR Clone](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/JTR_Debian1.png)

Nous avons maintenant besoin de compiler ce que nous venons de cloner.

``` bash 
cd john/src
./configure
make -s clean && make -sj? (? est le nombre de processeur disponible)
```

Vous devriez avoir un message qui confirme la fin du make :

![MakeFinish](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/Make_completed1.png)

L'outil zip2john sera dans jonh/run/ 

``` bash
ls ../run/zip2john
```

