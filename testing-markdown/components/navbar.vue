<template>
  <nav
    class="bg-white flex border-b px-5 py-3 items-center justify-between"
  >
    <div class="flex items-center">
      <svg
        class="w-8 h-8 lg:w-12 lg:h-12 block mr-2"
        viewBox="0 0 64 64"
        xmlns="http://www.w3.org/2000/svg"
      >
        <path
          d="M13.5 11.1C15.3 3.9 19.8.3 27 .3c10.8 0 12.15 8.1 17.55 9.45 3.6.9 6.75-.45 9.45-4.05-1.8 7.2-6.3 10.8-13.5 10.8-10.8 0-12.15-8.1-17.55-9.45-3.6-.9-6.75.45-9.45 4.05zM0 27.3c1.8-7.2 6.3-10.8 13.5-10.8 10.8 0 12.15 8.1 17.55 9.45 3.6.9 6.75-.45 9.45-4.05-1.8 7.2-6.3 10.8-13.5 10.8-10.8 0-12.15-8.1-17.55-9.45-3.6-.9-6.75.45-9.45 4.05z"
          transform="translate(5 16)"
          fill="#5CB0B4"
          fill-rule="evenodd"
        ></path>
      </svg>
      <span class="text-teal-dark"
        >Tailwind Cheatsheet</span
      >
    </div>

    <div class="flex items-center">
      <form
        class="flex items-center"
        @submit="handleSearch"
      >
        <input
          type="text"
          name="query"
          @input="handleSearch"
          v-model="query"
          class="mr-3 shadow appearance-none border rounded w-full py-2 px-3 text-grey-darker leading-tight focus:outline-none focus:shadow-outline"
        />
        <button
          type="submit"
          class="bg-teal rounded shadow text-teal-lightest py-2 px-3"
        >
          Search
        </button>
      </form>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      query: ''
    };
  },
  methods: {
    handleSearch() {
      this.$emit('searchTyped', {
        query: this.query
      });
    }
  }
};
</script>
