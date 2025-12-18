<script setup>

import { ref, computed } from 'vue';
import { useCoinsStore } from "../store/coinsStore";

const coinStore = useCoinsStore();


const sortBy = ref(null);

const sortedCoins = computed(() => {

  const coins = [...coinStore.coinDataTop50];


  if (!sortBy.value) {
    return coins;
  }


  if (sortBy.value === 'change') {
    return coins.sort((a, b) => {
      const changeA = parseFloat(a.changePercent24Hr) || 0;
      const changeB = parseFloat(b.changePercent24Hr) || 0;
      return changeB - changeA;
    });
  }


  if (sortBy.value === 'volume') {
    return coins.sort((a, b) => {
      const volumeA = parseFloat(a.volumeUsd24Hr) || 0;
      const volumeB = parseFloat(b.volumeUsd24Hr) || 0;
      return volumeB - volumeA;
    });
  }

  return coins;
});
</script>

<template>
  <div class="bg-[#1B2028] w-full rounded-[10px] p-[20px] mt-[20px] mb-[15px] mx-auto max-w-7xl">
    <h1 class="text-white font-bold text-3xl">Live Market</h1>


    <div class="flex gap-5 mt-[25px]">
      <button
          @click="sortBy = 'change'"
          :class="sortBy === 'change' ? 'bg-blue-900' : 'bg-gray-900'"
          class="px-4 py-2 text-white rounded-lg hover:bg-blue-800 transition-colors"
      >
        Sort by Change %
      </button>
      <button
          @click="sortBy = 'volume'"
          :class="sortBy === 'volume' ? 'bg-blue-900' : 'bg-gray-900'"
          class="px-4 py-2 text-white rounded-lg hover:bg-blue-800 transition-colors"
      >
        Sort by Volume 24h
      </button>
      <button
          @click="sortBy = null"
          :class="sortBy === null ? 'bg-blue-900' : 'bg-gray-900'"
          class="px-4 py-2 text-white rounded-lg hover:bg-blue-800 transition-colors"
      >
        Sort Reset
      </button>
    </div>

    <div class="grid grid-cols-5 gap-4 mt-[20px] text-gray-400">
      <p>Coin</p>
      <p>Change</p>
      <p>Market Cap</p>
      <p>24h Volume</p>
      <p>Price</p>
    </div>

    <div class="max-h-[400px] overflow-y-scroll">
      <div v-for="coin in sortedCoins" :key="coin.id" class="grid grid-cols-5 gap-4 mt-[20px] text-gray-300">
        <p>{{ coin.name }}</p>
        <p :class="(coin.changePercent24Hr) < 0 ? 'font-bold text-red-500' : 'font-bold text-[#1ECB4F]'">
          {{ Number(coin.changePercent24Hr).toFixed(2)}}%
        </p>
        <p>${{ Number(coin.marketCapUsd).toFixed(0) }}M</p>
        <p>${{ Number(coin.volumeUsd24Hr).toFixed(0)}}M</p>
        <p>${{ Number(coin.priceUsd).toFixed(4) }}</p>
      </div>
    </div>
  </div>
</template>