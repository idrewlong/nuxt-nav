<template>
  <div class="bg-white py-4 px-2">
    <nav
      class="bg-slate-100 outline-1 outline-slate-200 outline-double max-w-7xl container sm:rounded-3xl px-10 py-4 mx-auto md:flex md:justify-between md:items-center"
    >
      <div class="flex items-center justify-between">
        <NuxtLink to="/" class="flex items-center">
          <img
            src="/public/images/IDLOGO.png"
            class="h-14 hover:scale-105 duration-200"
            alt="Logo"
          />
          <span
            class="text-2xl font-semibold whitespace-nowrap text-white"
          ></span>
        </NuxtLink>
        <!-- Mobile menu button -->
        <div class="flex md:hidden">
          <button
            type="button"
            class="text-black hover:text-gray-600 focus:outline-none focus:text-gray-600"
            aria-label="Toggle Navigation"
            @click="toggleNavbar"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"
              />
            </svg>
          </button>
        </div>
      </div>
      <!-- Mobile Menu open: "flex", Menu closed: "hidden" -->
      <div
        :class="[
          'flex-col mt-8 space-y-4 md:flex md:space-y-0 md:flex-row md:items-center md:space-x-10 md:mt-0 uppercase',
          showMenu ? 'flex' : 'hidden',
        ]"
      >
        <div
          v-for="(link, index) in navLinks"
          :key="link.to"
          class="relative group"
        >
          <div class="flex items-center justify-between">
            <NuxtLink
              :to="link.to"
              class="text-black duration-500 hover:text-electriclime ease-in-out no-underline"
              @click="closeNavbar"
            >
              {{ link.text }}
            </NuxtLink>
            <button
              v-if="link.subItems"
              @click="toggleSubMenu(index)"
              class="ml-2 text-black focus:outline-none md:hidden"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-4 w-4"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M19 9l-7 7-7-7"
                />
              </svg>
            </button>
          </div>
          <div
            v-if="link.subItems"
            :class="[
              'mt-2 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 transition-all duration-300 ease-in-out z-10',
              {
                'md:absolute md:left-0 md:w-48 md:invisible md:group-hover:visible md:opacity-0 md:group-hover:opacity-100':
                  !isMobile,
              },
              { hidden: isMobile && !activeSubMenu.includes(index) },
              { 'relative w-full': isMobile },
            ]"
          >
            <div
              class="py-1"
              role="menu"
              aria-orientation="vertical"
              aria-labelledby="options-menu"
            >
              <NuxtLink
                v-for="subItem in link.subItems"
                :key="subItem.to"
                :to="subItem.to"
                class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 hover:text-gray-900"
                role="menuitem"
                @click="closeNavbar"
              >
                {{ subItem.text }}
              </NuxtLink>
            </div>
          </div>
        </div>
        <NuxtLink
          class="text-black duration-500 hover:text-electriclime ease-in-out no-underline md:hidden"
          to="/"
          @click="closeNavbar"
        >
          Contact
        </NuxtLink>
        <!-- Contact button for medium screens and higher -->
        <div class="hidden md:block">
          <NuxtLink
            class="inline-block rounded bg-blue-400 px-6 pb-2 pt-2.5 text-xs font-medium uppercase leading-normal text-white shadow-md transition duration-150 ease-in-out hover:bg-blue-500 hover:shadow-lg focus:outline-none focus:ring-2 focus:ring-blue-300 no-underline"
            to="/contact"
          >
            Contact
          </NuxtLink>
        </div>
        <Switch />
      </div>
    </nav>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const showMenu = ref(false);
const activeSubMenu = ref([]);
const windowWidth = ref(typeof window !== "undefined" ? window.innerWidth : 0);

const isMobile = computed(() => windowWidth.value < 768);

const navLinks = [
  {
    to: "/about",
    text: "About",
    subItems: [
      { to: "/about/history", text: "Our History" },
      { to: "/about/team", text: "Our Team" },
      { to: "/about/mission", text: "Our Mission" },
    ],
  },
  {
    to: "/services",
    text: "Services",
    subItems: [
      { to: "/services/web-development", text: "Web Development" },
      { to: "/services/app-development", text: "App Development" },
      { to: "/services/consulting", text: "Consulting" },
    ],
  },
  { to: "/pricing", text: "Pricing" },
];

function toggleNavbar() {
  showMenu.value = !showMenu.value;
}

function closeNavbar() {
  showMenu.value = false;
  activeSubMenu.value = [];
}

function toggleSubMenu(index) {
  const position = activeSubMenu.value.indexOf(index);
  if (position !== -1) {
    activeSubMenu.value.splice(position, 1);
  } else {
    activeSubMenu.value.push(index);
  }
}

function handleResize() {
  windowWidth.value = window.innerWidth;
}

onMounted(() => {
  window.addEventListener("resize", handleResize);
});

onUnmounted(() => {
  window.removeEventListener("resize", handleResize);
});
</script>

<style scoped>
a {
  text-decoration: none;
  color: inherit;
}

.no-underline:hover {
  text-decoration: none;
}
</style>
