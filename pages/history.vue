<template>
  <main
    class="history-container min-h-screen py-8 px-4 md:px-6 flex flex-col items-center duration-300"
  >
    <section class="w-full max-w-3xl">
      <header class="relative flex items-center justify-center h-12 mb-8">
        <nav>
          <a
            href="/"
            class="absolute left-0 top-4 flex items-center hover:text-accentColor pl-4"
          >
            <ArrowLeft class="mr-2" />
            <span class="hidden md:inline">Voltar</span>
          </a>
        </nav>

        <h2 class="text-xl font-bold text-center">Histórico de Pesquisas</h2>
      </header>

      <section
        v-if="searchHistory.length > 0"
        class="grid grid-cols-1 md:grid-cols-2 gap-6 w-full"
      >
        <article
          v-for="(item, index) in searchHistory"
          :key="index"
          class="card flex items-center p-4 rounded-lg shadow-md"
        >
          <img
            :src="item.avatar"
            alt="Avatar"
            class="w-12 h-12 rounded-full mr-4"
          />
          <div>
            <p class="font-semibold">{{ item.nome || "Sem nome" }}</p>
            <a
              :href="item.linkPerfil"
              target="_blank"
              rel="noopener noreferrer"
              class="text-accent hover:underline"
            >
              @{{ item.linkPerfil.split("https://github.com/")[1] }}
            </a>
          </div>
          <button class="flex ml-auto">
            <X
              @click="removeHistoryItem(index)"
              class="cursor-pointer w-5 h-5 hover:text-red-400 transition-colors duration-300 ease-in-out"
            />
          </button>
        </article>
      </section>

      <div v-else class="card text-center p-8 rounded-lg shadow-md">
        <p>Nenhuma pesquisa no histórico.</p>
      </div>
    </section>
  </main>
</template>

<script>
import { X, ArrowLeft } from "lucide-vue-next";

export default {
  name: "SearchHistory",
  components: {
    X,
    ArrowLeft,
  },
  data() {
    return {
      searchHistory: [],
    };
  },
  mounted() {
    // Carregar o histórico do localStorage
    const historico = JSON.parse(localStorage.getItem("searchHistory") || "[]");
    this.searchHistory = historico;
  },
  methods: {
    removeHistoryItem(index) {
      // Remover item do histórico
      this.searchHistory.splice(index, 1);
      // Atualizar o localStorage
      localStorage.setItem("searchHistory", JSON.stringify(this.searchHistory));
    },
  },
};
</script>

<style scoped>
.history-container {
  background-color: var(--v);
}

.card {
  background-color: var(--card-bg);
  color: var(--text-color);
}

.text-accent {
  color: var(--color-accent);
}
</style>
