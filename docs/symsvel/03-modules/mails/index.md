# Mails

Module natif : envoi des messages transactionnels, expéditeur commun et sélection du transport. Il est utilisé notamment par Auth pour les invitations et confirmations.

## Configuration

Dans le BO, ouvrez Settings puis Mails :

| Clé | Rôle |
| --- | --- |
| mails.sender_email | Adresse expéditrice valide, obligatoire |
| mails.sender_name | Nom affiché de l'expéditeur |
| mails.transport | Transport actif, obligatoire |

Choisissez un seul transport. Les options natives sont SMTP personnalisé (smtp), Mailpit (mailpit), MailHog (mailhog), smtp4dev (smtp4dev) et Mailtrap (mailtrap). Le service choisi doit être disponible ; le module ne l'installe pas automatiquement.

Pour Mailpit local, démarrez le service SMTP à l'adresse prévue par le backend. Pour SMTP personnalisé, configurez MAILER_DSN dans backend/.env.local ou dans l'environnement serveur. Mailtrap utilise MAILTRAP_DSN. Ne publiez jamais les identifiants SMTP. Des modules facultatifs peuvent ajouter d'autres transports.

## Envoyer un message métier

Injectez SymSvel\Module\Mails\Mail\ConfiguredMailSender dans votre service. Il utilise l'expéditeur et le transport sélectionnés dans Settings.

~~~php
use SymSvel\Module\Mails\Mail\RenderedMail;

$mail = new RenderedMail(
    'Votre commande',
    '<p>Votre commande est enregistrée.</p>',
    'Votre commande est enregistrée.',
);
$sender->send($mail, $recipient);
~~~

Pour des contenus réutilisables et traduits, déclarez un MailTemplateProviderInterface dans votre module puis utilisez RegisteredMailRenderer::render(id, locale, variables). Le renderer vérifie les variables et utilise Twig ; le sender effectue l'envoi. Un transport indisponible ou une configuration manquante doit être traité comme une erreur, pas comme une livraison réussie.
