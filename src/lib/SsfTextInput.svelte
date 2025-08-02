<script lang="ts" generics="T">
  import type { FieldApiOf } from "./_schemaFormTypes.svelte";

  let { field }: { field: FieldApiOf<T> } = $props();
</script>

<input
  class="ssf-input"
  name={field.name}
  value={field.state.value}
  onblur={field.handleBlur}
  oninput={(e) => field.handleChange((e.target as any)?.value)}
/>

<style>
  .ssf-input {
    border-radius: 4px;
    border: 1px solid black;
    background: white;
    padding: 2px 6px;
    font-size: calc(1rem - 2px);
    width: 100%;
  }
</style>
