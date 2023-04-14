<script setup lang="ts">
  const boardSize = 4;

  function charCodeFromA(offset: number): number {
    return 'A'.charCodeAt(0) + offset;
  }

  const alphabet: string[] = Array.from({ length: 26 }, (_, index) => String.fromCharCode(charCodeFromA(index)));


  let shuffled: string[] = alphabet
    .slice(1) //remove "A"
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value);

  let currentGame: string[] = [];
  
</script>

<template>
  <div class="game-wrapper">
    <div v-for="char in alphabet" :key="char" class="cell">{{ char }}</div>
  </div>
</template>

<style>
</style>