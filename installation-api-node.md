# Guide d'installation d'une API node

## Idée générale

L'idée est d'utiliser Nginx en tant que reverse proxy. Ce n'est plus Nginx qui est en charge de servir les ressources 
mais il s'occupe juste de réaliser la passerelle vers une application node existante

On ne sert jamais une application node sur le port 80 (http) ou 443 (https) mais plutôt sur un port 3000
Nginx cependant écoute sur le port 80 ou 443 et redirige les requêtes des utilisateurs vers le port 3000 de l'application Node

## Installation

* Lancer l'application Node sur le port 3000 de manière infinie, par exemple avec forever https://www.npmjs.com/package/forever

* Ajouter un nouveau site dans le répertoire site-availables de nginx qui contient la configuration suivante:

```
server {
    listen 80;
    server_name api.marinafront.fr;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         http://localhost:3000;
    }
}
```
Il convient de changer `api.marinafront.fr` par la bonne URL de l'API

* Générer les certificats SSL avec Certbot
* Créer un lien symbolique dans les sites-enabled
* Redémarrer Nginx: `sudo service nginx restart`
