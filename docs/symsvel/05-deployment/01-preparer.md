# Préparer le déploiement

SymSvel utilise un backend PHP et des interfaces SvelteKit statiques. Vous pouvez les héberger sur un même serveur ou les répartir sur plusieurs hébergements.

| Composant | Racine HTTP | Besoins |
| --- | --- | --- |
| Back | backend/public | PHP 8.4+, extensions, base de données, HTTPS |
| BO | dossier de build | Serveur de fichiers statiques, HTTPS |
| App | dossier de build | Idem, si installée |
| Front | dossier de build | Idem, si installé |

Node et npm servent à compiler les interfaces sur votre poste ou sur un runner CI/CD ; pas de serveur Node obligatoire en production. Configurez le serveur web pour servir les routes de l'application statique conformément à son adaptateur SvelteKit.

## 1. Préparer vos dépôts

Créez vos propres dépôts Git pour les composants du projet. Versionnez les sources et les verrous Composer/npm ; excluez vendor, node_modules, build, .env.local, secrets et les états privés .symsvel. Vérifiez git status avant le premier push.

## 2. Préparer vos origines

Créez les DNS, certificats TLS et hôtes virtuels correspondant à vos URL. Exemple :

- https://api.example.com : backend
- https://bo.example.com : BO
- https://app.example.com : App
- https://www.example.com : Front

Ces origines peuvent pointer vers des hébergeurs différents. Les liens et CORS seront configurés avec ces URL réelles au Setup.

## 3. Préparer la livraison des interfaces

Le wizard de production actuel déclenche les workflows deploy.yml des dépôts d'interface renseignés sur main. Préparez ces workflows avant de lancer le Setup. Ils doivent accepter workflow_dispatch, compiler avec PUBLIC_API_URL et publier les fichiers sur leur hébergement.

[Continuer : installer le backend](02-setup-production.md).
