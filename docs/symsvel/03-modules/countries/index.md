# Countries

Module natif : référentiel des pays et territoires avec code ISO 3166 alpha-2, nom natif, devise, langues officielles et état actif.

## Configuration et utilisation

Le référentiel est installé même si le projet est monolingue. Dans un projet multilingue, le BO propose l'administration des pays : un utilisateur autorisé peut activer ou désactiver un pays. Les codes ISO et métadonnées de référence ne sont pas éditables.

La visibilité de cet écran dépend de languages.multilingual. Il n'existe pas de paramètre Countries supplémentaire nécessaire à l'installation.

## Intégrer un sélecteur de pays

~~~bash
curl https://api.example.com/apiopen/countries
~~~

Adaptez l'adresse du backend. Le endpoint public retourne les pays actifs, y compris sur un projet monolingue. Les noms sont localisés avec Symfony Intl et utilisent le nom natif en repli.

| Endpoint | Fonction |
| --- | --- |
| GET /apiopen/countries | Référentiel public actif |
| GET /api/countries | Référentiel App actif |
| GET /apibo/countries | Liste BO complète, avec ACL |
| PATCH /apibo/countries/{code} | Modifier l'état active, avec ACL |

Conservez les codes ISO plutôt que les libellés traduits dans vos propres données métier.
