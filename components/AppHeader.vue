<template>
  <header class="flex justify-between items-center mb-8">
    <h1 class="text-2xl font-bold">devfinder</h1>
    <nav>
      <NuxtLink
        to="/history"
        class="text-1xl font-bold hover:text-blue-100 hover:underline cursor-pointer"
        >History</NuxtLink
      >
    </nav>
    <button
      @click="toggleTheme"
      class="flex items-center gap-2 uppercase font-bold text-accent cursor-pointer"
    >
      <template v-if="isDark">
        LIGHT
        <Sun />
      </template>
      <template v-else>
        DARK
        <Moon />
      </template>
    </button>
  </header>
</template>

<script>
import { Moon } from "lucide-vue-next";
import { Sun } from "lucide-vue-next";

export default {
  name: "AppHeader",
  components: {
    Moon,
    Sun,
  },
  data() {
    return {
      isDark: false,
    };
  },
  mounted() {
    const savedTheme = localStorage.getItem("theme");
    if (savedTheme === "dark") {
      this.isDark = true;
      document.documentElement.classList.add("dark-theme");
    } else {
      this.isDark = false;
      document.documentElement.classList.remove("dark-theme");
    }
  },
  methods: {
    toggleTheme() {
      // change state
      this.isDark = !this.isDark;

      if (this.isDark) {
        // Apply dark theme
        document.documentElement.classList.add("dark-theme");
        localStorage.setItem("theme", "dark");
      } else {
        // Apply dark theme
        document.documentElement.classList.remove("dark-theme");
        localStorage.setItem("theme", "light");
      }
    },
  },
};
</script>
