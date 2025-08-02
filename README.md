# Svelte Schema Form

Generate complex forms on svelte5 with just a schema with strict typing.

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

<CustomForm
  schema={schema}
  onSubmit={async ({ value }) => {
    console.log("submit", value);
  }}
/>;
```

For a more detailed example, have a look at [App.svelte](/src/App.svelte) and its corresponding [demo output](https://warm-bunny-5f2bd8.netlify.app/).

