<script lang="ts">
  import type { FormSchema } from "./lib/_schemaFormTypes.svelte";
  import SchemaForm from "./lib/SchemaForm.svelte";

  // step 1: define a type interface for the form inputs
  type Person = {
    name: string;
    dob: string;
    contactInfo: {
      email: string;
      phone: string;
      address: string;
      city: string;
      postcode: number;
      country: string;
    };
    hobbies: {
      hobbyName: string;
      interestLevel: number;
    }[];
  };

  // step 2: create a FormSchema which is essentially your form's config
  const schema: FormSchema<Person> = {
    name: { _title: "Name" },
    dob: { _title: "Date of Birth" },
    contactInfo: {
      email: { _title: "Email" },
      address: { _title: "Address" },
      city: { _title: "City" },
      country: { _title: "Country" },
      phone: { _title: "Phone Number" },
      postcode: { _title: "Postcode" },
    },
    hobbies: {
      _title: "Hobbies",
      _titleSingular: "Hobby",
      _defaultItem: {
        hobbyName: "",
        interestLevel: 0,
      },
      _arrayOf: {
        hobbyName: { _title: "Name" },
        interestLevel: { _title: "Interest Level" },
      },
    },
  };

  const defaultPerson: Person = {
    name: "",
    dob: "",
    contactInfo: {
      email: "",
      phone: "",
      address: "",
      city: "",
      postcode: 0,
      country: "",
    },
    hobbies: [],
  };
</script>

<!-- step 3: toss your schema into a SchemaForm component, 
 provide initial form values, and you're good to go! -->
<div class="page">
  <div class="form-container">
    <SchemaForm
      {schema}
      defaultValues={defaultPerson}
      onSubmit={async ({ value }) => {
        console.log("on submit", value);
      }}
      {submitter}
    />
  </div>
</div>

{#snippet submitter()}
  <button type="submit">Submit</button>
{/snippet}

<style>
  .page {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .form-container {
    width: 100%;
    max-width: 300px;
  }
</style>
