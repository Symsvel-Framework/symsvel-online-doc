# Réponses API et origines

Utilisez le contrat Core pour les endpoints métier :

~~~php
use SymSvel\Core\Http\ApiResponse;

return ApiResponse::success(['id' => 1], 200);
// ou
return ApiResponse::error('not_found', 'Ressource introuvable', 404);
~~~

Une réponse de succès est enveloppée dans data ; une erreur dans error avec code et message. Pour une validation HTTP 422, fournissez les violations. Pour une réponse sans contenu, utilisez ApiResponse::noContent().

## Origines CORS

Les URL Back, BO, App et Front sont configurées par le Setup. En cas de changement d'hébergement, actualisez les origines et la configuration CORS de façon cohérente. En production, utilisez HTTPS et autorisez uniquement les origines attendues, jamais un joker avec des cookies.

CORS n'est pas une autorisation métier : contrôlez l'identité et les droits dans les endpoints protégés.
