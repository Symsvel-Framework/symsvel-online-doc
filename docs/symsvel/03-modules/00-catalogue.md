# Catalogue des modules

Les modules natifs sont installés automatiquement par la CLI. Ils fournissent les fonctions de base de votre application ; vous n'avez pas à les réinstaller.

| Module | Fonction |
| --- | --- |
| Admins | Comptes administrateurs du BO |
| Auth | Authentification et parcours de compte |
| Roles | Rôles et protection des rôles système |
| ACLs | Droits d'accès par rôle et fonctionnalité |
| Settings | Paramètres du projet et des modules |
| Mails | Envoi d'e-mails et choix du transport |
| Languages | Langues et traductions |
| Countries | Référentiel des pays |
| TwoFactor | Double authentification |
| Users | Comptes utilisateurs (si App est installée) |

## Ajouter un module facultatif

Vérifiez d'abord son nom de package, les dépendances, sa disponibilité dans le canal de votre projet et ses instructions de configuration. Pour un module publié :

~~~bash
symsvel require symsvel-framework/module-NOM
~~~

Remplacez NOM par le nom réel du package. Les modules obligatoires ne peuvent pas être désinstallés. Les modules facultatifs possèdent leur propre guide une fois installés ; retrouvez-les dans cette rubrique.

Brevo et Google Login sont des modules facultatifs en préparation : ne les installez pas sur la seule base de leur nom. Attendez leur disponibilité dans votre canal et leur guide validé.

Pour découvrir les modules disponibles avant leur installation, consultez les [dépôts officiels](https://github.com/orgs/Symsvel-Framework/repositories).
