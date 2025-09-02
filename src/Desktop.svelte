<script lang="ts">
  import { draggable } from "@neodrag/svelte";
  import { activeThing, brightness, openedApps } from "$store";


  const toggleOpenApp = (app: string) => {
    if ($openedApps.includes(app)) {
      $activeThing = "";
      $openedApps = $openedApps.filter((oa) => oa !== app);
    } else {
      $activeThing = app;
      $openedApps = [...$openedApps, app];
    }
  };
</script>

<div class="desktop">
  <div class="dskAppGrid">

  </div>




  {#if $activeThing === "ActionCenter"}
    {#await import("$components/ActionCenter.svelte") then { default: ActionCenter }}
      <ActionCenter />
    {/await}
  {:else if $activeThing === "Calendar"}
    {#await import("$components/Calendar.svelte") then { default: Calendar }}
      <Calendar />
    {/await}
  {:else if $activeThing === "Search"}
    {#await import("$components/Search.svelte") then { default: Search }}
      <Search />
    {/await}
  {:else if $activeThing === "Start"}
    {#await import("$components/Start.svelte") then { default: Start }}
      <Start />
    {/await}
  {:else if $activeThing === "Widgets"}
    {#await import("$components/Widgets.svelte") then { default: Widgets }}
      <Widgets />
    {/await}
  {/if}
</div>

<div
  class="brightoverlay"
  style:background="rgb(0 0 0 / {100 - $brightness}%)"
/>

<style>
  .desktop {
    width: 100vw;
    height: calc(100vh - 48px); /* 48px is taskbars height */
    position: relative;
    overflow: hidden;
  }

  .dskAppGrid {
    position: absolute;
    inset: 0;
    display: grid;
    grid-template-columns: repeat(auto-fill, 74px);
    grid-template-rows: repeat(auto-fill, 70px);
    grid-auto-flow: column;
    padding-top: 6px;
    gap: 28px 1px;
  }
  .dskApp {
    background: unset;
    border: 1px solid transparent;
    height: min-content;
    min-height: 70px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: 12px;
    text-align: center;
    border-radius: 2px;
    color: white;
    text-shadow: 0 0 1px black, 0 0 2px black, 0 0 3px black, 0 0 4px black,
      0 1px 1px black, 0 1px 2px black;
    -webkit-user-drag: element;
  }
  .dskApp:focus,
  .dskApp:focus-visible {
    background: rgb(255 255 255 / 24%);
    outline: none;
  }
  .dskApp:hover {
    background: rgb(255 255 255 / 12%);
  }
  .dskApp:focus,
  .dskApp:focus-visible {
    border: 1px dotted;
  }
  .dskApp img {
    margin-bottom: 4px;
  }

  .brightoverlay {
    position: fixed;
    inset: 0;
    pointer-events: none;
    z-index: 99999;
  }
</style>
