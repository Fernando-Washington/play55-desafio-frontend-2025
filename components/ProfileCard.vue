<template>
  <article
    class="card bg-cardBg rounded-2xl shadow-md p-6 md:p-10"
    v-show="isVisible"
    aria-labelledby="profile-card-title"
  >
    <!-- DESKTOP -->
    <div class="hidden md:flex flex-row gap-8">
      <!-- Avatar -->
      <header class="flex-shrink-0">
        <img
          :src="user.avatar"
          alt="Profile"
          class="w-24 h-24 md:w-28 md:h-28 rounded-full"
        />
      </header>

      <!-- Profile Info -->
      <div class="flex-1">
        <!-- Header Info -->
        <header
          class="flex flex-row md:flex-row md:justify-between md:items-start mb-6"
        >
          <div>
            <h2 class="text-2xl font-bold mb-1">{{ user.name }}</h2>
            <a
              :href="'https://github.com/' + user.username"
              class="text-accent"
              target="_blank"
            >
              @{{ user.username }}
            </a>
            <p class="text-gray-500 mt-2">Joined {{ user.joinDate }}</p>
          </div>
        </header>

        <!-- Bio -->
        <section aria-label="User biography">
          <p class="mb-8 md:w-full">
            {{ user.bio || "This user has no bio" }}
          </p>
        </section>

        <!-- Stats -->
        <section
          class="stats grid grid-cols-3 gap-2 bg-pageBg rounded-lg p-4 mb-8"
          aria-label="User Statistics"
        >
          <div class="text-center md:text-left">
            <p class="text-sm text-gray-500">Repos</p>
            <p class="text-xl font-bold">{{ user.repos }}</p>
          </div>
          <div class="text-center md:text-left">
            <p class="text-sm text-gray-500">Followers</p>
            <p class="text-xl font-bold">{{ user.followers }}</p>
          </div>
          <div class="text-center md:text-left">
            <p class="text-sm text-gray-500">Following</p>
            <p class="text-xl font-bold">{{ user.following }}</p>
          </div>
        </section>

        <!-- Links -->
        <section class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.location }"
          >
            <MapPin />
            <span>{{ user.location || "Not Available" }}</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.twitter }"
          >
            <Twitter />
            <span>{{ user.twitter || "Not Available" }}</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.blog }"
          >
            <ExternalLink />
            <a
              :href="user.blog"
              class="hover:underline"
              target="_blank"
              v-if="user.blog"
              >{{ user.blog }}</a
            >
            <span v-else>Not Available</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.company }"
          >
            <Building2 />
            <span>{{ user.company || "Not Available" }}</span>
          </div>
        </section>
      </div>
    </div>

    <!-- MOBILE -->
    <div class="flex flex-col md:hidden gap-6">
      <header class="flex flex-row gap-5">
        <img :src="user.avatar" alt="Profile" class="w-20 h-20 rounded-full" />
        <div class="flex flex-col justify-center">
          <h2 class="text-xl font-bold">{{ user.name }}</h2>
          <a href="#" class="text-accent text-sm">@{{ user.username }}</a>
          <p class="text-gray-500 text-sm mt-1">Joined {{ user.joinDate }}</p>
        </div>
      </header>

      <!-- Bio -->
      <section class="mb-6 md:mb-8 text-sm md:text-base">
        {{ user.bio || "This profile has no bio" }}
      </section>

      <!-- Stats -->
      <section
        class="stats grid grid-cols-3 gap-2 bg-pageBg rounded-lg p-4 mb-6 md:mb-8 text-center md:text-left"
      >
        <div class="flex flex-col items-center md:items-start">
          <p class="text-xs md:text-sm text-gray-500">Repos</p>
          <p class="text-base md:text-xl font-bold">{{ user.repos }}</p>
        </div>
        <div class="flex flex-col items-center md:items-start">
          <p class="text-xs md:text-sm text-gray-500">Followers</p>
          <p class="text-base md:text-xl font-bold">{{ user.followers }}</p>
        </div>
        <div class="flex flex-col items-center md:items-start">
          <p class="text-xs md:text-sm text-gray-500">Following</p>
          <p class="text-base md:text-xl font-bold">{{ user.following }}</p>
        </div>
      </section>

      <!-- Links -->
      <section
        class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm md:text-base"
      >
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.location }"
        >
          <MapPin />
          <span>{{ user.location || "Not Available" }}</span>
        </div>
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.twitter }"
        >
          <Twitter />
          <span>{{ user.twitter || "Not Available" }}</span>
        </div>
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.blog }"
        >
          <ExternalLink />
          <a
            :href="user.blog"
            class="hover:underline truncate"
            v-if="user.blog"
            >{{ user.blog }}</a
          >
          <span v-else>Not Available</span>
        </div>
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.company }"
        >
          <Building2 />
          <span>{{ user.company || "Not Available" }}</span>
        </div>
      </section>
    </div>
  </article>
</template>

<script>
import { MapPin } from "lucide-vue-next";
import { ExternalLink } from "lucide-vue-next";
import { Twitter } from "lucide-vue-next";
import { Building2 } from "lucide-vue-next";

export default {
  name: "ProfileCard",
  components: {
    MapPin,
    ExternalLink,
    Twitter,
    Building2,
  },
  props: {
    user: {
      type: Object,
      required: true,
    },
    isVisible: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style>
.card {
  background-color: var(--card-bg);
  transition: background-color 0.3s ease;
}

.stats {
  background-color: var(--body-bg);
  transition: background-color 0.3s ease;
}
</style>
