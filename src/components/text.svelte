<script>
  export let text = "";
  let expanded = false;
  const maxLength = 120;
</script>

<div class="expand-text">
  {#if text.length <= maxLength}
    <span>{text}</span>
  {:else}
    <span on:click={() => expanded = !expanded} style="cursor: pointer;">
      {expanded ? text : text.slice(0, maxLength) + '...'}
    </span>
  {/if}
</div>

<style>
.expand-text span {
  word-break: break-word;
}
.expand-text button {
  margin-left: 6px;
  border: none;
  background: #eee;
  border-radius: 4px;
  cursor: pointer;
  padding: 2px 6px;
  font-size: 0.85em;
}
</style>
