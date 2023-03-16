<template>
  <div class="bg-slate-500 flex flex-col h-screen w-screen items-center justify-center gap-8">
    <h1 class="font-bold py-8 text-slate-800 text-4xl">Le juste Prix</h1>
    <div class="flex flex-col gap-8 max-w-lg w-full">
      <div class="bg-white flex flex-col gap-4 p-8 rounded-lg justify-center items-center w-full">
        <p v-if="blurIsApplied" class="font-normal text-3xl items-center text-center text-slate-800">Temps restant : {{
          time }}
        </p>
        <p v-if="blurIsApplied" class="blur font-normal text-3xl items-center text-center">tututu</p>
        <p v-else class="font-normal text-3xl items-center text-center">{{ price }} </p>
        <div class="w-full flex flex-row justify-evenly">
          <div :class="{ 'bg-green-500': isMore, 'bg-slate-500': !isMore }" class="text-white font-bold py-2 px-4 rounded">
            Plus
          </div>
          <div :class="{ 'bg-green-500': isLess, 'bg-slate-500': !isLess }" class="text-white font-bold py-2 px-4 rounded">
            Moins
          </div>
          <div :class="{ 'bg-green-500': isWon, 'bg-slate-500': !isWon }" class="text-white font-bold py-2 px-4 rounded">
            Gagné
          </div>
        </div>
      </div>
      <div class="flex flex-row gap-8">
        <input type="text" class="bg-white p-4 rounded-lg w-full" v-model="userInput" @keyup.enter="checkPrice">
        <button @click="checkPrice" class="bg-orange-400 hover:bg-orange-600 text-white font-bold py-2 px-4 rounded">
          Valider
        </button>
      </div>
      <div v-if="!blurIsApplied" class="flex flex-row gap-8 justify-center">
        <button @click="reset" class="bg-orange-400 hover:bg-orange-600 text-white font-bold py-2 px-4 rounded">
          Nouvelle partie
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

const price = Math.floor(Math.random() * 9999) + 1;
const priceString = (price: number) => price.toString();
const userInput = ref('');
const blurIsApplied = ref(true);
const isLess = ref(false);
const isMore = ref(false);
const isWon = ref(false);
const wait = (ms: number) => new Promise(resolve => setTimeout(resolve, ms));

const time = ref(60);

const timer = setInterval(() => {
  if (time.value > 0) {
    time.value--;
  } else {
    clearInterval(timer);
    blurIsApplied.value = false;
  }
}, 1000);

const checkPrice = () => {
  if (userInput.value === priceString(price)) {
    blurIsApplied.value = false;
    isWon.value = true;

  } else if (userInput.value > priceString(price)) {
    isLess.value = true;
    wait(1000).then(() => {
      isLess.value = false;
    })
    userInput.value = '';

  } else {
    isMore.value = true;
    wait(1000).then(() => {
      isMore.value = false;
    })
    userInput.value = '';
  }
};

const reset = () => {
  window.location.reload();
};

</script>