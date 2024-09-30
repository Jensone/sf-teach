# Comment déployer son projet dans l'espace 🚀

Ce n'est pas parce que l'application est terminée qu'elle est en ligne. Il faut la déployer sur un serveur de production.

# Pourquoi déployer une application Symfony ?

Le déploiement d'une application Symfony permet de mettre en ligne une application web. Cela permet de rendre accessible l'application à tous les utilisateurs à partie d'un navigateur web.

## Achat de nom de domaine + hébergement web

Il est indispensable d'avoir un serveur web pour pouvoir déployer une application. Le mieux est de l'accompagner avec un nom de dommaine pour se rendre plus facilement à l'adresse de l'application plutôt qu'une adresse IP.

Liste des hébergeurs web de confiance en fiabilité et de performances :

### Hostinger

[Hostinger](https://bit.ly/agiliteach-hostinger), que vous connaissez probablment avec ses nombreuses campagnes de promotion sur YouTube et auprès d'influenceurs en tout genre. Ils ont une bonne réputation en France et sont très rapides à répondre en cas de problème. Le prix des offres bouge beaucoup avec les promos, opter pour un achat long terme est donc une bonne idée.

Lien : [Prendre une offre sur Hostinger](https://bit.ly/agiliteach-hostinger)

### Infomaniak

[Infomaniak](https://bit.ly/agiliteach-infomaniak) est un hébergeur de confiance en fiabilité et de performances. Basé en Suisse, c'est l'acteur n°1 du secteur en matière d'exploitation zéro carbone. Ils propose un offre unique qui est bien taillé pour les applications web Symfony.

Lien : [Prendre une offre sur Infomaniak](https://bit.ly/agiliteach-infomaniak)

### o2Switch

[o2Switch](https://o2switch.fr) n'est pas le dernier de la liste, loin de l'être. Avec une offre unique aussi, vous avez également tout ce qu'il faut pour gérer votre application web Symfony. L'interface de gestion n'est pas une faite-maison, ils sont équipé de cPanel.

### Bons plans

Parmi les solutions pas chère pour les petit budgets, il y a [Obambu](https://obambu.com), plusieurs offre adapté à la demande. Peu cher veut aussi dire peu de services.

Pour se procurer des noms de domaine au prix d'un ou deux cafés, visitez [Amen.fr](https://amen.fr), c'est les soldes presque toutes l'année. Il suffira de faire pointer les DNS de votre nom de domaine vers l'adresse IP de serveur de production.


## Préparation du serveur de production

- Rattacher un nom de domaine au serveur
- Créer une base de données correspondante
- Vérifier la version PHP correspondante
- Vérifier la version de composer
- Identifiants de connexion SSH

Ces informations sont à récolter avant de créer le projet pour éviter les surprises lors du déploiement.

---

## Préparation du projet en local

Réinitialiser les migrations avec un seul fichier de version, afin de les migrer en une seule fois sur le serveur de production

Compilez les éléments nécessaires à la production (tailwindcss, assets, webpack, etc.)

Renseignez les informations du serveur de base de données, MAILER_DSN, API_KEY dans le fichier `.env` (Attention à ne pas mettre le `.env` à jour pour un dépôt de code publique sur Github)

Mettez en place le fichier `.htaccess` du dossier `public` avec le contenu suivant [public/.htaccess](https://gist.github.com/Jensone/8ba44e6e2f38876b39b41ef3b1afb6b7)

Dans le cas où vous devez aussi redirger la racine du nom de domaine vers le dossier public en passant par le serveur web, il faut mettre en place le fichier `.htaccess` du dossier `public` avec le contenu suivant [/.htaccess](https://gist.github.com/Jensone/eb436093af9a58e18344dca7b044e53b)

Commit et push de l'ensemble du projet sur GitHub. Veillez à ce que la branche principale soit celle de production. Cela permettra de cloner le projet sur le serveur de production avec la commande `git clone`.

## Déploiement du projet sur le serveur de production

Vérifier auprès de votre hébergeur la version de composer. Dans le cas de la version 2+ la commande sera probablement `composer2` au lieu de `composer`.

### Les étapes

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
  - Erreur ^500 : Réactiver le mode `dev` afin de voir l'erreur et la corriger ou consultez les logs
  - Erreur ^400 : Votre application ne pointe pas sur le bon dossier, vérifier que la racine de l'application est bien le dossier `/public`