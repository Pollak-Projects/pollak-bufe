<script setup>
import { ref, computed } from "vue";
import Rightside from "./components/Rightside.vue";
import { RouterView, RouterLink } from "vue-router";

const time = ref(new Date());

setInterval(() => {
  time.value = new Date();
}, 1000);

const getMinutes = computed(() =>
  time.value.getMinutes().toString().padStart(2, "0")
);
const getHours = computed(() =>
  time.value.getHours().toString().padStart(2, "0")
);

function navBack() {
  history.back();
}

function kosar() {}

</script>

<template>
  <img
    src="./assets/arrow_left.svg"
    alt=""
    class="h-8 w-8 absolute left-2 top-2"
    @click="navBack()"
  />
  <RouterLink to="/kosar">
    <img
      src="./assets/shoppingcart.svg"
      alt=""
      class="h-8 w-8 absolute right-32 top-8"
    />
  </RouterLink>
  <div
    class="h-10 w-20 absolute right-4 top-6 bg-[#F7F2FA] drop-shadow-lg text-3xl flex justify-center items-center rounded-xl font-sans"
  >
    {{ getHours }}:{{ getMinutes }}
  </div>
  <main class="w-screen h-screen max-h-screen bg-[#F5F5F5] flex">
    <RouterView />
  </main>
</template>

<style scoped></style>
