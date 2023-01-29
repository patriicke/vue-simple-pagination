<template>
  <div class="m-auto max-w-[1000px] p-2">
    <div class="w-full flex flex-col border-2 border-green-300 p-2 rounded-md">
      <div class="w-full flex justify-between rounded-md gap-2">
        <h1
          class="flex items-center justify-center w-1/2 py-3 text-white font-semibold text-lg bg-green-400"
        >
          First Name
        </h1>
        <h1
          class="flex items-center justify-center w-1/2 py-3 text-white font-semibold text-lg bg-green-400"
        >
          Age
        </h1>
      </div>
      <div
        class="w-full py-2 flex flex-col gap-2"
        v-for="user in data"
        :key="user.id"
      >
        <div class="w-full flex justify-between gap-2">
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            {{ user.firstName }}
          </h1>
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            {{ user.age }}
          </h1>
        </div>
      </div>
    </div>
    <ul class="flex gap-2 py-3 justify-end">
      <li class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer">
        <button
          type="button"
          @click="onClickFirstPage"
          aria-label="Go to first page"
        >
          First
        </button>
      </li>
      <li
        class="p-2 px-4 rounded-md cursor-pointer"
        v-for="num in changeButtons"
        :key="num"
        :class="{
          'bg-green-300': currentPageIndex == num,
          'bg-gray-200': currentPageIndex != num
        }"
      >
        <button type="button">
          {{ num }}
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer">
        <button
          type="button"
          @click="onClickPreviousPage"
          aria-label="Go to previous page"
        >
          Previous
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer">
        <button
          type="button"
          @click="onClickNextPage"
          aria-label="Go to next page"
        >
          Next
        </button>
      </li>
      <li class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer">
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
import axios from "axios";

export default {
  name: "HomeComponent",
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
  },
  async mounted() {
    try {
      const request = await axios.get(
        "https://dummyjson.com/users?limit=10&skip=10&select=firstName,age"
      );
      const response = await request.data;
      this.data = response.users;
    } catch (error) {
      console.log(error);
    }
  },
  data() {
    return {
      data: [],
      totalPages: 10,
      currentPageIndex: 1,
      maxVisibleButtons: 3,
      changeButtons: [1, 2, 3],
      maxPages: 10
    };
  }
};
</script>
