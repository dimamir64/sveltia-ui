<!--
  @component
  The equivalent of the HTML `<tr>` element.
  @see https://developer.mozilla.org/en-US/docs/Web/HTML/Element/tr
  @see https://w3c.github.io/aria/#row
-->
<script>
  /**
   * The `class` attribute on the wrapper element.
   * @type {string}
   */
  let className = '';
  export { className as class };
</script>

<div role="row" class="sui table-row {className}" {...$$restProps}>
  <slot />
</div>

<style lang="scss">
  .table-row {
    display: table-row;
    height: var(--sui-primary-row-height);
  }
</style>
