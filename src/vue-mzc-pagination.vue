<template>
  <div :class="baseClass">
    <span :class="`${baseClass}__item`">
      <button
        :class="[
          `${baseClass}__button ${baseClass}__button--prev`,
          current <= 1 ? `${baseClass}__button--disable` : '',
        ]"
        @click="prev()"
      >
        <slot name="prev">&larr;</slot>
      </button>
    </span>
    <span
      v-for="(page, index) in pages"
      :key="index"
      :class="`${baseClass}__item`"
    >
      <button
        v-if="page === '...'"
        tabindex="-1"
        :class="`${baseClass}__button ${baseClass}__button--spred`"
      >
        ...
      </button>
      <button
        v-else
        :class="[
          `${baseClass}__button`,
          page === current ? `${baseClass}__button--active` : '',
        ]"
        :disabled="page === current"
        @click="change(page)"
      >
        {{ page }}
      </button>
    </span>
    <span :class="`${baseClass}__item`">
      <button
        :class="[
          `${baseClass}__button ${baseClass}__button--next`,
          current >= count ? `${baseClass}__button--disable` : '',
        ]"
        @click="next()"
      >
        <slot name="next">&rarr;</slot>
      </button>
    </span>
  </div>
</template>

<script>
export default /*#__PURE__*/ {
  name: "VueMzcPagination",
  props: {
    current: {
      type: Number,
      default: 1,
      require: true,
    },
    count: {
      type: Number,
      default: 1,
      require: true,
    },
    offset: {
      type: Number,
      default: 1,
    },
    baseClass: {
      type: String,
      default: "vue-mzc-pagination",
    },
  },
  computed: {
    pages() {
      const range = this.getRange();
      const pages = this.addSpreds(range);

      return pages;
    },
  },
  methods: {
    getRange() {
      let range = [];
      let i = 1;

      while (i <= this.count) {
        if (
          i === 1 ||
          i === this.count ||
          (i >= this.current - this.offset &&
            i < this.current + this.offset + 1)
        ) {
          range.push(i);
        }
        i++;
      }
      return range;
    },
    addSpreds(range) {
      let previous = 0;
      let pages = [];

      for (let i = 0; i < range.length; i++) {
        const page = range[i];

        if (previous) {
          if (page - previous === 2) {
            pages.push(previous + 1);
          } else if (page - previous !== 1) {
            pages.push("...");
          }
        }
        pages.push(page);
        previous = page;
      }
      return pages;
    },
    prev() {
      if (this.current > 1) {
        this.change(this.current - 1);
      }
    },
    next() {
      if (this.current < this.count) {
        this.change(this.current + 1);
      }
    },
    change(page) {
      if (page !== this.current) {
        this.$emit("change", page);
      }
    },
  },
};
</script>

<style>
:root {
  --vue-mzc-pagination-size: 32px;
  --vue-mzc-pagination-width: var(--vue-mzc-pagination-size);
  --vue-mzc-pagination-height: var(--vue-mzc-pagination-size);
  --vue-mzc-pagination-font-size: 0.875em;
  --vue-mzc-pagination-font-weight: 700;
  --vue-mzc-pagination-primary-color: #539bf5;
  --vue-mzc-pagination-background-color: transparent;
  --vue-mzc-pagination-border-width: 1px;
  --vue-mzc-pagination-border-radius: 4px;
}
.vue-mzc-pagination {
  display: flex;
}
.vue-mzc-pagination__item {
	display: block;
}
.vue-mzc-pagination__button {
  box-sizing: border-box;
  cursor: pointer;
  padding: 0;
  display: block;
  width: 32px;
  height: 32px;
  width: var(--vue-mzc-pagination-width);
  height: var(--vue-mzc-pagination-height);
  font-size: 0.875em;
  font-size: var(--vue-mzc-pagination-font-size);
  font-weight: 700;
  font-weight: var(--vue-mzc-pagination-font-weight);
  color: #539bf5;
  color: var(--vue-mzc-pagination-primary-color);
  background-color: transparent;
  background-color: var(--vue-mzc-pagination-background-color);
  border-width: 1px;
  border-width: var(--vue-mzc-pagination-border-width);
  border-style: solid;
  border-color: transparent;
  border-radius: 4px;
  border-radius: var(--vue-mzc-pagination-border-radius);
}
.vue-mzc-pagination__button--spred {
  cursor: default;
}
.vue-mzc-pagination__button--active {
  border-color: #539bf5;
  border-color: var(--vue-mzc-pagination-primary-color);
}
.vue-mzc-pagination__button--disable {
	cursor: not-allowed;
  opacity: .5;
}
</style>