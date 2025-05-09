<template>
  <main
    class="min-h-screen py-8 px-4 md:px-6 flex flex-col items-center duration-300"
  >
    <section class="w-full max-w-3xl">
      <AppHeader @toggle-theme="toggleTheme" :isDark="isDarkMode" />
      <SearchBar @search="updateUser" />
      <ProfileCard :user="user" :isVisible="isCardVisible" />
    </section>
  </main>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";
import SearchBar from "@/components/SearchBar.vue";
import ProfileCard from "@/components/ProfileCard.vue";

export default {
  name: "index",
  components: {
    AppHeader,
    SearchBar,
    ProfileCard,
  },
  data() {
    return {
      isDarkMode: false,
      isCardVisible: false,
      user: {
        name: "The Octocat",
        username: "octocat",
        joinDate: "25 Jan 2011",
        bio: "Lorem ipsum dolor sit amet, consectetuer adipiscing elit.",
        avatar:
          "https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png",
        repos: 8,
        followers: 3938,
        following: 9,
        location: "San Francisco",
        twitter: "",
        blog: "https://github.blog",
        company: "@github",
      },
    };
  },
  methods: {
    toggleTheme() {
      this.isDarkMode = !this.isDarkMode;
      document.documentElement.classList.toggle("dark-theme", this.isDarkMode);
      localStorage.setItem("darkMode", this.isDarkMode ? "true" : "false");
    },
    updateUser(userData) {
      this.user = userData;
      this.isCardVisible = true;
    },
  },
};
</script>
