# ACLs

Module natif : il associe les permissions de chaque fonctionnalité aux rôles. Pour un rôle ordinaire, l'absence d'autorisation explicite signifie un refus.

## Configurer les droits

Dans le BO, ouvrez ACLs, choisissez un rôle et cochez les actions à autoriser parmi les modules installés. Enregistrez les changements. Les libellés sont personnalisables ; les codes techniques ne changent pas. Les opérations de gestion des permissions sont elles-mêmes protégées.

## Déclarer les permissions d'un module

Dans votre module métier, implémentez PermissionProviderInterface et fournissez des PermissionDefinition. Par exemple :

~~~php
use SymSvel\Module\Acls\Permission\PermissionDefinition;
use SymSvel\Module\Acls\Permission\PermissionProviderInterface;

final class CataloguePermissions implements PermissionProviderInterface
{
    public function permissions(): iterable
    {
        yield new PermissionDefinition('catalogue', 'list', 'Consulter le catalogue');
    }
}
~~~

Enregistrez le fournisseur comme service Symfony selon le contrat du module ACLs. La permission apparaît dans le catalogue lorsque le module et son fournisseur sont découverts.

## Protéger une action BO

Injectez SymSvel\Module\Acls\Access\BoAccessGate dans votre contrôleur et contrôlez la requête avant toute opération :

~~~php
$decision = $gate->check($request, 'catalogue', 'list');
if (!$decision->allowed()) {
    return ApiResponse::error('access_denied', 'Accès refusé', $decision->status);
}
~~~

Importez SymSvel\Core\Http\ApiResponse. Le gate renvoie 401 en l'absence d'identité et 403 en l'absence de droit. Ne vous fiez jamais uniquement à l'affichage conditionnel d'un bouton dans Svelte.


### Scopes natifs

Back et BO uniquement. Les interfaces d’administration sont exclusivement dans le BO. Les anciennes routes de test App/Front ne sont plus distribuées.
