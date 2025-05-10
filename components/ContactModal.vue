<template>
  <div
    v-if="true"
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-transparent backdrop-blur-[2px]"
    open
  >
    <div class="absolute inset-0" @click="close" aria-hidden="true"></div>
    <article
      class="form-container relative rounded-lg shadow-lg p-6 max-w-md w-full transform transition-all border-1 border-white"
    >
      <header class="flex items-start justify-between mb-4">
        <h2 class="text-xl font-bold">Contact Us</h2>
        <button
          @click="close"
          class="text-gray-500 hover:text-red-300"
          aria-label="Close"
        >
          <X class="cursor-pointer" />
        </button>
      </header>

      <form @submit.prevent="submitForm" class="space-y-4">
        <fieldset>
          <div class="mb-4">
            <label for="name" class="block text-sm font-medium mb-1"
              >Name</label
            >
            <input
              id="name"
              v-model="formData.name"
              type="text"
              required
              class="w-full p-2 rounded-md bg-inputBg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-accentColor"
            />
          </div>

          <div class="mb-4">
            <label for="email" class="block text-sm font-medium mb-1"
              >Email</label
            >
            <input
              id="email"
              v-model="formData.email"
              type="email"
              required
              class="w-full p-2 rounded-md bg-inputBg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-accentColor"
            />
          </div>

          <div class="mb-4">
            <label for="phone" class="block text-sm font-medium mb-1"
              >Phone</label
            >
            <input
              id="phone"
              v-model="formData.phone"
              type="tel"
              required
              class="w-full p-2 rounded-md bg-inputBg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-accentColor"
            />
          </div>

          <div class="mb-4">
            <label for="birthdate" class="block text-sm font-medium mb-1"
              >Birthdate</label
            >
            <input
              id="birthdate"
              v-model="formData.birthdate"
              type="date"
              required
              class="w-full p-2 rounded-md bg-inputBg border border-gray-300 focus:outline-none focus:ring-2 focus:ring-accentColor"
            />
          </div>
        </fieldset>

        <div v-if="errorMessage" class="text-red-500 text-sm mb-4">
          {{ errorMessage }}
        </div>

        <div v-if="successMessage" class="text-green-500 text-sm mb-4">
          {{ successMessage }}
        </div>

        <footer class="flex justify-end">
          <button
            type="submit"
            class="btn text-white px-4 py-2 rounded-md hover:opacity-90 transition-opacity flex items-center cursor-pointer"
            :disabled="isSubmitting"
          >
            <span v-if="isSubmitting" class="mr-2">
              <svg
                class="animate-spin h-4 w-4 text-white"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
              >
                <circle
                  class="opacity-25"
                  cx="12"
                  cy="12"
                  r="10"
                  stroke="currentColor"
                  stroke-width="4"
                ></circle>
                <path
                  class="opacity-75"
                  fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                ></path>
              </svg>
            </span>
            Submit
          </button>
        </footer>
      </form>
    </article>
  </div>
</template>

<script>
import { useRuntimeConfig } from "#app";
import { X } from "lucide-vue-next";
import { ref, onMounted, onBeforeUnmount } from "vue";

export default {
  name: "ContactModal",
  components: {
    X,
  },
  setup(props, { emit }) {
    const formData = ref({
      name: "",
      email: "",
      phone: "",
      birthdate: "",
    });
    const config = useRuntimeConfig();
    const isSubmitting = ref(false);
    const errorMessage = ref("");
    const successMessage = ref("");
    const recaptchaLoaded = ref(false);
    const recaptchaSiteKey = config.public.recaptchaSiteKey;

    onMounted(() => {
      // Load reCAPTCHA script
      loadRecaptcha();

      // Add keyboard event listener for Escape key
      document.addEventListener("keydown", handleEscape);
    });

    onBeforeUnmount(() => {
      document.removeEventListener("keydown", handleEscape);
    });

    function handleEscape(e) {
      if (e.key === "Escape") {
        close();
      }
    }

    function close() {
      emit("close");
      // Reset form data and messages
      formData.value = {
        name: "",
        email: "",
        phone: "",
        birthdate: "",
      };
      errorMessage.value = "";
      successMessage.value = "";
    }

    function loadRecaptcha() {
      // Check if reCAPTCHA script is already loaded
      if (window.grecaptcha) {
        recaptchaLoaded.value = true;
        return;
      }

      // Create script element
      const script = document.createElement("script");
      script.src = `https://www.google.com/recaptcha/api.js?render=${recaptchaSiteKey}`;
      script.async = true;
      script.defer = true;

      script.onload = () => {
        recaptchaLoaded.value = true;
      };

      script.onerror = () => {
        errorMessage.value = "Error loading reCAPTCHA. Please try again later.";
      };

      document.head.appendChild(script);
    }

    function executeRecaptcha() {
      return new Promise((resolve, reject) => {
        if (!window.grecaptcha) {
          reject(new Error("reCAPTCHA not loaded"));
          return;
        }

        window.grecaptcha.ready(() => {
          window.grecaptcha
            .execute(recaptchaSiteKey, { action: "form_submit" })
            .then((token) => {
              resolve(token);
            })
            .catch((error) => {
              reject(error);
            });
        });
      });
    }

    async function submitForm() {
      errorMessage.value = "";
      successMessage.value = "";

      // Validate form data
      if (
        !formData.value.name ||
        !formData.value.email ||
        !formData.value.phone ||
        !formData.value.birthdate
      ) {
        errorMessage.value = "Please fill in all fields.";
        return;
      }

      // Check if reCAPTCHA is loaded
      if (!recaptchaLoaded.value) {
        errorMessage.value = "reCAPTCHA not loaded. Please reload the page.";
        return;
      }

      isSubmitting.value = true;

      try {
        // Execute reCAPTCHA
        const token = await executeRecaptcha();

        // Send form data with reCAPTCHA token
        const response = await fetch(
          "https://form-validacao-captcha.vercel.app/validar-recaptcha",
          {
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            body: JSON.stringify({
              ...formData.value,
              token,
            }),
          }
        );

        const data = await response.json();

        if (!response.ok) {
          throw new Error(data.message || "Error submitting the form");
        }

        // Show success message
        successMessage.value = "Form submitted successfully!";

        // Reset form after 2 seconds
        setTimeout(() => {
          close();
        }, 2000);
      } catch (error) {
        errorMessage.value =
          error.message || "Error submitting the form. Please try again.";
      } finally {
        isSubmitting.value = false;
      }
    }

    return {
      formData,
      isSubmitting,
      errorMessage,
      successMessage,
      close,
      submitForm,
    };
  },
};
</script>
