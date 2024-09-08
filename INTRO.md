# Création d'un landing page

Rendez-vous sur ce dépôt GitHub : [Landing Page](https://github.com/Jensone/sf-00)

Téléchargez le projet et ouvrez-le dans VS Code (ou autre).

## Installation des dépendances PHP 8.3

Pour commencer, il faut installer les dépendances PHP. Pour cela, il faut exécuter la commande suivante dans votre terminal :

```bash
composer install
```

## Lancement du serveur web

Pour lancer le serveur web, il faut exécuter la commande suivante dans votre terminal :

```bash
symfony serve
```

Si la commande `symfony serve` n'est pas reconnue, optez pour la commande suivante :

```bash
php -S localhost:8000 -t public public/index.php
```

## Créer une base de données

Pour créer une base de données, il faut exécuter la commande suivante dans votre terminal :

```bash
php bin/console doctrine:database:create
```

Il s'agit d'une base de données SQLite. C'est un fichier qui a été créé dans le dossier `/var/data.db`.

## Créer les tables

Pour créer les tables, ça se passe en deux étapes :

1. Exécuter la commande suivante dans votre terminal : `php bin/console make:migration`
2. Puis, exécuter la commande suivante dans votre terminal : `php bin/console doctrine:migrations:migrate`

La première commande va créer un fichier dans le dossier `src/Migrations` qui contient les migrations. C'est un fichier qui contient des instructions SQL pour mettre à jour la base de données.

La deuxième commande va exécuter les migrations. Cela veut dire que chaque instruction SQL dans le fichier du dossier `src/Migrations` sera exécutée.

## Remplir la base de données

Pour remplir la base de données, il faut exécuter la commande suivante dans votre terminal :

```bash
php bin/console doctrine:fixtures:load
```

Cela va charger les données fictives dans la base de données.

*Note : Si le nom qui s'affiche sur votre page plus tard pour la première fois est Rebecca, vous avez gagnez une clé API d'OpenAI pour utiliser le service de génération de texte dans vos commits pendant un mois.*

---

Pause commit : enregistrez ce que vous avez fait jusqu'à maintenant.

```bash
git add .
git commit -m "Ajout de la base de données"
```

---

## Créer une page

Pour créer une page, nous avons besoin d'un contrôleur et d'une vue. Pour cela, il faut exécuter la commande suivante dans votre terminal :

```bash
php bin/console make:controller Home
```

Cela va créer un contrôleur dans le dossier `src/Controller`. Son nom c'est `HomeController.php`.
La vue va être créée dans le dossier `templates/home/index.html.twig`.

Rendez-vous sur votre navigateur à l'adresse `http://localhost:8000/home` et vous devriez voir la page qui a été crée.

## Modifier le contenu de la page

Dans le template (la vue), vous allez modifier son contenu avec celui-ci :

```twig
{% extends 'base.html.twig' %}

{% block title %}Landing Page{% endblock %}

{% block body %}
    <main class="bg-gradient-to-r from-neutral-50 to-neutral-100 h-screen flex items-center justify-center">
        <div class="text-center">
            <h1 class="text-5xl font-bold text-neutral-800">
                Welcome to my Landing Page
            </h1>
            <p class="mt-4 text-2xl text-neutral-500">
                I'm <span class="font-bold text-orange-800">{{ name ?? 'Nobody'}}</span>, a web developer.
            </p>
            <p class="mt-4 text-xl text-neutral-500">
                This is a landing page template for a personal website.
            </p>
        </div>
    </main>
{% endblock %}
```

---

Pause commit : enregistrez ce que vous avez fait jusqu'à maintenant.

```bash
git add .
git commit -m "Mise à jour du template de la page Home"
```

---

## Ajouter des données dynamiques

"Nobody" est le nom par défaut. Modifiez-le sans toucher au code du template. Pour cela, on passe par le controlleur `HomeController.php`.

```php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Routing\Annotation\Route;

class HomeController extends AbstractController
{
    #[Route('/{name}', name: 'home', methods: ['GET'])] // Ceci est une annotation PHP, ici elle permet de définir une route pour le contrôleur et ses paramètres.
    public function index(Request $request, string $name): Response // Ceci est le contrôleur qui va être appelé lorsque l'utilisateur accède à la page '/'
    {
        $name = $request->query->get('name'); // À l'aide de la classe Request de HttpFoundation, on peut récupérer les paramètres de la route.
        return $this->render('home/index.html.twig', [ // La méthode render() de la classe AbstractController permet de renvoyer un template avec des données.
            'name' => $name ?? 'Nobody', // Si le paramètre n'est pas renseigné, on lui donne un nom par défaut.
        ]);
    }
}
```

Rendez-vous sur votre navigateur à l'adresse `http://localhost:8000/` et ajoutez votre prénom à la fin de l'URL.

---

Pause commit : enregistrez ce que vous avez fait jusqu'à maintenant.

```bash
git add .
git commit -m "Récupération des paramètres de la route et renvoi du template avec les données"
```

## Afficher des données de la base de données

Il y a une liste de prénoms et noms de personnes dans la base de données. Vous allez les afficher de la manière suivante :

1. Mettez à jour le contrôleur `HomeController.php` pour qu'il récupère les données de la base de données.

```php

namespace App\Controller;

// ...

class HomeController extends AbstractController
{
    #[Route('/{name}', name: 'home', methods: ['GET'])] 
    public function index(Request $request, string $name, PersonRepository $personRepository): Response // On ajoute un paramètre supplémentaire pour récupérer les données de la base de données.
    {
        $name = $request->query->get('name'); 
        return $this->render('home/index.html.twig', [ 
            'name' => $name ?? 'Nobody', 
            'people' => $personRepository->findAll(), // On récupère toutes les personnes de la base de données et les envoie à la vue.
        ]);
    }
}
```

2. Bouclez sur les données de la base de données pour afficher les noms et prénoms.

```twig
{% extends 'base.html.twig' %}

//...

{% block body %}
    <main class="bg-gradient-to-r from-neutral-50 to-neutral-100 h-screen flex items-center justify-center">
        
        //...

        <ul class="mt-4 text-xl text-neutral-500">
            {% for person in people %}
                <li>{{ person.firstName }} {{ person.lastName }}</li>
            {% endfor %}
        </ul>
    </main>
{% endblock %}

```

---

## Félicitations !

Vous avez édité votre premier projet Symfony.

Maintenant, découvrons comment créer un projet de zéro avec pour thème une application de partage de notes, "CodeXpress".