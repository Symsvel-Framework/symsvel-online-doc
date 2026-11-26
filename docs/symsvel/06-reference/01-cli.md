# Référence CLI

La CLI est le point d'entrée local de SymSvel. Elle orchestre Composer, Git, les opérations Tooling et le wizard. Les commandes s'exécutent depuis la racine du projet ou son backend, sauf create-project qui s'exécute dans un dossier de destination vide.

| Commande | Utilité |
| --- | --- |
| symsvel doctor | Vérifier les prérequis |
| symsvel --version | Afficher la version |
| symsvel create-project --components=all | Créer un projet |
| symsvel setup | Reprendre le wizard local |
| symsvel make:module NOM --scopes=back,bo | Générer un module |
| symsvel module:migrate NOM | Générer une migration source |
| symsvel require PACKAGE | Installer un package |
| symsvel update [PACKAGE] | Mettre à jour les dépendances |
| symsvel update back/bo/app/front | Mettre à jour un composant (un seul argument) |
| symsvel remove PACKAGE | Retirer un module facultatif |
| symsvel documentation:build-dev | Reconstruire le site local |
| symsvel documentation:sync | Actualiser le corpus privé de documentation |
| symsvel permissions:fix | Réparer les ACL locales (CLI 0.3.1+) |

Utilisez symsvel --help pour les options exactes. Les opérations de maintenance avancées sont accessibles via php bin/console list symsvel depuis backend.

[Code source et référence CLI sur GitHub](https://github.com/Symsvel-Framework/symsvel-cli).
