# TwoFactor

Module natif qui ajoute la double authentification TOTP aux comptes du BO et de l'App.

## Première connexion

Après avoir fourni vos identifiants, suivez les instructions d'enrôlement. Scannez le QR code avec une application TOTP ou saisissez la clé manuellement, puis fournissez le code affiché. Conservez les codes de secours hors du projet et dans un emplacement sûr. Ne transmettez jamais la clé d'enrôlement ni les codes à un tiers.

## Configuration

| Paramètre | Rôle |
| --- | --- |
| BO | Second facteur obligatoire, pas de paramètre de désactivation |
| two-factor.app_required | Second facteur obligatoire pour App si activé |

Un compte App déjà enrôlé doit continuer à présenter son second facteur. Le BO propose les fonctions de gestion prévues selon les autorisations du compte.

## Intégration

Le flux de connexion Auth peut retourner HTTP 202 avec un challenge temporaire plutôt qu'une session. Ne considérez pas cette réponse comme une authentification réussie. Utilisez les interfaces natives ou les endpoints de validation :

- POST /apibo/auth/browser/second-factor pour BO.
- POST /apiopen/auth/second-factor pour App.

La session est émise seulement après vérification. Le QR code est généré localement, sans transmettre le secret à un service externe. Ne dupliquez pas ce mécanisme dans votre module métier.
