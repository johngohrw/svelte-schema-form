<script module>
</script>

<script lang="ts" generics="T extends Record<any, any>">
  import { createForm } from "@tanstack/svelte-form";
  import CustomFormNode from "./SchemaFormNode.svelte";
  import type { CustomSchema, FormOptionsOf } from "./_schemaFormTypes.svelte";
  import type { Snippet } from "svelte";

  export type CustomFormProps<T> = FormOptionsOf<T> & {
    schema: CustomSchema<T>;
    submitter?: Snippet;
  };

  let { schema, submitter, ...restProps }: CustomFormProps<T> = $props();

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
  <CustomFormNode {form} node={schema} trail={[]} />
  {#if submitter}
    {@render submitter()}
  {/if}
</form>

<style>
  :global(.ssf-form, .ssf-form *) {
    box-sizing: border-box;
  }
</style>
