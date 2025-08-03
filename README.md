# Svelte Schema Form

Generate complex svelte5 forms with a strictly-typed schema.

## Dependencies

- svelte >= 5.0.0
- @tanstack/svelte-form >= 1.14.2

## Example Usage

```ts
type Person = {
  name: string;
  age: number;
  email: string;
};

const schema: FormSchema<Person> = {
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

<SchemaForm
  schema={schema}
  onSubmit={async ({ value }) => {
    console.log("submit", value);
  }}
/>;
```

For a more detailed example, have a look at [App.svelte](/src/App.svelte) and its corresponding [demo output](https://warm-bunny-5f2bd8.netlify.app/).

## Before you use

While usable (you'd have to get your hands dirty), this is still very much a work in progress and many features are still missing. The goal is to be able to customize every aspect and behavior of the form by configuring the schema itself.

## To do
- Default input renderers for each field based on a preset type (e.g. 'number', 'email', 'phone', 'datetime', etc.) 
- Custom input renderers (render whatever you want for a field input)
- Props for styling fields and labels
- Validation
- Event handlers (onChange, onClick, onBlur, etc.)


