<!--
  @component
  The equivalent of the HTML `<td>` element.
  @see https://developer.mozilla.org/en-US/docs/Web/HTML/Element/td
  @see https://w3c.github.io/aria/#cell
-->
<script>
  /**
   * The `class` attribute on the wrapper element.
   * @type {string}
   */
  let className = '';
  export { className as class };
</script>

<div role="cell" class="sui table-cell {className}" {...$$restProps}>
  <slot />
</div>

<style lang="scss">
  .table-cell {
    display: table-cell;
  }
</style>
