# Mettre à jour son application

Avant une mise à jour, lisez les releases concernées, sauvegardez votre base et validez vos changements dans vos dépôts Git applicatifs. Travaillez d'abord sur un environnement local ou de préproduction.

## Packages Composer et composants

Depuis le workspace :

~~~bash
symsvel update
~~~

Cette commande met à jour les dépendances Composer selon les contraintes du projet et son canal. Elle ne met pas automatiquement à jour les squelettes natifs Back et Svelte.

Pour mettre à jour un seul package :

~~~bash
symsvel update symsvel-framework/module-auth
~~~

Pour actualiser les composants présents :

~~~bash
symsvel update back
symsvel update bo
symsvel update app
symsvel update front
~~~

N'exécutez que les commandes des interfaces installées. Les mises à jour de composants comparent les fichiers natifs à leur provenance ; les conflits sont signalés au lieu d'écraser silencieusement votre code.

Si une opération échoue, lisez le diagnostic et conservez les checkpoints. Ne relancez pas une opération destructive avant d'avoir résolu l'état de récupération. Les sauvegardes automatiques ne couvrent pas tous les moteurs ni les effets externes.

## Vérifier et publier

Relancez vos tests métier, vérifiez les migrations, l'authentification, les pages et les e-mails. Examinez git diff dans chaque dépôt. Une fois les vérifications effectuées, publiez vos propres commits et déployez selon votre stratégie habituelle.

Le canal du framework est indépendant de APP_ENV : passer une application en production ne change pas ses contraintes de packages.
