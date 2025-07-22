<script lang="ts" generics="T, U extends Record<any, any>">
	import type { CustomSchema, FormApiOf } from './SchemaForm.svelte';
	import SchemaFormNode from './SchemaFormNode.svelte';

	type SchemaFormNodeProps = {
		form: FormApiOf<T>;
		node: CustomSchema<U>;
		trail: (string | number)[];
	};
	let { form, node, trail, ...restProps }: SchemaFormNodeProps = $props();
	const index = trail[trail.length - 1];
	const fullIndex = trail.join('.');
	const isArray = '_arrayOf' in node;
	const isColumn = '_title' in node;
	const arrayNode = node['_arrayOf'] ?? {};
	const arrayNodeKeys = Object.keys(arrayNode);
	const defaultArrayItem = node['_defaultItem'] as any;
</script>

{#if isArray}
	<!-- array node -->
	<form.Field name={fullIndex}>
		{#snippet children(field)}
			{@const arrayItems = field.state.value as Record<any, any>[]}
			<div class="">
				<div class="mb-1 font-semibold">{node._title}:</div>
				{#if arrayItems.length > 0}
					{#each arrayItems as item, i}
						<div class="mb-1 flex rounded-xl border border-slate-200 p-2">
							<div class="mr-2 grow">
								{#each arrayNodeKeys as key}
									<SchemaFormNode
										{form}
										node={arrayNode[key as keyof typeof arrayNode] as CustomSchema<U[keyof U]>}
										trail={[...trail, i, key]}
									/>
								{/each}
							</div>
							<div class="flex flex-col">
								<button
									class="rounded border border-slate-200 bg-slate-100 p-1 text-sm"
									onclick={() => field.removeValue(i)}>Remove</button
								>
							</div>
						</div>
					{/each}
				{:else}
					<div
						class="mb-2 flex h-[50px] items-center justify-center rounded-xl border border-dashed border-slate-300 bg-slate-200/20"
					>
						empty!
					</div>
				{/if}

				<div class="flex">
					<button
						class="w-full rounded border border-slate-200 bg-slate-100 p-1 text-sm"
						onclick={() => field.pushValue(defaultArrayItem)}
						type="button"
					>
						Add {node._title}
					</button>
				</div>
			</div>
		{/snippet}
	</form.Field>
{:else if isColumn}
	<!-- normal column -->
	<div class="mb-2">
		<div class="text-sm">{node._title}:</div>
		<form.Field name={fullIndex}>
			{#snippet children(field)}
				<input
					class="rounded border border-slate-200 bg-slate-50 p-1 text-sm"
					name={field.name}
					value={field.state.value}
					onblur={field.handleBlur}
					oninput={(e) => field.handleChange((e.target as any)?.value)}
				/>
			{/snippet}
		</form.Field>
	</div>
{:else}
	<!-- nesting -->
	{#each Object.keys(node) as key}
		<SchemaFormNode {form} node={node[key] as CustomSchema<U[keyof U]>} trail={[...trail, key]} />
	{/each}
{/if}
