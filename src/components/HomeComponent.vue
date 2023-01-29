<template>
  <div class="m-auto max-w-[1000px] p-2">
    <div class="w-full flex flex-col border-2 border-green-300 p-2 rounded-md">
      <div class="w-full flex justify-between rounded-md gap-2">
        <h1
          class="flex items-center justify-center w-1/2 py-3 text-white font-semibold text-lg bg-green-400"
        >
          ID
        </h1>
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
        v-if="loading"
        class="flex items-center justify-center py-2 font-medium"
      >
        Loading...
      </div>
      <div
        class="w-full py-2 flex flex-col gap-2"
        v-for="user in data"
        :key="user.id"
      >
        <div class="w-full flex justify-between gap-2">
          <h1 class="flex items-center justify-center w-1/2 py-3 bg-gray-100">
            {{ user.id }}
          </h1>
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
      <li
        class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer hover:bg-green-400 hover:text-white"
        @click="onClickFirstPage"
      >
        First
      </li>
      <li
        class="p-2 px-4 rounded-md cursor-pointer"
        v-for="page in displayArray"
        :key="page"
        :class="{
          'bg-green-300': currentPage == page,
          'bg-gray-200': currentPage != page
        }"
        @click="onClickPage((page - 1) * 10)"
      >
        {{ page }}
      </li>
      <li
        class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer hover:bg-green-400 hover:text-white"
        @click="onClickPreviousPage"
      >
        Previous
      </li>
      <li
        class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer hover:bg-green-400 hover:text-white"
        @click="onClickNextPage"
      >
        Next
      </li>
      <li
        class="bg-gray-200 p-2 px-4 rounded-md cursor-pointer hover:bg-green-400 hover:text-white"
        @click="onClickLastPage"
      >
        Last
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HomeComponent",
  methods: {
    onClickFirstPage() {
      this.currentSkip = 0;
    },
    onClickPreviousPage() {
      this.currentSkip -= 10;
    },
    onClickPage(page) {
      this.currentSkip = page;
    },
    onClickNextPage() {
      this.currentSkip += 10;
    },
    onClickLastPage() {
      this.currentSkip = this.lastPage;
    },
    async updateData() {
      if ((this.currentSkip < 0) | (this.currentSkip >= this.lastPage)) return;
      this.currentPage = this.currentSkip + 1;
      try {
        this.loading = true;
        const request = await axios.get(
          `https://dummyjson.com/users?limit=10&skip=${this.currentSkip}&select=firstName,age`
        );
        const response = await request.data;
        this.data = response.users;
      } catch (error) {
        console.log(error);
      } finally {
        this.loading = false;
      }
    },
    changeToNumber(currentIndex) {
      return Number(String(currentIndex).split("")[0]);
    }
  },
  mounted() {
    this.updateData();
  },
  watch: {
    currentSkip() {
      this.updateData();
      this.currentPage = this.changeToNumber(this.currentSkip) + 1;
      if (this.currentPage === this.lastPage) {
        this.displayArray = [7, 8, 9];
      }
      if (this.currentPage > 1 && this.currentSkip < this.lastPage) {
        this.displayArray = [
          this.currentPage - 1,
          this.currentPage,
          this.currentPage + 1
        ];
        return;
      }
    }
  },
  data() {
    return {
      data: [],
      currentPage: 1,
      currentSkip: 0,
      loading: false,
      lastPage: 90,
      displayArray: [1, 2, 3]
    };
  }
};
</script>
