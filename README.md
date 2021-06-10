# vue-pagination

## Copy bove Index.vue as a component and use below method to use that component

```js
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
import customPagination from "./index.vue";
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
```
