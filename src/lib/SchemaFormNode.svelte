<script lang="ts" generics="T, U extends Record<any, any>">
  import type {
    ArrayNode,
    CustomSchema,
    FieldApiOf,
    FormApiOf,
  } from "./_schemaFormTypes.svelte";
  import SchemaFormNode from "./SchemaFormNode.svelte";
  import SsfTextInput from "./SsfTextInput.svelte";

  type SchemaFormNodeProps = {
    form: FormApiOf<T>;
    node: CustomSchema<U>;
    trail: (string | number)[];
  };
  let { form, node, trail, ...restProps }: SchemaFormNodeProps = $props();
  const index = trail[trail.length - 1];
  const fullIndex = trail.join(".");
  const isArray = "_arrayOf" in node;
  const isColumn = "_title" in node;
  const arrayNode = node["_arrayOf"] ?? {};
  const arrayNodeKeys = Object.keys(arrayNode);
  const defaultArrayItem = node["_defaultItem"] as any;
</script>

{#if isArray}
  {@const _node = node as ArrayNode<T>}
  <!-- array node -->
  <form.Field name={fullIndex}>
    {#snippet children(field)}
      {@const arrayItems = field.state.value as Record<any, any>[]}
      {@const singularTitle = _node._titleSingular ?? _node._title ?? "item(s)"}
      <div class="ssf-array-field">
        <div class="ssf-field-label">{_node._title}:</div>
        {#if arrayItems.length > 0}
          {#each arrayItems as item, i}
            <div class="ssf-array-field-item">
              <div class="ssf-array-field-item-fields">
                {#each arrayNodeKeys as key}
                  <SchemaFormNode
                    {form}
                    node={arrayNode[
                      key as keyof typeof arrayNode
                    ] as CustomSchema<U[keyof U]>}
                    trail={[...trail, i, key]}
                  />
                {/each}
              </div>
              <div class="ssf-array-field-item-actions">
                <button onclick={() => field.removeValue(i)}>Remove</button>
              </div>
            </div>
          {/each}
        {:else}
          <div class="ssf-array-empty">
            No {singularTitle.toLowerCase()} added
          </div>
        {/if}

        <div class="flex">
          <button
            onclick={() => field.pushValue(defaultArrayItem)}
            type="button"
          >
            Add {singularTitle.toLowerCase()}
          </button>
        </div>
      </div>
    {/snippet}
  </form.Field>
{:else if isColumn}
  <!-- normal column -->
  <div class="ssf-field">
    <div class="ssf-field-label">{node._title}:</div>
    <form.Field name={fullIndex}>
      {#snippet children(field)}
        <SsfTextInput {field} />
      {/snippet}
    </form.Field>
  </div>
{:else}
  <!-- nesting -->
  {#each Object.keys(node) as key}
    <SchemaFormNode
      {form}
      node={node[key] as CustomSchema<U[keyof U]>}
      trail={[...trail, key]}
    />
  {/each}
{/if}

<style>
  .ssf-field {
    margin-bottom: 0.5rem;
  }

  .ssf-field-label {
    font-size: calc(1rem - 2px);
  }

  .ssf-array-field {
    border: 1px solid black;
    border-radius: 8px;
    padding: 12px;
    margin-bottom: 0.5rem;
  }

  .ssf-array-field-item {
    border: 1px solid black;
    padding: 6px;
    display: flex;

    margin-bottom: 0.5rem;
  }

  .ssf-array-field-item-fields {
    margin-right: 0.5rem;
    flex-grow: 1;
  }
  .ssf-array-field-item-actions {
    display: flex;
    flex-direction: column;
  }

  .ssf-array-empty {
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    border: 1px dashed black;
    background: rgba(0, 0, 0, 0.1);
    padding: 24px;
    margin-bottom: 1rem;
    color: rgb(133, 133, 133);
    font-size: calc(1rem - 2px);
  }
</style>
