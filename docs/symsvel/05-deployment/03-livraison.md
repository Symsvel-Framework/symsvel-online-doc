# Livrer les interfaces et maintenir la production

## Livraison automatisée (recommandée)

Dans chaque dépôt Svelte, préparez un workflow GitHub Actions deploy.yml sur main, déclenchable manuellement (workflow_dispatch). Le workflow doit :

1. Récupérer les sources du projet.
2. Installer Node.js et les dépendances verrouillées avec npm ci.
3. Compiler avec PUBLIC_API_URL pointant vers le backend de production.
4. Publier le dossier build vers la racine web de cette interface (SSH/rsync ou mécanisme de votre hébergeur).

Stockez les clés SSH dans les secrets GitHub, épinglez la clé d'hôte dans known_hosts et utilisez un compte de déploiement limité aux répertoires concernés. N'injectez jamais de secret privé dans une variable PUBLIC_.

## Livraison manuelle des fichiers statiques

Le même build peut être livré manuellement :

~~~bash
npm ci
PUBLIC_API_URL=https://api.example.com npm run build
~~~

Transférez le contenu de build avec votre outil de publication sécurisé (par exemple rsync via SSH) vers le document root prévu. Vérifiez l'adresse de destination avant le transfert.

Cette méthode peut servir pour les publications ultérieures. Le premier wizard de production actuel attend des workflows CD pour les interfaces déclarées ; le déploiement manuel intégral sans CD n'est pas présenté comme un parcours automatisé pris en charge.

## Actualiser la production

Validez les mises à jour SymSvel en local, sauvegardez la production, puis publiez vos changements Git. Déployez le backend, ses dépendances verrouillées et les migrations nécessaires, puis les interfaces compatibles. Vérifiez les permissions, les logs et les parcours métier après chaque livraison. Ne lancez pas une mise à jour générale des dépendances directement sur le serveur de production.
