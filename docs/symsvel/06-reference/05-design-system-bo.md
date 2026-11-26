# Développement BO : composants natifs et Design System

> Référence des composants publics pour les auteurs de modules (scope `bo`).

## 1. Principe : les modules consomment, le BO hôte dessine

Les pages d'un module apportent leurs données, appels `apibo`, contrôles et
traductions. Le BO hôte apporte l'apparence, le thème, les composants partagés,
la navigation et les interactions visuelles génériques. **Ne pas copier les
composants du BO dans un module**, ni y déclarer une palette de couleurs
concurrente.

Une route source du module `bo/src/routes/exemple/+page.svelte` est synchronisée
par Tooling sous `src/lib/modules/<slug>/routes/**` dans l'interface BO.
Les imports utilisés dans les modules natifs suivent le chemin relatif
`../../../../components/...` depuis une page de premier niveau :

```svelte
<script lang="ts">
	import Button from '../../../../components/Button.svelte';
	import Alert from '../../../../components/Alert.svelte';
	import Dropdown from '../../../../components/Dropdown.svelte';
	import DataTable from '../../../../components/DataTable.svelte';
	import type { DataAction, DataColumn } from '../../../../components/data-table';
	import { boApi } from '../../../../api/session';
</script>
```

Si la route possède davantage de niveaux, **adapter le chemin relatif**.
Ne pas créer d'alias absolu différent de celui du BO. Les pages de modules
n'instancient jamais `AppShell` : il appartient au layout hôte.
Les autorisations sont vérifiées par le Back, pas déduites d'une couleur ou
d'une option visuelle.

### Inventaire des composants publics

| Composant      | Fichier hôte                   | Fonction                                                 |
| -------------- | ------------------------------ | -------------------------------------------------------- |
| **Button**     | `components/Button.svelte`     | Bouton d'action, variantes, attente, état indisponible   |
| **Alert**      | `components/Alert.svelte`      | Message d'erreur, de succès ou informatif                |
| **InputField** | `components/InputField.svelte` | Champ HTML lié avec libellé associé                      |
| **Dropdown**   | `components/Dropdown.svelte`   | Choix contrôlé, clavier et options enrichies             |
| **Modal**      | `components/Modal.svelte`      | Dialogue natif, contenu libre, fermeture contrôlée       |
| **DataTable**  | `components/DataTable.svelte`  | Tableau avec tri déclaré, filtres, actions et pagination |
| **Pagination** | `components/Pagination.svelte` | Sélecteur 10/20/50/100 et navigation de pages            |

`AppShell.svelte` est un composant **interne au BO hôte**, pas une primitive
à importer dans les modules. Il n'existe pas encore de composant public
`Badge`, `Tabs` ou `Accordion` : ne pas documenter de props fictives.

## 2. Couleurs sémantiques : pourquoi, et comment les appeler

**Utiliser `variant` sur `Button` et dans `DataAction`.**
La classe CSS correspondante (`.bo-button-success`, etc.) est produite par
le composant. On ne la colle pas manuellement sur un bouton natif de module
pour contourner ce contrat. Les CSS centralisés sont dans
`symsvel-bo/src/app.css`.

| `variant`   | Usage conseillé                                             | Raison de la couleur                                               | Jeton principal              |
| ----------- | ----------------------------------------------------------- | ------------------------------------------------------------------ | ---------------------------- |
| `primary`   | Action principale, créer, avancer                           | Bleu pétrole : appel à l'action neutre, sans annoncer de succès    | `--bo-primary`               |
| `secondary` | Éditer, annuler, revenir, rafraîchir                        | Neutre : action secondaire, non destructive                        | `--bo-surface` / `--bo-text` |
| `success`   | Enregistrer, valider, autoriser, activer                    | Vert : confirmation ou effet positif de l'action                   | `--bo-action-success`        |
| `danger`    | Supprimer, détruire définitivement                          | Rouge : perte ou conséquence destructive à examiner                | `--bo-danger`                |
| `warning`   | Désactiver, suspendre, révoquer un droit réversible         | Orange : retrait réversible demandant l'attention                  | `--bo-action-warning`        |
| `active`    | Afficher une **action** explicitement liée à l'état actif   | Bleu : signale une situation active, sans suggérer une suppression | `--bo-action-active`         |
| `inactive`  | Afficher une **action** explicitement liée à l'état inactif | Orange : signale une situation désactivée                          | `--bo-action-inactive`       |

