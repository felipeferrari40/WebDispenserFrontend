<script setup lang="ts">
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import {
  faBone,
  faRightToBracket,
  faBars,
} from "@fortawesome/free-solid-svg-icons";
import { ref } from "vue";

const isMenuOpen = ref(false);

library.add(faBone, faRightToBracket, faBars);
</script>

<template>
  <header class="fixed top-0 w-full h-30 bg-bgcolor text-fontcolor shadow-md">
    <nav class="grid grid-cols-2 h-full m-10">
      <div
        class="sm:pl-20 justify-start flex font-bold text-2xl align-middle items-center"
      >
        <font-awesome-icon icon="bone" class="mr-4" />
        <h1 class>BOM GAROTO</h1>
      </div>
      <div class="flex justify-end items-center font-thin">
        <ul class="hidden md:flex space-x-5">
          <li>
            <RouterLink
              class="hover:bg-fontcolor hover:text-black rounded-full p-5 flex"
              to="/"
              href="#home"
              >Home</RouterLink
            >
          </li>
          <li>
            <a
              href="#about"
              class="hover:bg-fontcolor hover:text-black rounded-full p-5 flex"
              >Sobre</a
            >
          </li>
          <li>
            <a
              href="#services"
              class="hover:bg-fontcolor hover:text-black rounded-full p-5 flex"
              >Serviços</a
            >
          </li>
          <li>
            <a
              href="#contact"
              class="hover:bg-fontcolor hover:text-black rounded-full p-5 flex"
              >Contato</a
            >
          </li>
          <li class="flex">
            <a
              href="./login.html"
              class="hover:bg-fontcolor bg-purple-500 hover:text-black rounded-full p-5"
            >
              <font-awesome-icon :icon="['fas', 'right-to-bracket']" />
              Login
            </a>
          </li>
        </ul>

        <div class="flex md:hidden">
          <button @click="isMenuOpen = !isMenuOpen" class="p-4">
            <font-awesome-icon icon="bars" class="text-2xl" />
          </button>
        </div>

        <div
          v-if="isMenuOpen"
          class="absolute top-20 right-0 bg-bgcolor shadow-md rounded-md w-full text-fontcolor"
        >
          <ul class="flex flex-col space-y-4 p-5 text-center">
            <li>
              <RouterLink
                class="hover:bg-fontcolor hover:text-black rounded-md p-3 block"
                to="/"
                href="#home"
                >Home</RouterLink
              >
            </li>
            <li>
              <a
                href="#about"
                class="hover:bg-fontcolor hover:text-black rounded-md p-3 block"
                >Sobre</a
              >
            </li>
            <li>
              <a
                href="#services"
                class="hover:bg-fontcolor hover:text-black rounded-md p-3 block"
                >Serviços</a
              >
            </li>
            <li>
              <a
                href="#contact"
                class="hover:bg-fontcolor hover:text-black rounded-md p-3 block"
                >Contato</a
              >
            </li>
            <li>
              <a
                href="./login.html"
                class="hover:bg-fontcolor bg-purple-500 hover:text-black rounded-md p-3 block"
              >
                <font-awesome-icon :icon="['fas', 'right-to-bracket']" />
                Login
              </a>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
</template>
