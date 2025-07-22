# Svelte Schema Form

Typescript schema-based form generation for Svelte.

## Example Usage

```ts
type Person = {
  name: string;
  age: number;
  email: string;
};

const schema: CustomSchema<Person> = {
  name: { _title: "Name" },
  age: {
    _title: "Age",
    type: "number",
  },
  email: {
    _title: "Email",
    type: "email",
  },
};

const defaultPerson: Person = {
  name: "",
  age: 0,
  email: "",
};

<CustomForm
  schema={schema}
  defaultValues={defaultPerson}
  onSubmit={async ({ value }) => {
    console.log("submit", value);
  }}
/>;
```

For more, have a look at [App.svelte](/src/App.svelte)
