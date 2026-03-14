# Sommaire

- [**HashCat**](#hashcat)
  -  [**Utilisation de base**](#utilisation-de-base)
  -  [**Utilisation avancée**](#utilisation-avancée)

- [**John The Ripper**](#johntheripper)
  -  [**Utilisation de base**](#utilisation-de-base-1)
  -  [**Utilisation avancée**](#utilisation-avancée-1)

- [**Attaque sur mot de passe d'un compte local**](effectuer-des-attaques-sur-le-mot-de-passe-d--un-compte-local-du-serveur)
- [**FAQ**](#faq)

# Guide d'utilisation HashCat

## HashCat

C'est un logiciel open source , exploitant la puissance des GPU pour déchiffrer des mots de passe cryptés.

## Utilisation de base 

Pour illustrer l'utilisation de **HashCat**, nous allons tenter de retrouver le mot de passe d'un fichier .zip chiffré, hébergé sur une machine Windows server.

Nous aurons besoin d'installer également [_**John The Ripper**_](https://www.openwall.com/john/) pour récupérer zip2john, et d'installer les protocoles cifs.

Grâce aux protocoles cifs et à l'IP de la machine nous allons récupérer, depuis notre serveur Debian, le **fichier2.zip**. 

``` bash
smbclient -L 172.16.10.5 -U Wilder
smbclient "//172.16.10.5/audit/" -U Wilder
```

Après avoir entré le password Wilder, un shell SMB s'ouvre. Nous pouvons donc vérifier le contenu du dossier et télécharger le **fichier2.zip**

``` bash
ls
get fichier2.zip
```

Quittez le shell SMB en faisant CTRL+C

Depuis notre serveur **Debian**, nous allons lancer zip2john sur le fichier ciblé en redirigeant la sortie dans un fichier texte pour récupérer le hash du mot de passe avec la commande : 

``` bash
cd john/run/
./zip2john "/root/fichier2.zip" > hash.txt
```

Une fois le hash récupéré, il est préférable de le lancer et de le vérifier.

![HASHBUG|1264](https://github.com/WildCodeSchool/TSSR-0226-P1-G1/blob/main/SCREENSHOT/HAS_MAUVAIS.png)

Sur l'exemple au dessus, le hash commence par le nom de notre fichier, ce qui peut poser problème lors du décryptage . Pour y remédier,  nous allons le mettre au propre.

``` bash
cut -d ':' -f2 hash.txt > clean_hash.txt
```

Maintenant, il nous reste plus qu'à le décrypter grâce a HashCat 

``` bash
hashcat -m 17200 clean_hast.txt -a 3 
```

## Utilisation avancée

Dans les fonctions avancées nous allons tester l'attaque par *masque d'attaque* .

### L'attaque par masque d'attaque . 

Pour faire une attaque par masque , il faut d'abord connaître la structure probable du mot de passe . C'est une attaque utile quand on connait une partie du mot de passe ou son format.

Les premières étapes sont les mêmes que pour la méthode simple. On récupère le *hash* du fichier cible, une fois fait on lance la commande pour faire l'attaque par masque d'attaque.
Sachant que nous connaissons une partie du mot de passe : 

``` bash
hashcat -m 17200 clean_hash.txt -a 3 Livre?u?l?l?l?d?d
```

Une fois le *hash* décrypté , on va lancer la commande pour afficher le mot de passe .

``` bash
hashcat -m 17200 --show clean_hash.txt
```

Une ligne de commande s'affiche avec votre hash et le mot de passe se situe à la fin.

Voilà, vous avez récupéré votre mot de passe ! 
_________________________

# Guide d'utilisation JohnTheRipper

## JohnTheRipper

C'est un logiciel libre de crackage de mots de passe.

## Utilisation de base 

Pour utiliser **JohnTheRipper**, il nous faut un dossier compressé et sécurisé qui sera pour notre projet en .zip sur une machine Windows 10.

Nous aurons besoin d'installer également les protocoles cifs.

D'abord il faudra créer le dossier de travail sur notre machine Linux. 

``` bash
mkdir -p ~/audit 
cd ~/audit
```

Ensuite, créer le point de montage et monter le partage grâce aux protocoles cifs

``` bash
sudo mkdir -p /mnt/windows_client
sudo mount -t cifs //172.16.10.10/audit /mnt/windows_client -o username=Wilder,password=Azerty1*
```

Nous allons maintenant extraire le hash.

``` bash
/snap/john-the-ripper/694/bin/zip2john /mnt/windows_client/fichier1.zip > /home/wilder/audit/hash_fichier1.txt
```

## Utilisation avancée

Dans les fonctions avancées nous allons tester l'attaque par *dictionnaire* .

### L'attaque par dictionnaire.

Les premières étapes sont les mêmes que pour la méthode simple. On récupère le *hash* du fichier cible, une fois fait on télécharge un fichier, en l'occurrence **rockyou.txt**, qui est une liste de mot de passe (wordlist) parmi les plus célèbre utilisé en cybersécurité.

``` bash
curl -L https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt -o /home/wilder/audit/rockyou.txt
```

Vérifiez que tous les fichiers sont là .

``` bash
ls /home/wilder/audit/
```

Tout est là, nous allons pouvoir lancer l'attaque .

``` bash
/snap/bin/john --wordlist=/home/wilder/audit/rockyou.txt /home/wilder/audit/hash_fichier1.txt
```

On peut enfin afficher le mot de passe cracké.

``` bash
/snap/bin/john --show --format=PKZIP /home/wilder/audit/hash_fichier1.txt
```
Une ligne de commande s'affiche avec le hash et le mot de passe.

Voilà, vous avez récupéré votre mot de passe !

## L'attaque en force brute

Les premières étapes sont les mêmes que pour la méthode simple. On récupère le *hash* du fichier cible, une fois fait on peut lancer l'attaque.

``` bash
john --incremental hash_fichier1.txt
```

Cela peut prendre beaucoup de temps suivant la compléxité du mot de passe. 

Une fois fini vous avez récupéré votre mot de passe ! 


# Effectuer des attaques sur le mot de passe d’un compte local du serveur

Pour pouvoir attaquer le mot de passe d'un compte local , ici sur Windows serveur, nous allons devoir vérifier si le port **SSH** est ouvert grâce au logiciel **nmap**.

```bash 
nmap -p 22 172.16.10.5
```
![CHECKIP]()

Une fois la vérification faîtes, nous allons télécharger une worldlist pour pouvoir lancer l'attaque.

``` bash
curl -L https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt -o /root/rockyou.txt
```

On peut maintenant lancer l'attaque pour récupérer le mot de passe Utilisateur, ici *Wilder*.

``` bash
hydra -l Wilder -P rockyou.txt -t 1 -W 10 ssh://172.16.10.5
```
![RESULTAT]()

Voilà, vous avez récupéré le mot de passe utilisateur.
## FAQ

**Q1**: J'ai une erreur **113** lorsque j'essaye de récupérer le fichier1.zip sur ma machine Windows.

**R1**: Vérifiez que le dossier est bien partagé "à tout le monde" sur la machine Windows.

**Q2**: Lorsque je veux décrypter mon hash, il me dit **No hashed loaded**.

**R2**: Vérifiez bien que le hash ne commence pas par le nom du fichier zip, dans ce cas utilisez la commande pour nettoyer le hash.

**Q3**: Quand je lance l'attaque Hydra, il me dit : Error all children were disabled due too many connection errors.

**R3**: Ce message s'affiche lorsque le Windows Serveur met en place le blocage de tentative de connexion a un compte après avoir échoué plusieurs mot de passes. Vous pouvez l'enlever directement sur Windows Serveur, ou en ligne de commande. 
