# Développer avec Symfony et Svelte

SymSvel n'impose pas de module pour votre code métier. Vous pouvez développer directement dans votre projet avec Symfony et SvelteKit.

## Exemple : une première API catalogue

Créez backend/src/Controller/CatalogueController.php :

~~~php
<?php

namespace App\Controller;

use Symfony\Component\Routing\Attribute\Route;
use Symfony\Component\HttpFoundation\JsonResponse;
use SymSvel\Core\Http\ApiResponse;

final class CatalogueController
{
    #[Route('/apiopen/catalogue-demo', methods: ['GET'])]
    public function index(): JsonResponse
    {
        return ApiResponse::success([
            ['id' => 1, 'name' => 'Carnet'],
            ['id' => 2, 'name' => 'Stylo'],
        ], 200);
    }
}
~~~

Depuis backend, vérifiez la route :

~~~bash
php bin/console debug:router
curl http://api.catalogue.test/apiopen/catalogue-demo
~~~

Cet exemple retourne des données publiques fictives. Pour des données privées, utilisez un scope protégé et contrôlez les autorisations côté serveur.

## Afficher les données dans le BO

Dans `backend/config/packages/nelmio_cors.yaml`, ajoutez sous `paths:` une règle pour ce point d'entrée public uniquement :

~~~yaml
        '^/apiopen/catalogue-demo$': ~
~~~

Conservez les règles existantes et les origines autorisées du Setup. Cette règle permet au navigateur du BO d'appeler cette route, sans ouvrir les autres API publiques.

Créez `bo/src/routes/catalogue-demo/+page.svelte` :

~~~svelte
<script lang="ts">
  import { onMount } from 'svelte';
  import { api } from '../../lib/api/client';

  type Product = { id: number; name: string };
  let products = $state<Product[]>([]);
  let loading = $state(true);
  let error = $state('');

  onMount(async () => {
    try {
      const result = await api<{ data: Product[] }>(
        '/apiopen/catalogue-demo',
        { public: true }
      );
      products = result.data;
    } catch {
      error = 'Impossible de charger le catalogue.';
    } finally {
      loading = false;
    }
  });
</script>

<h1>Catalogue</h1>
{#if loading}
  <p>Chargement…</p>
{:else if error}
  <p>{error}</p>
{:else}
  <ul>
    {#each products as product (product.id)}
      <li>{product.name}</li>
    {/each}
  </ul>
{/if}
~~~

Depuis `bo/`, construisez l'interface avec l'adresse réelle de votre API (exemple) :

~~~bash
PUBLIC_API_URL=http://api.catalogue.test npm run build
~~~

Servez le build avec le vhost BO configuré lors du Setup, connectez-vous et ouvrez `/catalogue-demo`. Vous devez retrouver Carnet et Stylo, cette fois lus depuis Symfony. Adaptez le domaine de l'API et du BO à votre installation. Si vous utilisez Vite en mode développement, son origine doit également être autorisée par votre configuration CORS.

## Aller plus loin

- Entités, repositories et services : [documentation Symfony](https://symfony.com/doc/current/index.html).
- Pages, composants et chargement des données : [documentation SvelteKit](https://svelte.dev/docs/kit/introduction).
- Pour vos propres tables applicatives : utilisez les migrations Doctrine sans modifier les tables gérées par les modules natifs.
- Pour une fonctionnalité réutilisable dans plusieurs projets, choisissez le [développement modulaire](02-module-metier.md).
