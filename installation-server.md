# Guide d'installation d'un serveur

## Installation générale

### Etape 1

Côté OVH, il faut demander l'installation d'un VPS (Virtual Private Server)
Le fonctionnement reste identique à un serveur dédié mais ne nécessite pas d'installation
physique chez l'hébergeur. Il en existe de différente puissance, capacité: https://www.ovhcloud.com/fr/vps/

**Conseil: Prendre le plus petit dans un premier temps quite à augmenter sa capacité dans le temps**

**Préférer l'utilisation d'Ubuntu dans sa dernière version, question demandée par OVH lors de l'installation**

### Etape 2

L'installation est rapide du côté OVH et envoi un mail une fois l'installation terminée. Ce mail contient
les informations de connexion pour l'utilisateur root avec le mot de passe associé ainsi que l'adresse IP du serveur (ex: 51.30.48.139)
On peut donc se connecter en ssh sur le serveur avec ces identifiants afin d'y installer nos applications:

```
ssh root@51.30.48.139
```

### Etape 2,5

Créer un utilisateur pour ne pas se connecter en temps que root.

-   sudo adduser marina
-   ssh marina@51.30.48.139

Donner les droits au nouvel utilisateur :
TODO completer

### Etape 3

Nous pouvons désormais installer nos différents programmes afin de faire fonctionner nos différentes applications, à savoir:

-   Nginx / Apache: Serveurs web permettant de servir nos ressources (fichiers, applications...)
-   MySQL, Mongo...: Serveurs de base de données
-   PHP, Node, Java: Langages permettant de faire fonctionner nos applications

Pour installer nos packages, on utilise le package manager du système d'exploitation. Attention, celui-ci
change en fonction du système. Une machine installée avec debian n'aura pas le même package manager qu'une machine Ubuntu

Sous Ubuntu, on utilise le package manager **apt**. Une multitude de commandes existent mais nous allons en utiliser qu'une petite partie:

-   `apt-get install <package>` permet d'installer un package
-   `apt-cache search <pacakge>` permet de rechercher un package
-   `apt-get update` permet de mettre à jour nos packages

Pour l'installation de node, préférer le téléchargement du binaire directement sur le site officiel: https://nodejs.org/en/download/
Exécuter les commandes suivantes:

```
cd /tmp
wget https://nodejs.org/dist/v12.18.1/node-v12.18.1-linux-x64.tar.xz
tar xvf node-v12.18.1-linux-x64.tar.xz
cd node-v12.18.1-linux-x64
cp bin/node /usr/local/bin
cp bin/npm /usr/local/bin
cp bin/npx /usr/local/bin
```

## Principe de déploiment d'une application Node.js

[![](https://imgur.com/8OMZg2Q.png)]()

## Penser à la sécurité

La sécurité est fondamentale lors de l'administration d'un serveur. Voici quelques règles élémentaires:

-   Ne pas permettre l'authentification avec le login / mot de passe en ssh à moins d'avoir un mot de passe assez fort. Préférer l'utilisation d'une clé ssh, qu'on envoie sur le serveur avec la commande `ssh-copy-id <user>@<ip_serveur>`
-   Autre possibilité, ne pas permettre l'accès ssh à d'autres utilisateurs que ceux qu'on sélectionne.
-   Pour toute ces options, consulter le fichier `/etc/ssh/sshd_config`

## installer certbot

[https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx](https://certbot.eff.org/lets-encrypt/ubuntufocal-nginx)

## installer un projet

-   créer un sous-domaine dans la zone DNS
-   cloner le projet dans /data
-   ajouter un fichier dans /etc/nginx/sites-available/ pour lier le dossier de travail au domaine
-   créer un lien symbolique vers sites enable : ln -s source destination : ln -s /etc/nginx/sites-available /etc/nginx/sites-enable/
-   redemmarer les services : sudo service nginx restart
-   générer le certifica ssl avec certbot : sudo certbot --nginx
