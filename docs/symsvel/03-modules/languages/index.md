# Languages

Module natif : référentiel des langues, traductions du BO, de l'App et du Front, préférences linguistiques des comptes.

## Configuration

| Clé | Rôle |
| --- | --- |
| languages.primary_language | Langue principale, définie au Setup et non modifiable ensuite |
| languages.multilingual | Active le mode multilingue |

Dans Languages du BO, activez les langues souhaitées. Choisissez la langue de traduction puis le module à traduire. L'éditeur distingue les valeurs manquantes de celles déjà enregistrées. Vous pouvez éditer les textes et utiliser les fonctions d'import/export disponibles selon vos permissions.

## Ajouter les textes de votre module

Pour chaque interface concernée, créez un dictionnaire source, par exemple modules/catalogue/bo/src/lib/translations/fr.ts :

~~~typescript
export default {
  "title": "Catalogue",
  "create": "Ajouter un produit"
} as const;
~~~

Utilisez un objet JSON strict dans l'export, sans code exécuté dans le dictionnaire. Les clés publiques suivent le format bo.catalogue.title et bo.catalogue.create. Pour App, utilisez le scope app. Le module est propriétaire de ses textes ; n'ajoutez pas ses clés dans un dictionnaire système de l'interface hôte.

Tooling installe les dictionnaires dans src/lib/translations/catalogue/<lang>.ts et synchronise les clés avec Languages dans le cycle de vie du module.

## API

GET /apiopen/languages et GET /api/languages exposent les langues disponibles ; GET /apibo/languages permet leur gestion BO selon les ACLs. Les choix de langue d'un compte sont accessibles via GET /apiopen/languages/account-options. Un texte de secours affiché n'est pas une traduction persistée dans la langue cible.


## Actions de validation des traductions (26/11/2026)

La validation d’une traduction en ligne, l’application d’un import CSV et la confirmation de création d’une clé utilisent `Button variant="success"` du BO. L’ouverture de l’éditeur reste primaire, les aperçus, annulations et exports restent secondaires, et les suppressions restent `danger`. Aucun changement de traitement, de permission ni de stockage.
