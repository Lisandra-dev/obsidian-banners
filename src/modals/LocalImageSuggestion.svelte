<script lang="ts">
  import type { SearchMatches, TFile } from 'obsidian';
  import { plug } from 'src/main';
  import { getPathParts } from './utils';

  export let file: TFile;
  export let matches: SearchMatches;
  export let showPreview: boolean;

  $: parts = getPathParts(file, matches);
  $: src = plug.app.vault.getResourcePath(file);
</script>

<div class="suggestion-text" class:with-preview={showPreview}>
  {file.name}
  <small class="filepath">
    {#each parts as { part, bold }, i (i)}
      <span class:suggestion-highlight={bold}>{part}</span>
    {/each}
  </small>
</div>
{#if showPreview}
  <img {src} alt="Banner for {file.name}" class="banner-preview" />
{/if}

<style lang="scss">
  .suggestion-text.with-preview {
    padding: 0.2em 0 0.4em;
  }

  .filepath {
    color: var(--text-muted);
    font-style: italic;
  }

  .banner-preview {
    height: 200px;
    width: 100%;
    object-fit: cover;
    object-position: 50% 50%;
  }
</style>