La couleur s'applique à la **finalité du bouton**. Par exemple, si un pays
est actif, l'action « Désactiver » utilise `warning`, **pas** `active` ;
si le pays est inactif, l'action « Activer » utilise `success`.
L'état actuel peut être écrit en texte « Actif » / « Inactif » dans
une cellule indépendante. Ne pas confondre « révoquer temporairement »
(`warning`) et « supprimer définitivement » (`danger`).

**Priorité des états :** `disabled` ou `loading` prime toujours sur
`variant`. En clair, le bouton désactivé devient gris clair avec du texte
gris suffisamment contrasté. En sombre, fond gris moyen et texte
`rgba(255, 255, 255, 0.8)`. Un blanc semi-transparent sur du gris trop pâle
serait illisible. Le contraste et l'état de focus restent obligatoires ;
une couleur seule ne doit jamais être l'unique information.

```svelte
<Button variant="primary" onclick={create}>Créer</Button>
<Button variant="secondary" onclick={cancel}>Annuler</Button>
<Button variant="success" type="submit" loading={saving}>Enregistrer</Button>
<Button variant="danger" onclick={remove} disabled={!canDelete}>Supprimer</Button>
<Button variant="warning" onclick={disable}>Désactiver</Button>
```

Ne pas coder `background: #...` pour un bouton, ni utiliser la variante
`danger` pour un simple champ invalide : employer `Alert kind="error"`.

### Référence des couleurs et surfaces

Les jetons sont résolus automatiquement par le thème clair, sombre ou
`prefers-color-scheme`. **Ne pas recopier leurs valeurs hexadécimales dans
le module**. Les couleurs ont une signification visuelle distincte :

| Famille             | Jetons CSS                                                                                                         | Pourquoi                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------- |
| Surfaces            | `--bo-bg`, `--bo-surface`, `--bo-surface-muted`                                                                    | Séparer fond, panneau et sous-zone sans inventer de blanc fixe |
| Champs              | `--bo-input`, `--bo-border`                                                                                        | Assurer une saisie lisible sur les deux thèmes                 |
| Textes              | `--bo-text`, `--bo-muted`                                                                                          | Hiérarchie de lecture, aide et métadonnées                     |
| Navigation / accent | `--bo-primary`, `--bo-primary-contrast`                                                                            | Identité visuelle et actions primaires                         |
| Focus               | `--bo-focus`                                                                                                       | Repère clavier distinct du survol                              |
| Suppression         | `--bo-danger`, `--bo-danger-contrast`                                                                              | Action destructive                                             |
| Actions             | `--bo-action-success`, `--bo-action-warning`, `--bo-action-active`, `--bo-action-inactive`, `--bo-action-contrast` | Finalité et état des boutons                                   |
| Désactivé           | `--bo-action-disabled-bg`, `--bo-action-disabled-fg`, `--bo-action-disabled-border`                                | État indisponible prioritaire                                  |
| Messages            | `--bo-success-bg/text`, `--bo-error-bg/text`, `--bo-info-bg/text`                                                  | Retours d'opération, **pas** boutons                           |
| Effet de surface    | `--bo-shadow`                                                                                                      | Élévation commune des panneaux/dialogues                       |

Pour les jetons de messages, employer les noms complets :
`--bo-success-bg` et `--bo-success-text`, `--bo-error-bg` et
`--bo-error-text`, `--bo-info-bg` et `--bo-info-text`.
La notation `-bg/text` dans le tableau est seulement un raccourci.

Exemple d'un état textuel **sans inventer de composant Badge** :

