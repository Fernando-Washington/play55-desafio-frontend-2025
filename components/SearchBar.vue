<template>
  <div
    class="input-container relative mb-6 rounded-2xl bg-cardBg shadow-md overflow-hidden"
  >
    <div class="flex items-center p-2 md:p-3">
      <div class="flex items-center justify-center w-12 h-12 text-accent">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          stroke="currentColor"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
          class="lucide lucide-search"
        >
          <circle cx="11" cy="11" r="8"></circle>
          <path d="m21 21-4.3-4.3"></path>
        </svg>
      </div>
      <input
        v-model="username"
        type="text"
        placeholder="Search GitHub username..."
        class="input flex-1 py-2 px-2 outline-none text-base md:text-lg "
      />
      <button
        @click="search"
        class="btn hover:opacity-90 text-white font-bold py-2 px-3 md:py-3 md:px-6 rounded-lg transition-colors duration-200 mr-2 cursor-pointer"
      >
        Search
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "SearchBar",
  data() {
    return {
      username: "",
    };
  },
  methods: {
    search() {
      this.$emit("search", this.username);
    },
  },
};
</script>
