<script module>
  import type { FieldApi, FormApi, SvelteFormApi } from "@tanstack/svelte-form";

  export type FormApiOf<T> = FormApi<
    T,
    any,
    any,
    any,
    any,
    any,
    any,
    any,
    any,
    any
  > &
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
  export type FormOptionsOf<T> = FormApiOf<T>["options"];

  export type CustomSchema<T> = CustomSchemaLoose<T>;
  export type CustomSchemaStrict<T> = SchemaNode<T>;
  export type CustomSchemaLoose<T> = Partial<SchemaNode<T, true>>;
  export type CustomSchemaNode<T> = SchemaNode<T>;

  type SchemaNode<T, Nullable extends boolean = false> = {
    [key in keyof T]: T[key] extends object[]
      ? ArrayNode<T[key][number], Nullable>
      : T[key] extends any[]
        ? CustomColumn<T[key]>
        : T[key] extends object
          ? Nullable extends true
            ? Partial<SchemaNode<T[key]>>
            : SchemaNode<T[key]>
          : CustomColumn<T[key]>;
  };

  export type ArrayNode<T, Nullable extends boolean = false> = {
    _title?: string;
    _titleSingular?: string;
    _arrayOf: Nullable extends true ? Partial<SchemaNode<T>> : SchemaNode<T>;
    _defaultItem: T;
  };

  export type CustomColumn<T> = {
    _title: string;
    type?: "text" | "number" | "email" | "checkbox" | "slider" | "date";
    inputRender?: (renderProps: { field: FieldApiOf<T> }) => Element;
    defaultClass?: string;
  };
</script>

<script lang="ts"></script>
