# vue-pagination

<template>
  <customPagination
    :current="current"
    :total="total"
    :per-page="perPage"
    @page-changed="current = $event"
    :showFilter="true"
  />
</template>

<script>
import { defineComponent } from "vue";
import customPagination from "./vad.vue";
export default defineComponent({
  name: "Pagination Example",
  components: {
    customPagination,
  },
  data() {
    return {
      current: 1,
      perPage: 2,
      total: 20,
    };
  },
});
</script>

<style scoped></style>
