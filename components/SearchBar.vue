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
        class="input flex-1 py-2 px-2 outline-none text-base md:text-lg"
        @keydown.enter="search"
      />
      <button
        type="submit"
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
    // Feature: add local storage to user and theme preferences
    async search() {
      if (!this.username) {
        alert("Por favor, insira um nome de usuário.");
        return;
      }
      try {
        const response = await fetch(
          `https://api.github.com/users/${this.username}`
        );
        if (!response.ok) {
          throw new Error("Usuário não encontrado");
        }
        const data = await response.json();
        // Emitir os dados do usuário para o componente pai
        this.$emit("search", {
          name: data.name,
          username: data.login,
          joinDate: new Date(data.created_at).toLocaleDateString("pt-BR"),
          bio: data.bio || "Esse usuário não tem bio",
          avatar: data.avatar_url,

          // stats
          repos: data.public_repos,
          followers: data.followers,
          following: data.following,

          // info
          location: data.location || "Não disponível",
          twitter: data.twitter_username || "Não disponível",
          blog: data.blog || "Não disponível",
          company: data.company || "Não disponível",
        });
      } catch (error) {
        console.error("Erro:", error);
        alert("Erro ao buscar o usuário. Tente novamente.");
      }
    },
  },
};
</script>