```svelte
<span class="account-state" class:is-active={account.enabled}>
	{account.enabled ? t.active : t.inactive}
</span>

<style>
	.account-state {
		color: var(--bo-muted);
	}
	.account-state.is-active {
		color: var(--bo-success-text);
	}
</style>
```

Garder le libellé et, lorsque pertinent, une icône : l'information ne
doit pas être véhiculée par la couleur seule.

## 3. Contrat détaillé des composants

### Button

```svelte
<Button type="submit" variant="success" loading={saving} disabled={!valid} class="my-layout-class">
	{t.save}
</Button>
```

| Prop                                   | Type / défaut                     | Comportement                                                          |
| -------------------------------------- | --------------------------------- | --------------------------------------------------------------------- |
| `children`                             | `Snippet` obligatoire             | Contenu affiché hors attente                                          |
| `type`                                 | HTML `button` / défaut `"button"` | **Jamais de submit implicite** ; passer `"submit"` dans un formulaire |
| `variant`                              | `BoButtonVariant` / `"primary"`   | Sept variantes définies dans `components/button.ts`                   |
| `loading`                              | `boolean` / `false`               | Désactive le bouton et affiche « Patientez… » traduit par le BO       |
| `disabled`                             | Attribut HTML / `false`           | Forcé également par `loading`                                         |
| `onclick`, `aria-*`, `title`, `class`… | `HTMLButtonAttributes`            | Attributs HTML standard, sans API métier ajoutée                      |

Les libellés des modules viennent de leurs dictionnaires, pas d'une
chaîne française codée en dur. `class` sert au layout (espacement,
largeur), **pas** à remplacer la sémantique de `variant`.

### Alert

```svelte
{#if error}<Alert kind="error">{error}</Alert>{/if}
{#if notice}<Alert kind="success">{notice}</Alert>{/if}
<Alert kind="info">{t.help}</Alert>
```

| Prop       | Type / défaut                                | Comportement                      |
| ---------- | -------------------------------------------- | --------------------------------- |
| `kind`     | `"error" \| "success" \| "info"` / `"error"` | Fond et texte via jetons du thème |
| `children` | `Snippet` obligatoire                        | Contenu du message                |

`error` rend `role="alert"` ; `success` et `info` rendent
`role="status"`. Ce composant n'a ni `variant` ni prop `onClose`.

### InputField

```svelte
<InputField
	id="module-label"
	label={t.label}
	type="text"
	bind:value={label}
	required
	maxlength={150}
/>
```

| Prop                                                   | Type / défaut                            | Comportement                  |
| ------------------------------------------------------ | ---------------------------------------- | ----------------------------- |
| `id`                                                   | `string` obligatoire                     | Associe le `label` au champ   |
| `label`                                                | `string` obligatoire                     | Texte affiché                 |
| `value`                                                | `string` bindable / `""`                 | État réactif via `bind:value` |
| `type`                                                 | `HTMLInputAttributes["type"]` / `"text"` | Type de saisie HTML           |
| `required`, `disabled`, `placeholder`, `autocomplete`… | `HTMLInputAttributes`                    | Attributs natifs disponibles  |

`InputField` est un champ texte HTML, pas un sélecteur, un validateur
métier, ni un widget de mot de passe complet. La validation et les
erreurs du module restent sous la responsabilité du parent et du Back.

### Dropdown

```svelte
<script lang="ts">
	let country = $state('FR');
	const options = [
		{ value: 'FR', label: 'France', code: 'FR', icon: '🇫🇷' },
		{ value: 'BE', label: 'Belgique', code: 'BE', icon: '🇧🇪' }
	];
</script>

<Dropdown
	value={country}
	{options}
	label={t.country}
	placeholder={t.choose}
	disabled={saving}
	onChange={(value) => {
		country = value;
	}}
/>
```

| Prop          | Type / défaut                         | Comportement                                                     |
| ------------- | ------------------------------------- | ---------------------------------------------------------------- |
| `value`       | `string` obligatoire                  | Option courante, contrôlée par le parent                         |
| `options`     | `DropdownOption[]` obligatoire        | `{ value: string, label: string, code?: string, icon?: string }` |
| `label`       | `string` obligatoire                  | Libellé accessible du déclencheur                                |
| `onChange`    | `(value: string) => void` obligatoire | Appelé seulement si la sélection change                          |
| `disabled`    | `boolean` / `false`                   | Interdit l'ouverture et la sélection                             |
| `placeholder` | `string` / `"—"`                      | Affiché quand aucune valeur ne correspond                        |

