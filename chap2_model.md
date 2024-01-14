# Les données de l'application

Nous allons créer une table pour l'instant dans l'application file rouge.

Et nous allons créer, évidemment, une base de données **db_trainer** dans MySQL.

Dans Symfony on utilise l'ORM (Object Relation Mapping) **Doctrine** pour gérer les données. On mappe les données dans des objets, ils sont en relation avec d'autres classes représentant d'autre(s) entité(s). Les **repositories** permettront pour leurs part de manipuler les données. 
 
```mermaid
flowchart LR
    id1[(db_trainer)]
```

## Installation des dépendances

🐚

```bash
composer require symfony/orm-pack
composer require --dev symfony/maker-bundle
```

C'est dans le fichier **.env** que l'on va renseigner la connexion à la base de données MySQL.

Une partie de code c'est ajouté automatiquement, commentez postgres et décommentez mysql

```t
###> doctrine/doctrine-bundle ###
# Format described at https://www.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/configuration.html#connecting-using-a-url
# IMPORTANT: You MUST configure your server version, either here or in config/packages/doctrine.yaml
#
# DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
# DATABASE_URL="mysql://app:!ChangeMe!@127.0.0.1:3306/app?serverVersion=8.0.32&charset=utf8mb4"
DATABASE_URL="mysql://root:antoine@127.0.0.1:8889/db_trainer?serverVersion=8.2&charset=utf8mb4"

# DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=15&charset=utf8"
###< doctrine/doctrine-bundle ###
```

Plusieurs choses à vérifier 

1. Le port voyez dans MAMP (si c'est votre environnement)
1. Vérifiez les variable d'environnement, l'exécutable mysql doit-être accessible depuis un terminale.

Puis vous devez en ligne de commande exécuter la commande suivante, elle va créer la base de données.

🚀

```bash
php bin/console doctrine:database:create
```

## Schéma de la base de données

Nous allons pour l'instant créer uniquement la table trainers, nous reviendrons sur la conception des données dans un autre chapitre, ici le but est de découvrir Doctrine.


### Table Trainer

```mermaid
---
title: Trainer Model
---
classDiagram
    class Trainer
    Trainer : +int id
    Trainer : +String name
    Trainer : +String email
    Trainer : +String phone
```

## Utilisation du maker pour créer l'entité Trainer

L'entité Trainer est la classe PHP qui va permettre de faire le lien avec la table physique en base de données.

Tapez les lignes de commandes suivantes et créez l'entité Trainer.

❌ Attention, la commande maker:entity va vous posez des questions sur le type des données de l'entité, vous devez répondre et précisez certaines choses. Vous pourrez par la suite revenir à l'entité créé pour la modifier.

🚀

```bash
# Création de l'entité > vous devez répondre aux questions pas à pas pour créer les champs.
php bin/console make:entity

# Création de la migration code SQL écrit dans du PHP
php bin/console make:migration

# Création physique de la table dans la base de données
php bin/console doctrine:migrations:migrate

# Modifier l'entité Trainer ajouter des champs
php bin/console make:entity Trainer
```

❌ Si vous modifiez une entité vous devez recréer une migration (un nouveau fichier de migration), et refaire les étapes ci-dessus pour que la table soit effectivement modifié.


## Ajout de données d'exemple 

🚀

```bash
composer require --dev orm-fixtures
php bin/console doctrine:fixtures:loa
```

