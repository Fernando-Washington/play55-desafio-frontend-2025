<template>
  <div
    class="card bg-cardBg rounded-2xl shadow-md p-6 md:p-10"
    v-show="isVisible"
  >
    <!-- DESKTOP -->
    <div class="md:flex flex-row gap-8">
      <!-- Avatar -->
      <div class="flex-shrink-0">
        <img
          :src="user.avatar"
          alt="Profile"
          class="w-24 h-24 md:w-28 md:h-28 rounded-full"
        />
      </div>

      <!-- Profile Info -->
      <div class="flex-1">
        <!-- Header Info -->
        <div
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
        </div>

        <!-- Bio -->
        <p class="mb-8 md:w-full">
          {{ user.bio || "Esse usuário não tem bio" }}
        </p>

        <!-- Stats -->
        <div class="stats grid grid-cols-3 gap-2 bg-pageBg rounded-lg p-4 mb-8">
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
        </div>

        <!-- Links -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.location }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-map-pin"
            >
              <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path>
              <circle cx="12" cy="10" r="3"></circle>
            </svg>
            <span>{{ user.location || "Not Available" }}</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.twitter }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-twitter"
            >
              <path
                d="M22 4s-.7 2.1-2 3.4c1.6 10-9.4 17.3-18 11.6 2.2.1 4.4-.6 6-2C3 15.5.5 9.6 3 5c2.2 2.6 5.6 4.1 9 4-.9-4.2 4-6.6 7-3.8 1.1 0 3-1.2 3-1.2z"
              ></path>
            </svg>
            <span>{{ user.twitter || "Not Available" }}</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.blog }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-link"
            >
              <path
                d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"
              ></path>
              <path
                d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"
              ></path>
            </svg>
            <a :href="user.blog" class="hover:underline" v-if="user.blog">{{
              user.blog
            }}</a>
            <span v-else>Not Available</span>
          </div>
          <div
            class="flex items-center gap-3"
            :class="{ 'opacity-50': !user.company }"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
              class="lucide lucide-building-2"
            >
              <path d="M6 22V4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v18Z"></path>
              <path d="M6 12H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h2"></path>
              <path d="M18 9h2a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2"></path>
              <path d="M10 6h4"></path>
              <path d="M10 10h4"></path>
              <path d="M10 14h4"></path>
              <path d="M10 18h4"></path>
            </svg>
            <span>{{ user.company || "Not Available" }}</span>
          </div>
        </div>
      </div>
    </div>

    <!-- MOBILE -->
    <div class="flex flex-col md:hidden gap-6">
      <div class="flex flex-row gap-5">
        <img :src="user.avatar" alt="Profile" class="w-20 h-20 rounded-full" />
        <div class="flex flex-col justify-center">
          <h2 class="text-xl font-bold">{{ user.name }}</h2>
          <a href="#" class="text-accent text-sm">@{{ user.username }}</a>
          <p class="text-gray-500 text-sm mt-1">Joined {{ user.joinDate }}</p>
        </div>
      </div>

      <!-- Bio -->
      <p class="mb-6 md:mb-8 text-sm md:text-base">
        {{ user.bio || "This profile has no bio" }}
      </p>

      <!-- Stats -->
      <div
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
      </div>

      <!-- Links -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm md:text-base">
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.location }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="lucide lucide-map-pin"
          >
            <path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path>
            <circle cx="12" cy="10" r="3"></circle>
          </svg>
          <span>{{ user.location || "Not Available" }}</span>
        </div>
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.twitter }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="lucide lucide-twitter"
          >
            <path
              d="M22 4s-.7 2.1-2 3.4c1.6 10-9.4 17.3-18 11.6 2.2.1 4.4-.6 6-2C3 15.5.5 9.6 3 5c2.2 2.6 5.6 4.1 9 4-.9-4.2 4-6.6 7-3.8 1.1 0 3-1.2 3-1.2z"
            ></path>
          </svg>
          <span>{{ user.twitter || "Not Available" }}</span>
        </div>
        <div
          class="flex items-center gap-3"
          :class="{ 'opacity-50': !user.blog }"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="lucide lucide-link"
          >
            <path
              d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"
            ></path>
            <path
              d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"
            ></path>
          </svg>
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
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            class="lucide lucide-building-2"
          >
            <path d="M6 22V4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v18Z"></path>
            <path d="M6 12H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h2"></path>
            <path d="M18 9h2a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2"></path>
            <path d="M10 6h4"></path>
            <path d="M10 10h4"></path>
            <path d="M10 14h4"></path>
            <path d="M10 18h4"></path>
          </svg>
          <span>{{ user.company || "Not Available" }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProfileCard",
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
