
# Guide d'utilisation HashCat

## HashCat

C'est un logiciel open source , exploitant la puissance des GPU pour déchiffrer des mots de passe cryptés.

## Utilisation de base 

Pour utiliser **HashCat**, il nous faut un dossier compressé et sécurisé qui sera pour notre projet en .zip sur une machine Windows 10.

Nous aurons besoin d'installer également [_**John The Ripper**_](https://www.openwall.com/john/) pour récupérer zip2john, et les protocoles cifs.

Grâce aux protocoles cifs et à l'IP de la machine nous allons récupérer, depuis notre serveur Debian, le **fichier1.zip**. 

``` bash
smbclient -L 172.16.10.5 -U Wilder
smbclient "//172.16.10.5/A partager/" -U Wilder
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

Une fois le hash récupérer, il est préférable de le lancer et de le vérifier.

![HASHBUG|1264](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/HAS_MAUVAIS.png)

Sur l'exemple au dessus, le hash commence par le nom de notre fichier, ce qui peut poser problème lors du décryptage . Pour y remédier,  nous allons le mettre au propre.

``` bash
cut -d ':' -f2 hash.txt > clean_hash.txt
```

Maintenant, il nous reste plus à le décrypter grâce a HashCat 

``` bash
hashcat -m 17200 clean_hast.txt -a 3 
```

## Utilisation avancé 

Dans les fonctions avancées nous allons tester l'attaque par *masque d'attaque* .

### L'attaque par masque d'attaque . 

Pour faire une attaque par masque , il faut d'abord savoir la structure probable du mot de passe . C'est une attaque utile quand on connais une partie du mot de passe ou son format.

Les premières étapes sont les mêmes que pour la méthode simple. On récupère le *hash* du fichier cible, une fois fait on lance la commande pour faire l'attaque par masque d'attaque.
Sachant que nous connaissons une partie du mot de passe : 

``` bash
hashcat -m 17200 clean_hast.txt -a 3 Livre?u?l?l?l?d?d
```

Une fois le *hash* décrypté , on va lancer la commande pour afficher le mot de passe .

``` bash
hashcat -m 17200 --show clean_hash.txt
```

Une ligne de commande s'affiche avec votre hash et le mot de passe se situe à la fin.

Voilà, vous avez récupérez votre mot de passe ! 
_________________________

