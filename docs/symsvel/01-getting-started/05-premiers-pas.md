# 5. Premiers pas

## Vérifier le backend

~~~bash
curl -i http://api.catalogue.test/api/health
~~~

Adaptez l'URL au projet. HTTP 200 indique que le routage PHP répond ; le healthcheck ne teste ni la base de données ni le transport mail.

## Se connecter au BO

Ouvrez l'origine BO choisie pendant le Setup. Connectez-vous avec le premier compte administrateur et suivez les instructions de double authentification lorsqu'elles sont proposées. Vous pouvez alors découvrir les modules natifs, les paramètres, les rôles et les droits.

Pour tester les invitations et confirmations par e-mail en local, démarrez le service SMTP de test choisi (par exemple Mailpit). SymSvel ne l'installe pas automatiquement.

## Votre workspace

~~~text
catalogue/
  backend/     Symfony, API, entités et services
  bo/          SvelteKit, back-office
  app/         SvelteKit, espace utilisateur (optionnel)
  front/       SvelteKit, site public (optionnel)
  modules/     Vos modules métier, après génération
  dev-doc/     Documentation locale
  admin-doc/   Documentation administrateur du projet
  user-doc/    Documentation utilisateur du projet
  secrets/     Configuration privée
~~~

Écrivez votre code classique dans backend/src et dans les sources Svelte de vos interfaces. Ne modifiez pas les fichiers dans vendor, node_modules ou les sorties générées. Le code d'un module métier reste dans son propre dossier source.

Pour reconstruire la documentation locale :

~~~bash
symsvel documentation:build-dev
~~~

Ouvrez dev-doc/site/index.html. Pour consulter le site avec un serveur HTTP local :

~~~bash
cd dev-doc/site
python3 -m http.server 8000 --bind 127.0.0.1
~~~

Passez au [développement métier](../02-development/01-code-metier.md).
