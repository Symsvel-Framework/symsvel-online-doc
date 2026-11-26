# Roles

Module natif : il organise les comptes BO par rôle et les associe aux permissions déclarées par ACLs.

## Rôles fournis

| Code | Usage |
| --- | --- |
| ROLE_DEV | Premier administrateur et opérations techniques |
| ROLE_SUPER_ADMIN | Administration étendue, hors opérations techniques réservées |
| ROLE_ADMIN | Administration selon les permissions attribuées |
| ROLE_USER | Identité utilisateur |

Les rôles système sont protégés. Créez des rôles métier depuis le BO avec un code ROLE_ suivi de lettres majuscules, chiffres ou underscores et un libellé lisible. Affectez les permissions nécessaires, puis attribuez le rôle aux administrateurs autorisés.

## Configuration

Il n'y a pas de fichier à éditer pour gérer les rôles. Les règles d'attribution et les contrôles anti-escalade sont appliqués côté serveur. Un rôle utilisé ou protégé ne peut pas être supprimé librement.

## API BO

GET /apibo/roles liste les rôles ; POST crée un rôle, PATCH /apibo/roles/{id} le modifie, DELETE le supprime si autorisé. Utilisez [ACLs](../acls/index.md) pour vérifier les permissions d'une opération métier.


### Scopes natifs

Back et BO uniquement. Les interfaces d’administration sont exclusivement dans le BO. Les anciennes routes de test App/Front ne sont plus distribuées.