Navigation clavier : flèches, Home/End, Entrée/Espace, Échap et Tab.
Le panneau possède `role="listbox"`. Le composant ne crée pas de
`<select>` HTML caché et ne soumet pas de valeur de formulaire à votre
place : le parent soumet sa propre valeur dans l'appel API.

**Attention aux accordéons :** le menu est positionné en `absolute`.
Éviter `overflow: hidden` et les contextes empilés qui le coupent ;
le BO hôte a déjà des règles de visibilité pour les listes de Settings.
Tester le menu ouvert devant les éléments suivants.

### Modal

```svelte
<Modal
	open={editing !== null}
	title={t.edit}
	onClose={() => {
		editing = null;
	}}
>
	<form onsubmit={save}>
		<InputField id="name" label={t.name} bind:value={name} required />
		<Button
			type="button"
			variant="secondary"
			onclick={() => {
				editing = null;
			}}
		>
			{t.cancel}
		</Button>
		<Button type="submit" variant="success" loading={saving}>{t.save}</Button>
	</form>
</Modal>
```

| Prop       | Type                     | Comportement                                                             |
| ---------- | ------------------------ | ------------------------------------------------------------------------ |
| `open`     | `boolean` obligatoire    | Synchronise l'ouverture de `<dialog>` natif                              |
| `title`    | `string` obligatoire     | Titre relié au dialogue par `aria-labelledby`                            |
| `onClose`  | `() => void` obligatoire | Fermer via Échap ou bouton ✕ ; **remettre `open` à faux dans le parent** |
| `children` | `Snippet` obligatoire    | Contenu libre, formulaire inclus                                         |

Le composant ne soumet pas de formulaire, ne crée pas d'API
et ne choisit pas les traductions du module.

### Pagination

```svelte
<Pagination
	{page}
	{limit}
	total={filtered.length}
	{loading}
	onPageChange={(next) => {
		page = next;
	}}
	onLimitChange={(next) => {
		limit = next;
		page = 1;
	}}
/>
```

| Prop            | Type / défaut                           | Comportement                                    |
| --------------- | --------------------------------------- | ----------------------------------------------- |
| `page`          | `number` obligatoire                    | Index **commençant à 1**                        |
| `limit`         | `10 \| 20 \| 50 \| 100` / `20`          | Nombre de lignes par page                       |
| `total`         | `number` obligatoire                    | Taille de la liste filtrée, **avant découpage** |
| `loading`       | `boolean` / `false`                     | Verrouille les interactions                     |
| `onPageChange`  | `(page: number) => void` obligatoire    | Le parent met à jour son état                   |
| `onLimitChange` | `(limit: PageSize) => void` obligatoire | Le parent ajuste taille et page                 |

Ne pas cumuler une pagination autonome avec celle de `DataTable`
si ce dernier reçoit déjà sa prop `pagination`.

### DataTable

