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
        Buscar usuário no GitHub
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
        class="btn hover:opacity-90 text-white font-bold py-2 px-3 md:py-3 md:px-6 rounded-lg transition-colors duration-200 mr-2 cursor-pointer"
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
      @close="showErrorModal = false"
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
        this.handleError("Por favor, insira um nome de usuário.");
        return;
      }

      this.hasError = false;

      try {
        const response = await fetch(
          `https://api.github.com/users/${this.username}`
        );
        if (!response.ok) {
          throw new Error("Usuário não encontrado");
        }
        const data = await response.json();

        const userData = this.formatUserData(data);

        this.updateSearchHistory(userData);

        this.$emit("search", userData);
      } catch (error) {
        this.handleError("Erro ao buscar o usuário. Tente novamente.");
      }
    },
    handleError(message) {
      this.hasError = true;
      this.errorMessage = message;
      this.showErrorModal = true;
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
        bio: data.bio || "Esse usuário não tem bio",
        avatar: data.avatar_url,
        repos: data.public_repos,
        followers: data.followers,
        following: data.following,
        location: data.location || "Não disponível",
        twitter: data.twitter_username || "Não disponível",
        blog: data.blog || "Não disponível",
        company: data.company || "Não disponível",
      };
    },
    updateSearchHistory(userData) {
      const storageData = {
        avatar: userData.avatar,
        nome: userData.name,
        linkPerfil: `https://github.com/${userData.username}`,
      };

      const historico = JSON.parse(
        localStorage.getItem("searchHistory") || "[]"
      );

      const alreadyExistsIndex = historico.findIndex(
        (element) => element.nome === storageData.nome
      );

      if (alreadyExistsIndex !== -1) {
        historico.splice(alreadyExistsIndex, 1);
      }

      historico.unshift(storageData);

      localStorage.setItem("searchHistory", JSON.stringify(historico));
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

.btn {
  background-color: var(--color-accent);
}

.btn:hover {
  background-color: var(--color-accent-hover);
}
</style>
