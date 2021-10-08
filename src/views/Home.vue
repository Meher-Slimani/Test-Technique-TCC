<template>
  <div class="container mt-3">
    <div class="mb-2 d-flex justify-content-between align-items-center">
      <span class="text-light fs-2">List of Persons</span>
      <div class="w-50 d-flex justify-content-around align-items-center">
        <div>
          <label class="text-light">Search by:</label>
          <select v-model="keyFilter">
            <option v-for="type in searchType" :key="type">{{ type }}</option>
          </select>
        </div>
        <input type="text" v-model="keySearch" placeholder="Search" />
      </div>
      <button
        class="btn btn-success rounded-circle"
        data-bs-toggle="modal"
        data-bs-target="#addModal"
      >
        +
      </button>
    </div>
    <div :key="person.id" v-for="person in filteredPersons">
      <PersonLign
        @delete-person="deletePerson"
        :person="person"
        :persons="persons"
      />
    </div>

    <!-- Modal -->
    <div
      class="modal fade"
      id="addModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Add Person</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <!-- Form -->
            <form @submit="onSubmit">
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
              @click="onSubmit"
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
import PersonLign from "../components/PersonLign.vue";

export default {
  name: "Home",
  components: {
    PersonLign,
  },
  data() {
    return {
      keySearch: "",
      keyFilter: "All",
      searchType: ["First Name", "Last Name", "Email"],
      persons: [],
      firstName: "",
      lastName: "",
      email: "",
    };
  },
  methods: {
    async fetchData() {
      const res = await fetch("http://localhost:5000/persons");
      const data = await res.json();
      return data;
    },

    async onSubmit(e) {
      e.preventDefault();
      const newPerson = {
        firstName: this.firstName,
        lastName: this.lastName,
        email: this.email,
      };
      const res = await fetch("http://localhost:5000/persons", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newPerson),
      });
      const data = await res.json();

      this.firstName = "";
      this.lastName = "";
      this.email = "";

      this.persons = [...this.persons, data];
    },
    async deletePerson(id) {
      const res = await fetch(`http://localhost:5000/persons/${id}`, {
        method: "DELETE",
      });
      await res.json();

      this.persons = this.persons.filter((person) => person.id !== id);
    },
  },
  async created() {
    this.persons = await this.fetchData();
  },
  computed: {
    filteredPersons: function () {
      switch (this.keyFilter) {
        case "First Name":
          return this.persons.filter((person) => {
            return person.firstName.toLowerCase().match(this.keySearch);
          });
        case "Last Name":
          return this.persons.filter((person) => {
            return person.lastName.toLowerCase().match(this.keySearch);
          });
        case "Email":
          return this.persons.filter((person) => {
            return person.email.toLowerCase().match(this.keySearch);
          });

        default:
          return this.persons;
      }
    },
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}
</style>
