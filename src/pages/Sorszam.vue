<script setup>
import { RouterLink, useRouter } from "vue-router";
import { ref, onMounted } from "vue";
import { JelenlegiSorszam, UtolsoNapiSorszam } from "../config/lekerdezes";
import { sorszam } from "../config/store";

const router = useRouter();
const re = ref(0);
onMounted(async () => {
  re.value = await UtolsoNapiSorszam();
  fetch(`http://localhost:5000/print`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      order_number: (re.value + 1).toString(),
    }),
  })
    .then(async (result) => {
      const res = await result.text();
      const valasz = JSON.parse(res);
      console.log(valasz);
      resolve(valasz);
    })
    .catch((error) => console.log("error", error));
  setTimeout(() => {
    location.replace("https://bufe.pollak.info/Kezdes");
  }, 5000);
});
</script>
<template>
  <div class="h-screen flex flex-col items-center">
    <h1
      class="w-screen h-1/3 flex justify-center items-end drop-shadow-2xl move"
    >
      Sikeres rendelés:
    </h1>
    <h1
      class="w-screen h-1/3 flex justify-center items-end drop-shadow-2xl move"
    >
      A sorszámod:
    </h1>
    <div class="flex justify-center items-center h-7/16">
      <div class="kulsonegy flex justify-center items-center">
        <div class="belsonegy flex justify-center items-center" id="sorszamDiv">
          {{ re + 1 }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.kulsonegy {
  background-color: rgb(90, 90, 192);
  width: 530px;
  height: 350px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 45px;
}

.belsonegy {
  width: 490px;
  height: 310px;
  border-style: solid;
  border-color: white; /* Búzaszín szegély */
  border-width: 20px;
  border-radius: 45px;
  color: white;
  font-size: 160px;
  font-family: "Abril Fatface";
  padding: 60px; /* Néhány belső térköz a szöveg körül */
  text-align: center;
}

h1 {
  padding-bottom: 10px;
  color: rgb(57, 74, 135);
  text-align: center;
  font-size: 100px;
  font-family: "Abril Fatface";
}

.move {
  animation: move 600s infinite;
}

@keyframes move {
  0%,
  50%,
  100% {
    transform: translateY(0);
  }
  25%,
  75% {
    transform: translateY(-2px);
  }
}
</style>
