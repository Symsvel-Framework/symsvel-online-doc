# 3. Créer un projet

Créez un dossier vide, puis lancez la CLI depuis votre terminal. Ce parcours suppose une CLI fonctionnelle. Sous Windows, suivez le [guide de qualification](01a-windows.md) et utilisez impérativement `symsvel create-project --components=all --dev` pour le CT initial SQLite ; l'exemple stable ci-dessous concerne les systèmes déjà validés :

~~~bash
mkdir catalogue
cd catalogue
symsvel create-project --components=all
~~~

Le BO est toujours installé. Choisissez les interfaces selon vos besoins :

| Option | Backend | BO | App | Front |
| --- | --- | --- | --- | --- |
| bo | Oui | Oui | Non | Non |
| app | Oui | Oui | Oui | Non |
| front | Oui | Oui | Non | Oui |
| all | Oui | Oui | Oui | Oui |

App est destinée à l'espace utilisateur ; Front à l'interface publique. Pour un projet métier, utilisez le canal stable (par défaut). L'option --dev sélectionne les branches de développement du framework ; elle est réservée aux essais de nouvelles fonctionnalités.

La CLI récupère les dépendances, installe les modules natifs nécessaires, prépare les interfaces et ouvre automatiquement le wizard dans votre navigateur.

Si vous configurez un serveur web local, dirigez le document root du backend vers backend/public et les interfaces compilées vers leurs dossiers build. Ne pointez jamais un document root sur la racine du projet.

[Étape suivante : configurer le projet](04-setup-local.md).
