<script lang="ts">
  import type { IconString } from 'src/bannerData';
  import { getSetting, parseCssSetting } from 'src/settings';
  import settings from 'src/settings/store';
  import Icon from './Icon.svelte';
  import { getHeaderTransform } from './utils';

  export let icon: IconString | undefined;
  export let header: string | undefined;
  export let withBanner: boolean;
  export let isEmbed: boolean;
  $: ({
    headerSize,
    headerDecor,
    headerHorizontalAlignment,
    headerHorizontalTransform,
    headerVerticalAlignment,
    headerVerticalTransform
  } = $settings);

  $: decor = getSetting('headerDecor', headerDecor);
  $: horizontal = getSetting('headerHorizontalAlignment', headerHorizontalAlignment);
  $: hTransform = getSetting('headerHorizontalTransform', headerHorizontalTransform);
  $: vertical = getSetting('headerVerticalAlignment', headerVerticalAlignment);
  $: vTransform = getSetting('headerVerticalTransform', headerVerticalTransform);
  $: transform = withBanner ? getHeaderTransform(horizontal, hTransform, vertical, vTransform) : '';
  $: fontSize = parseCssSetting(getSetting('headerSize', headerSize));
</script>

<div
  class="banner-header"
  class:with-banner={withBanner}
  class:shadow={decor === 'shadow'}
  class:border={decor === 'border'}
  class:align-left={horizontal === 'left'}
  class:align-center={horizontal === 'center'}
  class:align-right={horizontal === 'right'}
  class:center-of-banner={vertical === 'center'}
  style:transform
  style:font-size={fontSize}
>
  {#if icon}
    <Icon
      {icon}
      {isEmbed}
      on:open-icon-modal
    />
  {/if}
  {#if header}
    <h1 class="banner-header-title">{header}</h1>
  {/if}
</div>

<style lang="scss">
  .banner-header {
    display: flex;
    align-items: center;
    gap: 0.2em;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    padding: 4px var(--file-margins);
    margin: 0 auto;

    :global(.is-readable-line-width) & {
      max-width: calc(var(--file-line-width) + (var(--file-margins) * 2));
    }

    &.shadow { text-shadow: var(--background-primary) 0 0 6px; }
    &.border { -webkit-text-stroke: 2px var(--background-primary); }

    &.with-banner {
      top: initial;
      bottom: 0;

      &.align-left { justify-content: start; }
      &.align-center { justify-content: center; }
      &.align-right { justify-content: end; }
      &.center-of-banner { bottom: 50%; }
    }
  }

  .banner-header-title {
    font-size: 1em;
    margin: 0;
  }
</style>
