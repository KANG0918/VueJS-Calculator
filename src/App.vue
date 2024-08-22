<script setup>
import { ref } from "vue";


let showVal = ref('0')
let calcArr = ['0']
let isComplete = false

const pressKeyBtn = (e) => {
  const ele = e.target
  if (ele.classList.contains('display')) {
    return
  }

  const val = String(ele.innerHTML)
  
  if (isFunKey(val)) {
    addFunKey(val)
  }
  else {
    addNumber(val)
  }
  // console.log(showVal.value, calcArr)
}

const addFunKey = key => {
  let cnt = calcArr.length

  switch(key) {
    case 'AC':
      showVal.value = '0'
      calcArr = ['0']
      break
      
    case '.':
      showVal.value = (showVal.value.includes(key)) ? showVal.value : showVal.value + key
      break
    
    case '=':
      if (cnt == 2) {
        calcArr[2] = calcArr[0]
      }

      if (cnt > 2) {
        calcArr = calculator(calcArr)
        isComplete = true
      }
      break

    case '+/-':
      cnt--
      if (!isFunKey(calcArr[cnt])) {
        calcArr[cnt] = (-Number(calcArr[cnt])).toString()
        showVal.value = calcArr[cnt]
      }
      
      break

    case '%':
      cnt--
      if (!isFunKey(calcArr[cnt])) {
        calcArr[cnt] = (Number(calcArr[cnt])/100).toString()
        showVal.value = calcArr[cnt]
      }
      break

    default:
      if (cnt == 1) {
        calcArr.push(key)
      }
      else if (cnt == 2) {
        isComplete = false
        calcArr[1] = key
      }
      else {
        calcArr = calculator(calcArr)
        calcArr.push(key)
      }      
      break
  }
}

const addNumber = num => {      
  if (calcArr.length == 2) {
    showVal.value = '0'
  }

  showVal.value = (showVal.value === '0' || isComplete) ? num : showVal.value + num
  isComplete = false
  if (calcArr.length == 1) {
    calcArr[0] = showVal.value
  }
  else {
    calcArr[2] = showVal.value
  }
}


const isFunKey = (key) => {
  const funKey = ['+/-', '+', '–', '=', '×', '÷', '%', 'AC' ,'.' ,'=' ,'+/-' ,'%']
  return funKey.includes(key)
}

const calculator = calcArr => {
  const SYMBOL = {'×':'*', '÷':'/', '–':'-'}
  let sum = 0, funKey = calcArr[1];
  
  funKey = (SYMBOL[funKey]) ? SYMBOL[funKey] : funKey;
  
  sum = eval(`${calcArr[0]} ${funKey} ${calcArr[2]}`).toString()
  calcArr = [sum]
  showVal.value = sum

  return calcArr
}
</script>

<template>
  <main>
    <div class="calc" @click="pressKeyBtn">
      <div class="display display1">{{ showVal }}</div>

      <div class="row">
        <button class="special-key">AC</button>
        <button class="special-key">+/-</button>
        <button class="special-key">&#37;</button>
        <button class="operator-key">&#247;</button>
      </div>

      <div class="row">
        <button class="number-key">7</button>
        <button class="number-key">8</button>
        <button class="number-key">9</button>
        <button class="operator-key">&#215;</button>
      </div>

      <div class="row">
        <button class="number-key">4</button>
        <button class="number-key">5</button>
        <button class="number-key">6</button>
        <button class="operator-key">–</button>
      </div>

      <div class="row">
        <button class="number-key">1</button>
        <button class="number-key">2</button>
        <button class="number-key">3</button>
        <button class="operator-key">+</button>
      </div>

      <div class="row">
        <button class="number-key double-size">0</button>
        <button class="number-key">.</button>
        <button class="operator-key">=</button>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  width: 232px;
}

.row {
  display: flex;
}

button {
  width: 58px;
  height: 48px;
  border: 1px solid rgba(90, 90, 90, 0.4);
  user-select: none;
}

.display {
  background-color: #5a5a5a;
  color: #ebebeb;
  text-align: right;
  line-height: 66px;
  padding: 0 10px;
  font-size: 36px;
  font-weight: 300;
  user-select: none;
  overflow: hidden;
}

.special-key {
  background-color: #6c6c6c;
  color: #e5e5e5;
  font-size: 18px;
}

.special-key:active {
  background-color: #868686;
}

.operator-key {
  background-color: #ff9f1d;
  color: #e9e9e9;
  font-size: 24px;
}

.operator-key:active {
  background-color: #ca7e14;
}

.number-key {
  background-color: #868686;
  color: #e9e9e9;
  font-size: 22px;
}

.number-key:active {
  background-color: #b9b9b9;
}

.double-size {
  width: 116px;
}

</style>
