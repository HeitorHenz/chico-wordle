
<template>
  <div v-if="!show.value">
    <button class="rounded-full bg-green-300 p-5 m-5 text-xl" @click="show.value=true"><b>CHICO</b></button>
    <img src="./assets/chico.jpeg" alt="CHICO" width="500" class="float-left p-5">
  </div>
  <div class="flex flex-col h-screen max-w-md mx-auto justify-evenly" v-if="show.value">
    <div>
      <word-row v-for="(guess, i) in state.guesses" :key="i" :value="guess" :solution="state.solution" :submitted="i < state.currentGuessIndex"></word-row>
    </div>
    <p v-if="wonGame" class="text-center text-4xl">
          <button class="rounded-full bg-green-300 p-5 m-5 text-xl" @click="show.value=false"><b>CHICO</b></button>

    </p>
    <p v-if="lostGame" class="text-center">
      Sem mais tentativas
    </p>
    <simple-keyboard @onKeyPress="handleInput" :guessedLetters="state.guessedLetters"/>
  </div>
</template>
<script setup>
import { onMounted, reactive, computed } from '@vue/runtime-core';
import SimpleKeyboard from './components/SimpleKeyboard.vue';
import WordRow from './components/WordRow.vue';

const state = reactive({
  solution: 'chico',
  guesses: ['', '', '', '', '', ''],
  currentGuessIndex: 0,
  guessedLetters:{
    miss:[],
    found:[],
    hint:[]
  }
})
var show = reactive(
  {
    value: false
  }
)
const wonGame = computed(
  () => 
  state.guesses[state.currentGuessIndex -1] === state.solution

)
const lostGame = computed(
  () =>
    !wonGame.value && state.currentGuessIndex >= 6
)

const handleInput = (key) =>{
  if(state.currentGuessIndex >= 6 || wonGame.value){
    return;
  }
  const currentGuess = state.guesses[state.currentGuessIndex]

  if(key == '{enter}'){
    if(currentGuess.length == 5){
      state.currentGuessIndex++
      for(var i = 0; i < currentGuess.length; i++){
        let c = currentGuess.charAt(i)
        if (c == state.solution.charAt(i)){
          state.guessedLetters.found.push(c)
        }else if (state.solution.indexOf(c) != -1){
          state.guessedLetters.hint.push(c);
        }else{
          state.guessedLetters.miss.push(c)
        }
      }
    }
  }else if(key == '{bksp}'){
    state.guesses[state.currentGuessIndex] = currentGuess.slice(0,-1)
  }else if(currentGuess.length < 5){
    const alphaRegex = /[a-zA-Z]/;
    if(alphaRegex.test(key)){
      state.guesses[state.currentGuessIndex] += key;
    }
  }
  
}

onMounted(() => {
  window.addEventListener("keyup", (e) =>{
    e.preventDefault()
        let key =
      e.keyCode == 13
        ? "{enter}"
        : e.keyCode == 8
        ? "{bksp}"
        : String.fromCharCode(e.keyCode).toLowerCase();
    handleInput(key);
  })
})
</script>

<style>

</style>
