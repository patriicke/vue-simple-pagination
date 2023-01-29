<template>
  <div class="m-auto max-w-[1000px] p-2">
    <div class="w-full flex flex-col border-2 border-green-300 p-2 rounded-md">
      <div class="w-full flex justify-between rounded-md gap-2">
        <h1
          class="flex items-center justify-center w-1/2 py-3 text-white font-semibold text-lg bg-green-400"
        >
          Name
        </h1>
        <h1
          class="flex items-center justify-center w-1/2 py-3 text-white font-semibold text-lg bg-green-400"
        >
          Power
        </h1>
      </div>
      <div class="w-full py-2 flex flex-col gap-2">
        <div class="w-full flex justify-between gap-2">
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Chuck Norris
          </h1>
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Infinity
          </h1>
        </div>
        <div class="w-full flex justify-between gap-2">
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Chuck Norris
          </h1>
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Infinity
          </h1>
        </div>
        <div class="w-full flex justify-between gap-2">
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Chuck Norris
          </h1>
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            Infinity
          </h1>
        </div>
      </div>
    </div>
    <ul class="flex gap-2 py-3 justify-end">
      <li class="bg-gray-200 p-2 px-4 rounded-md">
        <button
          type="button"
          @click="onClickFirstPage"
          aria-label="Go to first page"
        >
          First
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md">
        <button
          type="button"
          @click="onClickPreviousPage"
          aria-label="Go to previous page"
        >
          Previous
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md">
        <button
          type="button"
          @click="onClickNextPage"
          aria-label="Go to next page"
        >
          Next
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md">
        <button
          type="button"
          @click="onClickLastPage"
          aria-label="Go to last page"
        >
          Last
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HomeComponent",
  props: {},
  computed: {
    startPage() {
      if (this.currentPage === 1) {
        return 1;
      }

      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1;
      }

      return this.currentPage - 1;
    },
    endPage() {
      return Math.min(
        this.startPage + this.maxVisibleButtons - 1,
        this.totalPages
      );
    },
    pages() {
      const range = [];

      for (let i = this.startPage; i <= this.endPage; i += 1) {
        range.push({
          name: i,
          isDisabled: i === this.currentPage
        });
      }

      return range;
    },
    isInFirstPage() {
      return this.currentPage === 1;
    },
    isInLastPage() {
      return this.currentPage === this.totalPages;
    }
  },
  methods: {
    onClickFirstPage() {
      this.$emit("pagechanged", 1);
    },
    onClickPreviousPage() {
      this.$emit("pagechanged", this.currentPage - 1);
    },
    onClickPage(page) {
      this.$emit("pagechanged", page);
    },
    onClickNextPage() {
      this.$emit("pagechanged", this.currentPage + 1);
    },
    onClickLastPage() {
      this.$emit("pagechanged", this.totalPages);
    },
    isPageActive(page) {
      return this.currentPage === page;
    }
  }
};
</script>
