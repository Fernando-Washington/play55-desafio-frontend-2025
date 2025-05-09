<template>
  <transition name="fade">
    <div
      v-if="isVisible"
      class="fixed inset-0 z-50 flex items-center justify-center p-4"
      role="dialog"
      aria-labelledby="error-modal-title"
      aria-describedby="error-modal-description"
    >
      <div class="absolute inset-0 bg-transparent" @click="close"></div>
      <div
        class="modal-container relative bg-cardBg rounded-lg shadow-lg p-5 max-w-xs w-full transform transition-all"
      >
        <div class="flex items-start mb-3">
          <div class="flex-shrink-0 mr-3">
            <CircleAlert class="text-red-500" />
          </div>
          <div class="flex-1">
            <h3 class="text-base font-medium mb-1">Erro</h3>
            <p class="text-sm opacity-80">{{ message }}</p>
          </div>
          <button
            @click="close"
            class="flex-shrink-0 ml-2 text-gray-500 hover:text-gray-700"
            aria-label="Fechar modal"
          >
            <X class="cursor-pointer" />
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import { X } from "lucide-vue-next";
import { CircleAlert } from "lucide-vue-next";
export default {
  name: "ErrorModal",
  components: {
    X,
    CircleAlert,
  },
  props: {
    isVisible: {
      type: Boolean,
      default: false,
    },
    message: {
      type: String,
      default: "Ocorreu um erro.",
    },
  },
  methods: {
    close() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.2s, transform 0.2s;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
