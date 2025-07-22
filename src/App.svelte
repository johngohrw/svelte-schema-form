<script lang="ts">
  import CustomForm, { type CustomSchema } from "./lib/SchemaForm.svelte";

  type Person = {
    name: string;
    age: number;
    email: string;
    nest: {
      nesta: string;
      nestb: number;
    };
    nums: number[];
    emergencyContacts: {
      name: string;
      phone: string;
      kids: {
        kidName: string;
        kidAge: number;
      }[];
    }[];
  };

  const defaultPerson: Person = {
    name: "",
    age: 0,
    email: "",
    nest: {
      nesta: "",
      nestb: 0,
    },
    nums: [],
    emergencyContacts: [],
  };

  const schema: CustomSchema<Person> = {
    name: {
      _title: "Name",
      type: "text",
    },
    age: {
      _title: "Age",
      type: "number",
    },
    email: {
      _title: "Email",
      type: "email",
    },
    nest: {
      nesta: { _title: "Nesta", type: "email" },
      nestb: { _title: "Nestb", type: "text" },
    },
    nums: {
      _title: "Some Numbers",
      type: "number",
    },
    emergencyContacts: {
      _title: "Emergency Contacts",
      _arrayOf: {
        name: { _title: "Name" },
        phone: { _title: "EC phone" },
        kids: {
          _title: "Kids",
          _arrayOf: {
            kidName: { _title: "Kid Name" },
            kidAge: { _title: "Kid Age" },
          },
          _defaultItem: {
            kidName: "",
            kidAge: 0,
          },
        },
      },
      _defaultItem: {
        kids: [],
        name: "",
        phone: "",
      },
    },
  };
</script>

<div class="flex flex-col items-center">
  <div class="w-full max-w-[500px]">
    <CustomForm
      {schema}
      defaultValues={defaultPerson}
      onSubmit={async ({ value }) => {
        console.log("submit", value);
      }}
    />
  </div>
</div>