```svelte
<script lang="ts">
	import DataTable from '../../../../components/DataTable.svelte';
	import type { DataAction, DataColumn, SortOrder } from '../../../../components/data-table';
	import type { PageSize } from '../../../../components/pagination';

	type Row = { id: number; name: string; enabled: boolean };
	let rows = $state<Row[]>([]);
	let page = $state(1);
	let limit = $state<PageSize>(20);
	let sortOrder = $state<SortOrder>('asc');
	let busy = $state(false);

	const columns: DataColumn<Row>[] = [
		{ key: 'name', label: 'Nom', sortable: true },
		{ key: 'enabled', label: 'État', format: (_, row) => (row.enabled ? 'Actif' : 'Inactif') }
	];
	const actions: DataAction<Row>[] = [
		{
			key: 'toggle',
			label: (row) => (row.enabled ? 'Désactiver' : 'Activer'),
			variant: (row) => (row.enabled ? 'warning' : 'success'),
			disabled: () => busy,
			onClick: (row) => {
				void toggleFromApi(row);
			}
		}
	];

	// Exemple de logique métier à fournir par votre module.
	declare function toggleFromApi(row: Row): Promise<void>;

	const sorted = $derived(
		[...rows].sort((a, b) =>
			sortOrder === 'asc' ? a.name.localeCompare(b.name) : b.name.localeCompare(a.name)
		)
	);
	const visible = $derived(sorted.slice((page - 1) * limit, page * limit));
</script>

<DataTable
	rows={visible}
	{columns}
	{actions}
	rowKey="id"
	loading={busy}
	sortKey="name"
	{sortOrder}
	onSortChange={(_, order) => {
		sortOrder = order;
		page = 1;
	}}
	pagination={{
		page,
		limit,
		total: rows.length,
		onPageChange: (next) => {
			page = next;
		},
		onLimitChange: (next) => {
			limit = next;
			page = 1;
		}
	}}
/>
```

L'exemple montre le **contrat des props**, pas un module prêt à exécuter :
`toggleFromApi` doit appeler l'API réelle de votre module puis remplacer la
ligne concernée en mémoire. Le tri est ici appliqué au tableau `sorted`
**avant** `slice` ; `DataTable` ne trie pas les données par magie.

| Prop             | Type / défaut                                                    | Comportement                                                |
| ---------------- | ---------------------------------------------------------------- | ----------------------------------------------------------- |
| `rows`           | `T[]` obligatoire                                                | **Lignes déjà préparées et paginées** par le parent         |
| `columns`        | `DataColumn<T>[]` obligatoire                                    | `key`, `label`, `sortable?`, `align?`, `format?(value,row)` |
| `actions`        | `DataAction<T>[]` / `[]`                                         | Actions déclaratives, boutons du Design System              |
| `filters`        | `DataFilter[]` / `[]`                                            | Filtres `search` ou `select`, non appliqués automatiquement |
| `filterValues`   | `Record<string,string>` / `{}`                                   | Valeurs contrôlées des filtres                              |
| `sortKey`        | `string \| null` / `null`                                        | Colonne triée pour l'affichage de l'indicateur              |
| `sortOrder`      | `"asc" \| "desc"` / `"asc"`                                      | Ordre sélectionné                                           |
| `pagination`     | `{ page, limit, total, onPageChange, onLimitChange }` facultatif | Affiche la pagination partagée                              |
| `loading`        | `boolean` / `false`                                              | Désactive tri, filtres et actions                           |
| `emptyMessage`   | `string` optionnel                                               | Message lorsque `rows` est vide                             |
| `rowKey`         | `string` / `"id"`                                                | Clé stable pour les lignes                                  |
| `onSortChange`   | `(key, order) => void` optionnel                                 | Signale la demande de tri ; le parent applique le tri       |
| `onFilterChange` | `(key, value) => void` optionnel                                 | Signale un changement ; le parent filtre les données        |

#### Détail de `DataAction<T>`

```ts
type DataAction<T extends object> = {
	key: string;
	label: string | ((row: T) => string);
	onClick: (row: T) => void;
	variant?: BoButtonVariant | ((row: T) => BoButtonVariant);
	disabled?: (row: T) => boolean;
};
```

Une action sans variante utilise `secondary` (neutre).
Une suppression doit déclarer `variant: 'danger'` ;
un toggle peut utiliser une fonction recevant la ligne.
Ne jamais interpréter les mots « supprimer », « désactiver » ou
leurs traductions pour décider de la couleur dans le composant.

#### Détail de `DataFilter` et `DataColumn`

```ts
type DataFilter = {
	key: string;
	label: string;
	type: 'search' | 'select';
	placeholder?: string;
	options?: { value: string; label: string }[];
};
type DataColumn<T extends object> = {
	key: string;
	label: string;
	sortable?: boolean;
	align?: 'left' | 'center' | 'right';
	format?: (value: unknown, row: T) => string;
};
```

