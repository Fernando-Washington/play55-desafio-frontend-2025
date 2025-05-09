<template>
  <form
    @submit.prevent="search"
    class="input-container relative mb-6 rounded-2xl bg-cardBg shadow-md overflow-hidden"
    aria-labelledby="search-bar-title"
  >
    <fieldset
      class="flex items-center p-2 md:p-3"
      aria-describedby="search-help"
    >
      <!-- Search Icon -->
      <legend id="search-bar-title" class="sr-only">
        Search user on Github
      </legend>
      <div class="flex items-center justify-center w-12 h-12 text-accent">
        <Search />
      </div>

      <!-- Input Field -->
      <input
        v-model="username"
        type="text"
        id="search-bar"
        placeholder="Search GitHub username..."
        class="input flex-1 py-2 px-2 outline-none text-base w-2/4 md:text-lg sm:w-full"
        :aria-invalid="hasError"
        aria-describedby="error-message"
        autocomplete="off"
      />

      <!-- Search btn -->
      <button
        type="submit"
        class="btn text-white font-bold py-2 px-3 md:py-3 md:px-6 rounded-lg transition-colors duration-200 mr-2 cursor-pointer"
        aria-label="Buscar usuário no GitHub"
      >
        Search
      </button>
    </fieldset>

    <!-- Error message -->

    <!-- modal -->
    <ErrorModal
      :isVisible="showErrorModal"
      :message="errorMessage"
      @close="handleModalClose"
    />
  </form>
</template>

<script>
import { Search } from "lucide-vue-next";
import ErrorModal from "./ErrorModal.vue";

export default {
  name: "SearchBar",
  components: {
    Search,
    ErrorModal,
  },
  data() {
    return {
      username: "",
      hasError: false,
      showErrorModal: false,
      errorMessage: "",
    };
  },
  methods: {
    async search() {
      if (!this.username.trim()) {
        this.handleError("Please enter a username.");
        return;
      }

      this.hasError = false;

      try {
        const response = await fetch(
          `https://api.github.com/users/${this.username}`
        );
        if (!response.ok) {
          throw new Error("User not found");
        }
        const data = await response.json();

        const userData = this.formatUserData(data);

        this.updateSearchHistory(userData);

        this.$emit("search", userData);
      } catch (error) {
        this.handleError("Error fetching user. Please try again.");
      }
    },
    handleModalClose() {
      this.showErrorModal = false;
      this.hasError = false;
    },
    handleError(message) {
      this.hasError = true;
      this.errorMessage = message || "An unexpected error has occurred.";
      this.showErrorModal = true;

      setTimeout(() => {
        this.showErrorModal = false;
      }, 3000);
    },
    formatUserData(data) {
      return {
        name: data.name,
        username: data.login,
        joinDate: new Date(data.created_at)
          .toLocaleDateString("pt-BR", {
            timeZone: "America/Sao_Paulo",
            day: "2-digit",
            month: "2-digit",
            year: "numeric",
            hour: "2-digit",
            minute: "2-digit",
            hour12: false,
          })
          .replace(",", ""),
        bio: data.bio || "This user has no bio",
        avatar: data.avatar_url,
        repos: data.public_repos,
        followers: data.followers,
        following: data.following,
        location: data.location || "Not available",
        twitter: data.twitter_username || "Not available",
        blog: data.blog || "Not available",
        company: data.company || "Not available",
      };
    },
    updateSearchHistory(userData) {
      const storageData = {
        avatar: userData.avatar,
        name: userData.name,
        linkProfile: `https://github.com/${userData.username}`,
      };

      const history = JSON.parse(
        localStorage.getItem("searchHistory") || "[]"
      );

      const alreadyExistsIndex = history.findIndex(
        (element) => element.name === storageData.name
      );

      if (alreadyExistsIndex !== -1) {
        history.splice(alreadyExistsIndex, 1);
      }

      history.unshift(storageData);

      localStorage.setItem("searchHistory", JSON.stringify(history));
    },
  },
};
</script>

<style>
.input-container {
  background-color: var(--card-bg);
}

.input {
  border: 1px solid transparent;
}

.input::placeholder {
  color: var(--text-color);
  transition: background-color 0.3s ease;
}

.btn {
  background-color: var(--color-accent);
  transition: background-color 0.3s ease;
}

.btn:hover {
  filter: brightness(0.8);
}

.btn:active {
  transform: scale(0.95);
}
</style>
