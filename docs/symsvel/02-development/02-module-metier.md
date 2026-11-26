# Créer un module métier

Un module regroupe les fonctionnalités que vous souhaitez isoler ou réutiliser : backend, interfaces, migrations et documentation.

## 1. Générer le squelette

Dans un projet de développement SymSvel (créé avec --dev pour ce tutoriel) :

~~~bash
symsvel make:module catalogue --scopes=back,bo,app
~~~

Le dossier modules/catalogue est créé. Le scope back est toujours présent ; choisissez seulement les interfaces nécessaires.

## 2. Développer le module

- back/src : bundle, contrôleurs, entités, repositories et services.
- bo/src, app/src : ressources Svelte du module.
- module.json : nom, dépendances, tables possédées, composants, documentation.
- composer.json : package et autoload PHP.
- recipe/recipe.json : modèle de recette Flex à publier si vous distribuez le module.

Déclarez les tables dont le module est propriétaire avant de générer ses migrations. Ne modifiez pas le package copié dans vendor.

## 3. Intégrer le module local

Le canal dev sait résoudre un module local de modules/catalogue via un dépôt Composer path. Le registre actuel reconnaît le préfixe symsvel-framework/module- ; conservez donc le vendor par défaut du Maker pour ce parcours local.

~~~bash
symsvel require symsvel-framework/module-catalogue dev-main
~~~

Le lien Composer permet de conserver la source dans modules/catalogue. Vérifiez l'inventaire et les routes depuis backend :

~~~bash
php bin/console symsvel:module:plan --help
php bin/console debug:router
~~~

## 4. Ajouter les migrations

Pour une entité du module, déclarez d'abord sa table dans module.json. La commande :

~~~bash
symsvel module:migrate Catalogue
~~~

génère une migration source Schema lorsque le diff est pris en charge. Lisez le fichier généré avant toute exécution. Les modifications de tables existantes et relations doivent respecter les protections du générateur ; ne contournez pas un refus en éditant une migration compilée.

## 5. Distribuer le module

Versionnez le dossier du module, fournissez les dépendances, les migrations et les guides, puis rendez son package accessible à Composer et publiez la recette Flex nécessaire. Le squelette de recette généré n'est pas automatiquement publié. Un package local lié par Composer path n'est pas un artefact de production : préparez sa distribution avant de déployer.

La prise en charge automatique de vendors tiers par le registre n'est pas généralisée ; vérifiez la compatibilité avant de distribuer un module externe.
