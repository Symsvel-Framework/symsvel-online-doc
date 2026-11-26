# Auth

Module natif pour l'authentification BO et App : connexion, renouvellement de session, déconnexion, invitations, confirmation d'adresse et récupération du mot de passe.

## Configuration

Les URL des liens envoyés par e-mail dérivent des origines SYMSVEL_BO_ORIGIN et SYMSVEL_APP_ORIGIN configurées au Setup. Elles ne se modifient pas dans Settings. Configurez Mails pour les invitations et récupérations. TwoFactor applique la politique du second facteur.

## Connexion et sécurité

L'interface BO conserve le token d'accès en mémoire ; le renouvellement passe par un cookie HttpOnly. En production, HTTPS est requis. N'enregistrez pas les tokens dans localStorage et ne remplacez pas les contrôles de session natifs par des vérifications côté client.

| Endpoint BO | Rôle |
| --- | --- |
| POST /apibo/auth/browser/login | Connexion |
| POST /apibo/auth/browser/refresh | Renouvellement |
| POST /apibo/auth/browser/logout | Déconnexion |
| POST /apibo/auth/browser/second-factor | Validation du second facteur |

Un challenge HTTP 202 exige la validation du second facteur avant l'émission de la session. Utilisez le client API et les parcours de l'interface hôte. Les identités BO et App sont séparées ; un token BO ne doit pas être utilisé comme identité App.

## Mot de passe

Les nouveaux mots de passe doivent respecter les critères affichés par les formulaires : au moins 12 caractères, une majuscule, une minuscule, un chiffre et un caractère spécial. Les liens d'activation et de récupération sont temporaires et à usage unique.

Pour vos endpoints métier BO, utilisez le [contrôle ACL](../acls/index.md). Pour l'espace utilisateur, voir Users lorsque ce module est installé.
