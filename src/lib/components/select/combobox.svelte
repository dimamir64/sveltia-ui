<!--
  @component
  A variant of the `<Select>` widget with an auto-complete text input field.
  @see https://w3c.github.io/aria/#combobox
  @see https://www.w3.org/WAI/ARIA/apg/patterns/combobox/
-->
<script>
  import { createEventDispatcher } from 'svelte';
  import { _ } from 'svelte-i18n';
  import { writable } from 'svelte/store';
  import Button from '../button/button.svelte';
  import Icon from '../icon/icon.svelte';
  import Listbox from '../listbox/listbox.svelte';
  import TextInput from '../text-field/text-input.svelte';
  import Popup from '../util/popup.svelte';
  import { getRandomId } from '../util/util';

  /**
   * CSS class name on the button.
   * @type {string}
   */
  let className = '';

  export { className as class };

  /**
   * Where to show the dropdown menu.
   * @type {PopupPosition}
   */
  export let position = 'bottom-left';

  /**
   * Whether the textbox is editable.
   */
  export let readOnly = false;

  export let disabled = false;

  export let label = '';

  /**
   * Selected option’s value.
   * @type {(string | number | undefined)}
   */
  export let value = undefined;

  const dispatch = createEventDispatcher();
  const id = getRandomId('combobox');
  let comboboxElement;
  /** @type {TextInput?} */
  let inputComponent;
  /** @type {Popup?} */
  let popupComponent;
  let isPopupOpen = writable(false);
</script>

<div class="sui combobox {className}">
  {#if readOnly}
    <div
      class:selected={value !== undefined}
      role="combobox"
      {id}
      tabindex={disabled ? -1 : 0}
      aria-controls="{id}-popup"
      aria-expanded={$isPopupOpen}
      aria-disabled={disabled ? true : undefined}
      aria-haspopup="listbox"
      aria-activedescendant="selected-option"
      {...$$restProps}
      bind:this={comboboxElement}
    >
      <div class="label">
        {value !== undefined ? label || value : $_('_sui.combobox.select_an_option')}
      </div>
    </div>
  {:else}
    <TextInput
      role="combobox"
      {id}
      {value}
      {disabled}
      aria-controls="{id}-popup"
      aria-expanded={$isPopupOpen}
      aria-disabled={disabled ? true : undefined}
      aria-haspopup="listbox"
      aria-activedescendant="selected-option"
      {...$$restProps}
      bind:this={inputComponent}
    />
  {/if}
  <Button
    {disabled}
    tabindex={readOnly || disabled ? -1 : 0}
    aria-controls="{id}-popup"
    aria-expanded={$isPopupOpen}
    aria-disabled={disabled ? true : undefined}
    class="ghost iconic"
    on:click={(event) => {
      event.stopPropagation();

      if (!disabled) {
        $isPopupOpen = !$isPopupOpen;
      }
    }}
  >
    <Icon
      slot="start-icon"
      name="expand_more"
      label={$isPopupOpen ? $_('_sui._.collapse') : $_('_sui._.expand')}
    />
  </Button>
</div>
<Popup
  id="{id}-popup"
  anchor={comboboxElement || inputComponent?.element}
  {position}
  bind:open={isPopupOpen}
  bind:this={popupComponent}
>
  <Listbox
    on:click={(event) => {
      if (/** @type {HTMLElement} */ (event.target).matches('[role="option"]')) {
        // eslint-disable-next-line prefer-destructuring
        const target = /** @type {HTMLButtonElement?} */ (event.target);

        // @todo support more types
        value = target.dataset.type === 'number' ? Number(target.value) : target.value;
        label = target.querySelector('.label')?.textContent || target.value;
        dispatch('change', { target: inputComponent?.element, value });
      }
    }}
  >
    <slot />
  </Listbox>
</Popup>

<style lang="scss">
  .combobox {
    display: flex;
    align-items: center;
    position: relative;
    min-width: var(--combobox-min-width, 160px);

    :global(.icon) {
      font-size: var(--font-size--xx-large);
      opacity: 0.5;
    }

    & > :global(.icon) {
      position: absolute;
      inset: 8px auto auto 8px;
      z-index: 1;
    }

    & > :global(button) {
      position: absolute;
      inset: 0 0 auto auto;
      z-index: 1;
      border-top-left-radius: 0;
      border-bottom-left-radius: 0;
    }

    & > :global(button[tabindex='-1']) {
      pointer-events: none;
    }

    div[role='combobox'] {
      display: flex;
      align-items: center;
      border-width: 1px;
      border-color: var(--control-border-color);
      border-radius: 4px;
      padding: 0 32px 0 8px;
      width: 100%;
      height: var(--input--medium--height);
      background-color: var(--disabled-background-color);
      -webkit-user-select: none;
      user-select: none;
      cursor: pointer;
      transition: all 200ms;

      &:not(.selected) {
        font-style: italic;
      }

      &:hover,
      &:focus {
        background-color: var(--highlight-background-color);
      }

      &[aria-disabled='true'] {
        background-color: var(--disabled-background-color);
        opacity: 0.4;
        cursor: default;
      }

      .label {
        display: block;
        overflow: hidden;
        width: 100%;
        white-space: nowrap;
        text-overflow: ellipsis;
      }
    }

    :global(input) {
      padding: 0 32px 0 8px;
      width: 100%;
    }

    & + :global([role='listbox']) {
      // display: contents;
      position: fixed;
      z-index: 100;
      border-radius: 4px;
      box-shadow: 0 8px 16px var(--popup-shadow-color);
      overflow: auto;
      background-color: var(--secondary-background-color);
      backdrop-filter: blur(16px);
      // Add .1s delay before the position can be determined
      transition: opacity 100ms 100ms;
    }

    & + :global([role='listbox']:not(.open)) {
      opacity: 0;
      pointer-events: none;
    }
  }
</style>