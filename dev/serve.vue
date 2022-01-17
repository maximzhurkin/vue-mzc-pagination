<script>
import Vue from "vue";
import VueMzcPagination from "@/vue-mzc-pagination.vue";
import './serve.css'

export default Vue.extend({
  name: "ServeDev",
  components: {
    VueMzcPagination,
  },
  data() {
    return {
      page: 1,
      count: 15,
    };
  },
});
</script>

<template>
  <div id="app">
    <vue-mzc-pagination
      :current="page"
      :count="count"
      :offset="1"
      @change="(p) => (page = p)"
    >
    </vue-mzc-pagination>

    <p>&nbsp;</p>

    <vue-mzc-pagination
      :current="page"
      :count="count"
      :offset="1"
      base-class="custom"
      @change="(p) => (page = p)"
    >
      <template v-slot:prev>prev</template>
      <template v-slot:next>next</template>
    </vue-mzc-pagination>
  </div>
</template>