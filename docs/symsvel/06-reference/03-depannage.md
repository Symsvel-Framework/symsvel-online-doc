# Dépannage

| Symptôme | Action |
| --- | --- |
| Prérequis manquant | Relancez symsvel doctor |
| Wizard interrompu | Relancez symsvel setup ; consultez le diagnostic |
| Backend inaccessible | Vérifiez l'origine et le document root backend/public ; testez /api/health |
| Erreur CORS | Vérifiez l'origine réelle du navigateur et les variables de configuration |
| Mail non reçu | Vérifiez le transport choisi, son service et les logs |
| Twig ne peut pas créer un dossier de cache | Vérifiez les droits CLI/PHP-FPM |
| Mise à jour interrompue | Conservez les checkpoints ; lisez le message de récupération |

## Cache et logs locaux

Avec la CLI 0.3.1 ou ultérieure, depuis le workspace :

~~~bash
symsvel permissions:fix
~~~

Si plusieurs comptes PHP-FPM existent, sélectionnez le compte correspondant à votre projet avec --http-user=COMPTE. Sur Linux, setfacl doit être disponible. Si des fichiers ont été créés par un autre compte avant la pose des ACL, une intervention ciblée du propriétaire ou de l'administrateur local peut être nécessaire.

Ne rendez jamais l'ensemble du projet accessible en écriture à tous. Les secrets et les fichiers de base SQLite sont exclus de cette réparation.
