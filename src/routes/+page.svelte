<script lang="ts">
  import {
    InstantSearch,
    SearchBox,
    Hits,
    Pagination,
    HitsPerPage,
    PoweredBy,
    RefinementList,
    ClearRefinements,
    ToggleRefinement,
    SortBy,
    Highlight,
    HierarchicalMenu,
    Breadcrumb,
    CurrentRefinements,
    Menu,
    RangeInput,
    Stats,
  } from "$lib";
  import algoliasearch from "algoliasearch/lite";

  import Panel from "./Panel.svelte";

  const searchClient = algoliasearch("latency", "6be0576ff61c053d5f9a3225e2a90f76");
</script>

<svelte:head>
  <title>svelte-algolia-instantsearch | Demo</title>
</svelte:head>

<InstantSearch indexName="instant_search" routing {searchClient}>
  <div class="Container">
    <div>
      <Panel header="Brands"
        ><RefinementList
          attribute="brand"
          searchable
          searchablePlaceholder="Search brands"
          showMore
        /></Panel
      >
      <Panel header="Hierarchy">
        <HierarchicalMenu
          attributes={[
            "hierarchicalCategories.lvl0",
            "hierarchicalCategories.lvl1",
            "hierarchicalCategories.lvl2",
          ]}
          showMore
        />
      </Panel>
      <Panel header="Categories">
        <Menu attribute="categories" showMore />
      </Panel>
      <Panel header="Price">
        <RangeInput attribute="price" />
      </Panel>
      <Panel header="Free Shipping">
        <ToggleRefinement attribute="free_shipping" label="Free shipping" />
      </Panel>
      <Panel header="Stats">
        <Stats translations={{ rootElementText: (options) => `${options.areHitsSorted}` }} />
      </Panel>
    </div>

    <div class="Search">
      <Breadcrumb
        attributes={[
          "hierarchicalCategories.lvl0",
          "hierarchicalCategories.lvl1",
          "hierarchicalCategories.lvl2",
        ]}
      />

      <SearchBox placeholder="Search" />

      <div class="Search-header">
        <PoweredBy />
        <HitsPerPage
          items={[
            { label: "20 hits per page", value: 20, default: true },
            { label: "40 hits per page", value: 40 },
          ]}
        />
        <SortBy
          items={[
            { label: "Relevance", value: "instant_search" },
            { label: "Price (asc)", value: "instant_search_price_asc" },
            { label: "Price (desc)", value: "instant_search_price_desc" },
          ]}
        />
      </div>

      <div class="CurrentRefinements">
        <CurrentRefinements
          transformItems={(items) =>
            items.map((item) => {
              const label = item.label.startsWith("hierarchicalCategories")
                ? "Hierarchy"
                : item.label;

              return {
                ...item,
                label,
              };
            })}
        />
        <ClearRefinements translations={{ resetButtonText: "Clear filters" }} />
      </div>

      <Hits let:hit>
        <a href="/test">
          <Highlight attribute="name" classes={{ root: "Hit-label" }} {hit} />
          <span class="Hit-price">${hit.price}</span>
        </a>
      </Hits>

      <Pagination />
    </div>
  </div>
</InstantSearch>
