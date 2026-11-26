# Settings

Module natif : il centralise les paramètres des modules et leurs valeurs. Les paramètres sont regroupés par module dans le BO.

## Utilisation

Dans Settings, ouvrez le groupe du module voulu et modifiez ses paramètres autorisés. Les champs sont adaptés à leur type (texte, booléen, adresse e-mail, URL, choix, secret). Les valeurs secrètes ne sont pas renvoyées dans les réponses BO ; ne les placez pas dans des variables publiques Svelte.

## Déclarer les paramètres de votre module

Implémentez SymSvel\Module\Settings\Service\SettingDefinitionProviderInterface :

~~~php
use SymSvel\Module\Settings\Service\SettingDefinition;
use SymSvel\Module\Settings\Service\SettingDefinitionProviderInterface;

final class CatalogueSettings implements SettingDefinitionProviderInterface
{
    public function definitions(): iterable
    {
        yield new SettingDefinition(
            key: 'contact_email',
            label: 'Adresse du catalogue',
            group: 'catalogue',
            type: 'email',
        );
    }
}
~~~

Enregistrez le fournisseur comme service Symfony autoconfiguré dans le module catalogue. Le registre qualifie la clé en catalogue.contact_email.

## Lire un paramètre

Injectez SymSvel\Module\Settings\Service\SettingManager :

~~~php
$email = $settings->get('catalogue.contact_email');
~~~

get retourne une chaîne ou null. Une clé inconnue est refusée. Pour écrire depuis un service autorisé, utilisez set puis flush ; les valeurs sont validées selon leur définition. Les opérations de cycle de vie du module synchronisent les définitions ; ne créez pas arbitrairement de lignes dans la table Settings.
