<template>
  <main>
    <h1>Easy-1-to-20</h1>
    <div class="body">
      <div class="list">
        <listNumber v-for="i in 20" :key="i" :number="i" :value="numberList[i-1]" :canBeClicked="canBeClicked[i-1]" @setValue="() => {
            numberList[i-1] = currentValue
            generateSuitableNumber()
        }"/>
      </div>
      <div class="currentValue"  @click="startGame()">
        <div v-if="!gameStarted  " >
          Start
        </div>
        <div v-else-if="lostGame">
          You Lost... Shomehow...<br> Click To Restart
        </div>  
        <div v-else-if="gameFinished()">
          You Won!<br> Click To Restart
        </div>       
        <div v-else>
          {{currentValue}}
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import listNumber from './components/listNumber.vue'
import { ref } from 'vue'

const gameStarted = ref(false)
const currentValue = ref(0)
const numberList = ref([-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1])
const canBeClicked = ref([false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false])
const lostGame = ref(false)
const gameFinished = () => {
  return !numberList.value.some((value) => value === -1)
}

function startGame() {
  if(gameFinished() || lostGame.value || !gameStarted.value){
    clearGame()
    generateSuitableNumber()
  }
}
function clearGame() {
  gameStarted.value = false
  currentValue.value = 0
  numberList.value = [-1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1, -1]
  canBeClicked.value = [false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false, false]
  lostGame.value = false   
  gameStarted.value = true

}
function generateSuitableNumber() {
  let value = -1
  const numbersTried = []
  do {
    
    value = Math.floor(Math.random() * 1000) + 1
    numbersTried.push(value)
    if(numbersTried.length > 999){
      lostGame.value = true
      console.log(numbersTried.length)
      console.log('Game lost')
          return;    
    }
    setClickableNumbers(value)
  } while (!canBeClicked.value.includes(true) && !lostGame.value && !gameFinished())
  if(gameFinished()){
    console.log('You Won')
    return
  }
  currentValue.value = value
  
}

function setClickableNumbers(number: number) {
  for (let i = 0; i < canBeClicked.value.length; i++) {
    canBeClicked.value[i] = numberList.value[i]===-1 && checkBehind(i, number) && checkAhead(i, number)
  }
}

function checkBehind(index: number, number: number) {
  if (index === 0) {
    if(numberList.value[index] === -1){
      return true
    }
    return false
  }
  for(let i = index - 1; i >= 0; i--){
    if (numberList.value[i] !== -1) {
      if (numberList.value[i] < number) {
        return true
      } else {
        return false
      }
    }
  }
  return true
}

function checkAhead(index: number, number: number) {
  if (index === 20) {
    if (numberList.value[index] === -1) {
      return true
    }
    else if (numberList.value[index] < number) {
      return false
    }
  }
  for(let i = index + 1; i < 20; i++){
    if (numberList.value[i] !== -1) {
      if (numberList.value[i] > number) {
        return true
      } else {
        return false
      }
    }
  }
  return true
}

</script>

<style scoped>
body{
  margin: 0;
  padding: 0;
  background-color: black;
}
main{
  color: white;
  background-color: black;
  display: flex;
  flex-direction: column;
  align-self: center;
  justify-content: center;
  width: 100%;
}

.body{  
  background-color: black;
  margin: 2rem;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.list{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
  gap: 10px;
}
.currentValue{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 50%;
  background-color: #3d3d3d;
  height: 10rem;
  width: 20rem;
  margin: 2rem;
}
</style>
