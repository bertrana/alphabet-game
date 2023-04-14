<script setup lang="ts">
  import { reactive } from "vue";
  const boardSize = 4;

  function charCodeFromA(offset: number): number {
    return 'A'.charCodeAt(0) + offset;
  }

  let alphabet: string[] = Array.from({ length: 26 }, (_, index) => String.fromCharCode(charCodeFromA(index)));

  // Ensure "A" is present at the beginning of the game
  // before starting the game as it's the first character in the alphabet
  let shuffled: string[] = alphabet
    .slice(1)
    .map(value => ({ value, sort: Math.random() }))
    .sort((a, b) => a.sort - b.sort)
    .map(({ value }) => value);
    
  let currentGame: string[] = reactive(shuffled.slice(0, 15));
  currentGame.splice(Math.floor(Math.random() * 15), 0, "A");
  let leftover = shuffled.slice(15, shuffled.length - 1);

  function cellClicked(event: Event) {
    let clickedChar = (event.target as HTMLDivElement).innerText;
    if (clickedChar == alphabet[0]) {
      alphabet.shift();
      let nextChar: string | undefined = "";
    
      if (currentGame.includes(alphabet[0])) {
        nextChar = leftover.shift();
        if (!nextChar) nextChar = "empty" ; // TODO: empty cell
      } else {
        nextChar = alphabet[0];
        leftover.splice(leftover.indexOf(alphabet[0]), 1);
      }
      currentGame.splice(currentGame.indexOf(clickedChar), 1, nextChar);
    }

  }
</script>

<template>
  <div class="game-wrapper">
    <div 
      v-for="char in currentGame" 
      :key="char" 
      @click="cellClicked"
      class="cell"
      >{{ char }}
    </div>
  </div>
</template>

<style>
  .game-wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;

    width: 400px;
  }

  .cell {
    display: block;

    width: 80px;
    height: 80px;

    text-align: center;

    border: 1px solid #f00;
    border-radius: 5px;
  }
</style>