# Publication officielle SymSvel

Ce dépôt publie uniquement les fichiers statiques de doc.symsvel.com.

## Déploiement

Le workflow `.github/workflows/deploy.yml` construit site/ puis le synchronise via SSH/rsync. Le déclenchement manuel permet le premier déploiement ; les modifications des sources sur main déclenchent les suivants. Le runner doit disposer de ssh et rsync, et le serveur de rsync. Les outils manquants sont installés sur le runner Linux si apt-get et les droits requis sont disponibles. Le port SSH doit être un nombre de 1 à 65535. Toute modification du workflow déclenche également une CD.

Secrets Actions : `SSH_HOST`, `SSH_USER`, `SSH_PORT`, `SSH_PRIVATE_KEY`, `SSH_KNOWN_HOSTS` (clé hôte vérifiée), `DEPLOY_PATH` (racine web absolue dédiée, jamais public_html). La clé SSH doit être autorisée sur le serveur. Comme pour la landing, `SSH_KNOWN_HOSTS` contient les lignes complètes de known_hosts correspondant exactement à `SSH_HOST` et `SSH_PORT` (hôte seul pour le port 22, `[hôte]:port` sinon). Les fins de ligne CRLF sont normalisées. Le workflow vérifie la correspondance, la connexion SSH, le marqueur et rsync distant séparément, sans afficher les clés ni désactiver la vérification stricte. Ne jamais versionner ces valeurs sensibles.

Avant le premier déploiement, créer dans le dossier cible un fichier `.symsvel-deploy-target` contenant exactement `symsvel-online-doc`. Le workflow refuse une cible sans marqueur correspondant. La synchronisation supprime les anciens artefacts du site, mais préserve le marqueur, .well-known/ et .htaccess. Ne placer aucun autre projet dans ce dossier.

## Actualisation des guides

Le projet local SymSvel reste la source du site : exécuter `symsvel update`, puis dans dev-doc `git add -A` et `git add -f docs/symsvel/` (le dossier généré est volontairement ignoré par le framework pour les projets ordinaires). Vérifier le contenu destiné au public avant commit/push. Ne pas modifier les guides à la main, ne jamais versionner site/ ni .venv/. Le build MkDocs strict doit réussir avant publication.

La publication HTTP(S) doit être contrôlée après le premier déploiement ; le succès de la synchronisation SSH ne constitue pas une validation HTTP.
