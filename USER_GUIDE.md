
# Guide d'utilisation HashCat

## HashCat

C'est un logiciel open source , exploitant la puissance des GPU pour déchiffrer des mots de passe cryptés.

## Utilisation de base 

Pour utiliser **HashCat**, il nous faut un dossier compressé et sécurisé qui sera pour notre projet en .zip sur une machine Windows 10.

Nous aurons besoin d'installer également [_**John The Ripper**_](https://www.openwall.com/john/) pour récupérer zip2john, et les protocoles cifs.

Grâce aux paquets cifs et à l'IP de la machine nous allons récupérer, depuis notre serveur Debian, le **fichier1.zip**. 

``` bash
smbclient "//172.16.10.10"/A partager/" -U Wilder
```

Après avoir entré le password Wilder, un shell SMB s'ouvre. Nous pouvons donc vérifier le contenu du dossier et télécharger le **fichier1.zip**

``` bash
ls
get fichier1.zip
```

Depuis notre serveur **Debian**, nous allons lancer zip2john sur le fichier ciblé en redirigeant la sortie dans un fichier texte pour récupérer le hash du mot de passe avec la commande : 

``` bash
cd john/run/
./zip2john "/root/fichier1.zip" > hash.txt
```

Une fois le hash récupérer, il nous reste simplement à le décrypter grâce a HashCat 

``` bash
hashcat -m 17200 hast.txt -a 3 
```

## Utilisation avancé 

Dans les fonctions avancées nous allons tester l'attaque par *masque d'attaque* .

### L'attaque par masque d'attaque . 

Pour faire une attaque par masque , il faut d'abord savoir la structure probable du mot de passe . C'est une attaque utile quand on connais une partie du mot de passe ou son format.

Les premières étapes sont les mêmes que pour la méthode simple. On récupère le *hash* du fichier cible, une fois fait on lance la commande pour faire l'attaque par masque d'attaque.
Sachant que nous connaissons une partie du mot de passe : 

``` bash
hashcat -m 17200 hast.txt -a 3 Livre?u?l?l?l?d?d
```

Une fois le *hash* décrypté , on va lancer la commande pour afficher le mot de passe .

``` bash
hashcat -m 17200 show hash.txt
```

Une ligne de commande s'affiche et le mot de passe se situe à la fin.

Voilà, vous avez récupérez votre mot de passe ! 

## F.A.Q
_________________________

