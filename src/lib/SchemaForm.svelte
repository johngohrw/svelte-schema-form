<script module>
	export type FormApiOf<T> = FormApi<T, any, any, any, any, any, any, any, any, any> &
		SvelteFormApi<T, any, any, any, any, any, any, any, any, any>;
	export type FieldApiOf<T> = FieldApi<
		T,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any,
		any
	>;
	export type FormOptionsOf<T> = FormApiOf<T>['options'];
	export type CustomFormProps<T> = FormOptionsOf<T> & { schema: CustomSchema<T> };

	export type CustomSchema<T> = CustomSchemaLoose<T>;
	export type CustomSchemaStrict<T> = SchemaNode<T>;
	export type CustomSchemaLoose<T> = Partial<SchemaNode<T, true>>;
	export type CustomSchemaNode<T> = SchemaNode<T>;

	type SchemaNode<T, Nullable extends boolean = false> = {
		[key in keyof T]: T[key] extends object[]
			? {
					_title?: string;
					_arrayOf: Nullable extends true
						? Partial<SchemaNode<T[key][number]>>
						: SchemaNode<T[key][number]>;
					_defaultItem: T[key][number];
				}
			: T[key] extends any[]
				? CustomColumn<T[key]>
				: T[key] extends object
					? Nullable extends true
						? Partial<SchemaNode<T[key]>>
						: SchemaNode<T[key]>
					: CustomColumn<T[key]>;
	};

	export type CustomColumn<T> = {
		_title: string;
		type?: 'text' | 'number' | 'email' | 'checkbox' | 'slider' | 'date';
		inputRender?: (renderProps: { field: FieldApiOf<T> }) => Element;
	};
</script>

<script lang="ts" generics="T extends Record<any, any>">
	import { createForm, FieldApi, FormApi, type SvelteFormApi } from '@tanstack/svelte-form';
	import CustomFormNode from './SchemaFormNode.svelte';

	let { schema, ...restProps }: CustomFormProps<T> = $props();

	const form = createForm(() => ({
		...restProps
	}));
</script>

<form
	onsubmit={(e) => {
		e.preventDefault();
		e.stopPropagation();
		form.handleSubmit();
	}}
>
	<CustomFormNode {form} node={schema} trail={[]} />
	<button type="submit">Submit</button>
	<button
		onclick={(e) => {
			e.preventDefault();
			e.stopPropagation();
			console.log('PFV', form.state.values);
		}}
	>
		Print Form Values
	</button>
</form>
