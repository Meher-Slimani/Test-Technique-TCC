<template>
  <div class="container mt-5">
    <div v-if="person !== null" class="card">
      <div class="card-header">Person Details</div>
      <div class="card-body">
        <h5 class="card-title">{{ person.firstName }} {{ person.lastName }}</h5>
        <p class="card-text">
          {{ person.email }}
        </p>
        <button
          class="btn btn-primary btn-sm px-4"
          data-bs-toggle="modal"
          data-bs-target="#editModal"
        >
          Edit
        </button>
      </div>
    </div>
    <!-- Modal -->
    <div
      class="modal fade"
      id="editModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Edit Person</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <!-- Form -->
            <form @submit="editPerson">
              <div class="mb-3">
                <label for="firstName" class="form-label">First Name</label>
                <input
                  type="text"
                  ref="firstName"
                  v-model="firstName"
                  class="form-control"
                  id="firstName"
                  aria-describedby="emailHelp"
                />
              </div>
              <div class="mb-3">
                <label for="lastName" class="form-label">Last Name</label>
                <input
                  type="text"
                  ref="lastName"
                  v-model="lastName"
                  class="form-control"
                  id="lastName"
                />
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input
                  type="email"
                  ref="email"
                  v-model="email"
                  class="form-control"
                  id="lastName"
                />
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-primary"
              data-bs-dismiss="modal"
              @click="editPerson"
            >
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Person",
  data() {
    return {
      id: this.$route.params.id,
      person: null,
      firstName: "",
      lastName: "",
      email: "",
    };
  },
  async created() {
    const res = await fetch(`http://localhost:5000/persons/${this.id}`);
    const data = await res.json();
    console.log(data);
    this.person = data;
    this.firstName = data.firstName;
    this.lastName = data.lastName;
    this.email = data.email;
  },
  methods: {
    // onSave(e) {
    //   e.preventDefault();
    //   const editedPerson = {
    //     id: this.person.id,
    //     firstName:
    //       this.firstName === "" ? this.person.firstName : this.firstName,
    //     lastName: this.lastName === "" ? this.person.lastName : this.lastName,
    //     email: this.email === "" ? this.person.email : this.email,
    //   };

    //   this.firstName = "";
    //   this.lastName = "";
    //   this.email = "";

    //   this.$emit("edit-person", editedPerson);
    // },
    async editPerson(e) {
      e.preventDefault();
      const editedPerson = {
        id: this.id,
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
      };
      const res = await fetch(`http://localhost:5000/persons/${this.id}`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(editedPerson),
      });

      const data = await res.json();

      console.log(data);

      this.person = data;

      // this.persons = this.persons.map((person) =>
      //   person.id === editedPerson.id ? editedPerson : person
      // );
    },
  },
};
</script>
