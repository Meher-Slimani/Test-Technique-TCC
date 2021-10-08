<template>
  <div
    class="
      border border-dark
      rounded
      bg-light
      d-flex
      justify-content-between
      align-items-center
      px-2
      py-2
      mb-2
    "
  >
    <span class="fw-bold">{{ person.firstName }} {{ person.lastName }}</span>
    <div class="btn-group" role="group">
      <router-link :to="{ name: 'Person', params: { id: person?.id } }">
        <button class="btn btn-warning btn-sm">Details</button>
      </router-link>
      <button
        class="btn btn-danger btn-sm"
        @click="$emit('delete-person', person.id)"
      >
        Delete
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PersonLign",
  props: {
    person: Object,
    persons: Array,
  },
};
</script>

<style scoped></style>
