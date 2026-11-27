# 1. Prérequis

SymSvel s'installe sur votre poste de développement, sans Docker obligatoire.

## Choisir votre système

| Environnement | État |
| --- | --- |
| macOS | Parcours validé |
| Linux | Parcours validé |
| Windows natif | Installation initiale avec SQLite validée en CI sur `dev` ; CT indépendant en attente : [préparer votre poste Windows](01a-windows.md) |
| Windows avec WSL2 | Parcours Linux envisageable, non encore validé sous WSL2 |

Windows natif ne requiert ni WSL2 ni Docker. Pour participer à la qualification, utilisez la CLI `dev` et créez un projet de test avec `--dev` ; le support stable Windows n'est pas encore publié.

## Outils nécessaires

| Outil | Minimum / rôle |
| --- | --- |
| PHP CLI et serveur HTTP | PHP 8.4+, extensions requises par Composer, PDO et pilote de votre SGBD |
| Composer | Composer 2 |
| Git | Récupération des composants et gestion des sources |
| Node.js et npm | Node 22 recommandé ; compilation des interfaces |
| Python 3 | Construction locale de la documentation MkDocs (venv et pip disponibles) |
| SGBD | PostgreSQL, MySQL, MariaDB ou SQLite |

Sous macOS et Linux, installez ces outils avec le gestionnaire de paquets de votre système. Sous Windows, suivez les instructions du [guide Windows](01a-windows.md), notamment pour PHP, le PATH utilisateur, `npm.cmd` et `python`. Vérifiez que les commandes sont accessibles depuis votre terminal (exemples macOS/Linux) :

~~~bash
php -v
composer --version
git --version
node --version
npm --version
python3 --version
php -m
~~~

Pour SQLite, aucun serveur SQL n'est nécessaire ; activez pdo_sqlite. Pour les autres moteurs, démarrez le serveur et préparez un compte autorisé à créer la base du projet. Si vous utilisez PHP-FPM avec un compte distinct de votre utilisateur CLI, vérifiez les droits de traversée du workspace ; sous Linux, installez le paquet système acl pour les ACL locales.

Préparez également votre serveur web local et vos noms d'hôte (par exemple api.catalogue.test et bo.catalogue.test). Le wizard n'installe pas de serveur web ni ne crée vos hôtes virtuels.

[Étape suivante : installer la CLI](02-cli.md).
