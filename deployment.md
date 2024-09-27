---
description: Comment déployer son projet dans l'espace 🚀
---

---
description: Déployer une application Symfony sur un serveur de production
---

# Le déploiement

Ce n'est pas parce que l'application est terminée qu'elle est en ligne. Il faut la déployer sur un serveur de production.

# Pourquoi déployer une application Symfony ?

Le déploiement d'une application Symfony permet de mettre en ligne une application web. Cela permet de rendre accessible l'application à tous les utilisateurs.

## Achat de nom de domaine + hébergement web

Il est indispensable d'avoir un serveur web pour pouvoir déployer une application. Le mieux est de l'accompagner avec un nom de dommaine :

Valeures sûres :
- [Hostinger]()
- [Infomaniak]()
- [o2Switch]()

Peu chères :
- [Obambu]()


## Préparation du serveur de production

- Rattacher un nom de domaine au serveur
- Créer une base de données correspondant idéalement au projet
- Vérifier la version PHP correspondante
- Vérifier la version de composer
- Identifiants SSH

Ces informations sont à récolter avant de créer le projet pour éviter les surprise lors du déploiement.

---

## Préparation du projet en local

- Réinitialiser les migrations avec un seul fichier de version, afin de les migrer en une seule fois sur le serveur de production
- Compiler les éléments nécessaires à la production (tailwindcss, assets, webpack, etc.)
- Renseignez les informations du serveur de base de données, MAILER_DSN, API_KEY dans le fichier `.env` (Attention à ne pas mettre en public sur Github ses informations)
- Mettre en place le fichier `.htaccess` du dossier `public` avec le contenu suivant [public/.htaccess]()
- Dans le cas où vous devez aussi redirger la racine du nom de domaine vers le dossier public en passant par un serveur web, il faut mettre en place le fichier `.htaccess` du dossier `public` avec le contenu suivant [/.htaccess]()
- Commit et push de l'ensemble du projet sur GitHub. Veiller à ce que la branche principale soit celle de production. Cela permettra de cloner le projet sur le serveur de production avec la commande `git clone`.

## Déploiement du projet sur le serveur de production

Vérifier auprès de votre hébergeur la version de composer. Dans le cas de la version 2+ la commande sera probablement `composer2` au lieu de `composer`.

- Cloner le projet sur le serveur de production avec la commande `git clone`
- Renommer le dossier du projet si nécessaire.
- Modifier le fichier `.env` avec l'environnement de production dans le cas où votre dépôt GitHub est public.
- Rendez-vous dans le terminal du serveur de production et se rendre dans le dossier du projet et réaliser les commandes suivantes :
  - `composer install` ou `composer2 install`
  - Si vous n'avez pas de fichier de migration :
    - `php bin/console make:migration`
  - `php bin/console d:m:m` si besoin (lancez les fixtures)
  - Passer l'environnement en production dans le fichier `.env`
  - `php bin/console cache:clear && php bin/console cache:warmup`
  - `composer install --no-dev --optimize-autoloader`
- Vérifier que l'application est bien en ligne
- Dans le cas où une erreur se produit, voici les précautions :
  - Erreur ^500 : Réactiver le mode `dev` afin de voir l'erreur et la corriger
  - Erreur ^400 : Votre application ne pointe pas sur le bon dossier, vérifier que la racine de l'application est bien le dossier `public`