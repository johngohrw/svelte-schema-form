<script module>
</script>

<script lang="ts" generics="T extends Record<any, any>">
  import { createForm } from "@tanstack/svelte-form";
  import SchemaFormNode from "./SchemaFormNode.svelte";
  import type { FormSchema, FormOptionsOf } from "./_schemaFormTypes.svelte";
  import type { Snippet } from "svelte";

  export type SchemaFormProps<T> = FormOptionsOf<T> & {
    schema: FormSchema<T>;
    submitter?: Snippet;
  };

  let { schema, submitter, ...restProps }: SchemaFormProps<T> = $props();

  const form = createForm(() => ({
    ...restProps,
  }));
</script>

<form
  class="ssf-form"
  onsubmit={(e) => {
    e.preventDefault();
    e.stopPropagation();
    form.handleSubmit();
  }}
>
  <SchemaFormNode {form} node={schema} trail={[]} />
  {#if submitter}
    {@render submitter()}
  {/if}
</form>

<style>
  :global(.ssf-form, .ssf-form *) {
    box-sizing: border-box;
  }
</style>
