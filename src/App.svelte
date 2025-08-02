<script lang="ts">
  import type { CustomSchema } from "./lib/_schemaFormTypes.svelte";
  import CustomForm from "./lib/SchemaForm.svelte";

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

  const schema: CustomSchema<Person> = {
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
</script>

<div class="page">
  <div class="form-container">
    <CustomForm
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
    /* flex flex-col items-center */
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .form-container {
    width: 100%;
    max-width: 300px;
  }
</style>
