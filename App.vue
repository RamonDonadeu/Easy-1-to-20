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
      <div class="currentValue">
        <div v-if="!gameStarted  " @click="startGame()" >
          Start
        </div>
        <div v-else-if="lostGame" @click="startGame()">
          You Lost... Shomehow...<br> Click To Restart
        </div>  
        <div v-else-if="gameFinished()" @click="startGame()">
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
  clearGame()
  generateSuitableNumber()
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
  let isSuitable = false
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
  let passedLowerBound = false
  let passedUpperBound = false
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
main{
  display: flex;
  flex-direction: column;
  align-self: center;
  justify-content: center;
  width: 100%;
}

.body{
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
