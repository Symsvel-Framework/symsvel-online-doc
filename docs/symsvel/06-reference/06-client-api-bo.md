# Utiliser le client API du BO

Les modules BO utilisent le client natif de l'interface. Il applique l'origine backend, les préférences de langue, le format de réponse et la gestion de session.

## Appel authentifié

Dans une page de premier niveau d'un module (après synchronisation) :

```typescript
import { boApi } from '../../../../api/session';

const items = await boApi<{ id: number; name: string }[]>('/apibo/catalogue');
```

Cet exemple suppose que votre module a déclaré et protégé le endpoint correspondant. Adaptez le chemin relatif à la profondeur de la page.

Pour les requêtes publiques, utilisez api du fichier src/lib/api/client.ts avec l'option public: true. Les requêtes publiques n'envoient ni Bearer ni cookie. Le client BO gère le renouvellement de session ; ne stockez pas vous-même des tokens dans localStorage.

## Configuration

Définissez PUBLIC_API_URL avec l'origine réelle du backend avant la compilation. Cette variable publique n'est pas un emplacement pour des secrets. Les origines des interfaces sont autorisées côté backend.

Le préfixe /apibo n'est pas à lui seul un contrôle d'accès : le serveur doit vérifier l'identité et les ACL. Un ApiError contient le statut HTTP et, lorsqu'il est disponible, le code d'erreur API.