Pour un filtre `select`, `DataTable` utilise le `Dropdown` hôte,
mais le tableau ne fait **pas** de requête API à votre place.
`format` retourne une chaîne, pas du HTML arbitraire. Le parent
conserve la responsabilité de la sécurité, du filtrage, du tri,
de la pagination et des mises à jour locales après sauvegarde.

## 4. Layout et CSS dans une page de module

```svelte
<div class="module-page">
	<header class="page-heading">
		<h1>{t.heading}</h1>
		<Button variant="secondary" onclick={refresh}>{t.refresh}</Button>
	</header>
	<section class="panel">
		<h2>{t.list}</h2>
		<!-- Composants natifs ici -->
	</section>
</div>

<style>
	.module-page {
		max-width: 1200px;
		margin: auto;
		display: grid;
		gap: 20px;
	}
	.page-heading {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 12px;
	}
	.panel {
		background: var(--bo-surface);
		color: var(--bo-text);
		border: 1px solid var(--bo-border);
		border-radius: 14px;
		padding: 24px;
		min-width: 0;
	}
</style>
```

`.panel` et `.page-heading` sont des **conventions de page** présentes
dans les modules, pas des composants Svelte ni des classes dotées de
props. `.bo-button-*`, `.bo-dropdown__*` et `.bo-data-table-*` sont
des classes **internes aux composants** : ne pas les copier dans les
pages pour inventer des variantes. Seules les variables CSS sémantiques
et les props publiques constituent le contrat réutilisable.

Les nouvelles pages utilisent `--bo-surface`, jamais `#fff` en dur :
sinon panneaux blancs et textes clairs se mélangent en mode sombre
lorsqu'un module et l'hôte n'ont pas la même version.

## 5. Checklist avant une PR de module

- Le module importe les **composants hôtes** et ne les duplique pas.
- L'action principale est `primary`, la validation `success`, la suppression
  `danger`, la désactivation réversible `warning` ; les actions neutres
  restent `secondary`. Un bouton `disabled` reste gris.
- `Button` a `type="button"` par défaut ; les vrais formulaires utilisent
  `type="submit"` et `onsubmit` avec `event.preventDefault()`.
- Les couleurs suivent des variables CSS, testées en **clair et sombre**,
  y compris menu `Dropdown` dans une modale et un accordéon.
- L'API et les ACL du module restent côté Symfony, les interactions utilisent
  le client `boApi`, jamais un `fetch` libre contournant la session.
- Tous les textes affichés viennent des **traductions du module**.
- La page utilise des éléments accessibles : labels, focus, clavier,
  états lisibles sans dépendre uniquement de la couleur.
- Le tri, le filtrage et le découpage des lignes sont effectués par le parent.
- Vérifier les vues et les interactions dans les deux thèmes.

### Sources du contrat

- `src/lib/components/{Button,Alert,InputField,Dropdown,Modal,DataTable,Pagination}.svelte`
- `src/lib/components/{button,data-table,pagination,dropdown}.ts`
- `src/app.css`, pour les jetons clair/sombre et les variantes
- [Sources du BO](https://github.com/Symsvel-Framework/symsvel-bo)
- [Contrat de synchronisation des modules](https://github.com/Symsvel-Framework/symsvel-tooling/blob/main/docs/modules/contrat.md)

### PasswordFeedback

Les formulaires de choix d'un nouveau mot de passe importent `passwordStrength` et `PasswordFeedback.svelte` depuis `lib/password/` (chemin relatif adapté à la profondeur du module). Le composant reçoit `{password}`, affiche cinq critères FR/EN rouges ou verts avec un symbole textuel, puis une jauge indicative accessible. Le résultat `valid` exige 12 points de code Unicode minimum, une majuscule, une minuscule, un chiffre et un signe de ponctuation ou symbole. Le plafond de 1024 octets UTF-8 et l'absence de NUL restent des garde-fous. Cette validation d'interface ne remplace jamais la politique Auth côté serveur. Les mots de passe existants ne sont pas soumis à ces critères lors de la connexion.
