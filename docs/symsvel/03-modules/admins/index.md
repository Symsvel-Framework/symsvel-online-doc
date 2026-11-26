# Admins

Module natif, installé dans tous les projets. Il gère les administrateurs du BO ; les utilisateurs de l'App relèvent de Users.

## Utiliser le module

Connectez-vous au BO avec votre compte administrateur. Dans Administrateurs, consultez les comptes, invitez un nouveau membre, sélectionnez un rôle autorisé et activez ou désactivez son accès. L'invité reçoit un lien pour définir son mot de passe. Configurez le transport Mails avant d'envoyer une invitation. Le premier compte créé au Setup possède ROLE_DEV ; les autres comptes ne peuvent pas s'attribuer ce rôle.

## Configuration

Aucune installation ni configuration supplémentaire propre au module. Les rôles proviennent de Roles, les autorisations d'ACLs, les sessions et invitations d'Auth. Les adresses et la langue des comptes se gèrent dans le BO.

## Intégration

- GET /apibo/admins/invitations : invitations, sous réserve d'autorisation.
- POST /apibo/admins/invitations : inviter un administrateur.
- POST /apibo/admins/{id}/invitations/resend : renvoyer une invitation.

Utilisez les services et contrôleurs existants plutôt que d'écrire directement dans symsvel_admins. Pour protéger un endpoint métier du BO, utilisez [ACLs](../acls/index.md).


## Profil personnel

Le profil BO conserve le shell de son interface. Les informations de compte et le changement d’adresse sont regroupés dans un bloc ; le changement de mot de passe possède un second bloc indépendant. Les deux blocs sont côte à côte si la largeur le permet, puis empilés sur écran étroit. Le choix de langue utilise `/apiopen/languages`, qui respecte `languages.multilingual`, et ne s’affiche que lorsque plusieurs langues sont disponibles. Les parcours de confirmation et de sécurité restent inchangés.


### Scopes natifs

Back et BO uniquement. Les interfaces administrateur sont exclusivement dans le BO. Les anciennes routes de test App/Front ne sont plus distribuées.
