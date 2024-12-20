# Fork from svelte-algolia-instantsearch

This lib is a temporary fork of **Aymeric Giraudet** [svelte-algolia-instantsearch](https://github.com/aymeric-giraudet/svelte-algolia-instantsearch) library.

It fixes the routing in SvelteKit > 2 that uses his own pushState and replaceState function instead of history.pushState() / history.replaceState().

## Fixes

- Adds a default stateMapping: singleIndex(indexName) to prevent the index name to appear in the search params.
- Replace the default routing history.pushState() with SK replaceState navigation.

Allows us to use the **Back button** when navigating away from the search page and get back to the filtered view.

## Documentation

- Refer to the original library doc : https://github.com/aymeric-giraudet/svelte-algolia-instantsearch

## Thanks

Huge thanks to Aymeric for creating this wrapper. A PR has been made on the original library, this fork will be deleted once the PR is merged.
