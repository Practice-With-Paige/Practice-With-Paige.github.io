<script setup lang="ts">
import { reactive, computed, ref } from 'vue'

function getRandomArbitrary(min: number, max: number): number {
  return Math.random() * (max - min) + min;
}

function isAddition(): boolean {
  return Math.random() < 0.9;
}

function getSolution(props: any): number {
  return props.isAddition ? props.firstNumber + props.secondNumber : 
    props.firstNumber > props.secondNumber ? props.firstNumber - props.secondNumber :
      props.secondNumber - props.firstNumber

}

function isSolutionFourDigits(solution: number): boolean {
  return solution.toString().length == 4;
}

function getNumberAtIndex(numberToSplit: number, index: number): string {
  const numAsString = numberToSplit.toString();
  if (index >= numAsString.length ) {
    return ""
  } else {
    return numberToSplit.toString()[index];
  }
}

function showInputtedSolutionAlert() {
  console.log(actualSolution.value);
  const isCorrect = Number(inputtedSolution.value) == actualSolution.value;
  const message = `Your inputted value of ${inputtedSolution.value} was ${ isCorrect ? 'Correct' :'Wrong'}`;
  alert(message);
}

const firstNumber: number = Math.trunc(getRandomArbitrary(100, 999));
const secondNumber: number = Math.trunc(getRandomArbitrary(100, 999)); 

const props = reactive({
  isAddition: isAddition(),
  firstNumber: firstNumber,
  secondNumber: secondNumber,
})

const firstCharacterOfSolutionInput = ref('')
const secondCharacterOfSolutionInput = ref('')
const thirdCharacterOfSolutionInput = ref('')
const fourthCharacterOfSolutionInput = ref('')



const signToShow = computed(() => {
  return props.isAddition ? "+" : "-";
})

const topNumber = computed( () => {
  return props.firstNumber > props.secondNumber ? props.firstNumber : props.secondNumber
})

const bottomNumber = computed( () => {
  return props.firstNumber === topNumber.value ? props.secondNumber : props.firstNumber

})

const actualSolution = computed( () => {
  return props.isAddition ? topNumber.value + bottomNumber.value : topNumber.value - bottomNumber.value
})

const inputtedSolution = computed( () => {
  return firstCharacterOfSolutionInput.value + secondCharacterOfSolutionInput.value+ thirdCharacterOfSolutionInput.value+ fourthCharacterOfSolutionInput.value;
})



</script>

<template>
  <main>
    <h1>Math!</h1>
    <table>
      <tr>
        <td></td>
        <td class="contains-text">{{ getNumberAtIndex(topNumber, 0) }}</td>
        <td class="vert contains-text">{{ getNumberAtIndex(topNumber, 1) }}</td>
        <td class="contains-text">{{ getNumberAtIndex(topNumber, 2) }}</td>
      </tr>
      <tr>
        <td class="hori contains-text">{{ signToShow }}</td>
        <td class="hori contains-text">{{ getNumberAtIndex(bottomNumber, 0) }}</td>
        <td class="vert hori contains-text">{{ getNumberAtIndex(bottomNumber, 1) }}</td>
        <td class="hori contains-text">{{ getNumberAtIndex(bottomNumber, 2) }}</td>
      </tr>
      <tr>
        <td class="contains-text">
          <input style="font-size:25px;" type="text" maxlength="1" pattern="\d+" v-model="firstCharacterOfSolutionInput" placeholder="?" />
        </td>
        <td class="left-only contains-text">
          <input style="font-size:25px;" type="text" maxlength="1" pattern="\d+" v-model="secondCharacterOfSolutionInput" name="answer2" placeholder="?" />
        </td>
        <td class="vert contains-text">
          <input style="font-size:25px;" type="text" maxlength="1" pattern="\d+" v-model="thirdCharacterOfSolutionInput" name="answer3" placeholder="?" />
        </td>
        <td class="contains-text">
          <input style="font-size:25px;" type="text" maxlength="1" pattern="\d+" v-model="fourthCharacterOfSolutionInput" name="answer4" placeholder="?" />
        </td>
      </tr>
      <!-- <tr>
        <td class="contains-text">{{ isSolutionFourDigits(solution) ? getNumberAtIndex(solution, 0) : "" }}</td>
        <td class="contains-text">{{ isSolutionFourDigits(solution) ? getNumberAtIndex(solution, 1) : getNumberAtIndex(solution, 0) }}</td>
        <td class="vert contains-text">{{ isSolutionFourDigits(solution) ? getNumberAtIndex(solution, 2) : getNumberAtIndex(solution, 1)}}</td>
        <td class="contains-text">{{ isSolutionFourDigits(solution) ? getNumberAtIndex(solution, 3) : getNumberAtIndex(solution, 2) }}</td>
      </tr> -->
    </table>
    <button @click="showInputtedSolutionAlert">Submit</button>

  </main>
</template>


<style scoped>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }

  h1 {
  text-align: center;
  }
  td {
    width: 100px;
    height: 100px;
  }

  td.contains-text {
    font-size: x-large;
    font-weight:bold;
    text-align: center;
    vertical-align: middle;
    line-height: auto;    
  }
  input {
    text-align: center;
    max-width: 2em;
  }
  table {
    margin: 5px auto;
  }
  .vert {
    border-left: 2px solid whitesmoke;
    border-right: 2px solid whitesmoke;
  }
  .left-only {
    border-left: 2px solid whitesmoke;
  }
  .hori {
    border-top: 2px solid whitesmoke;
    border-bottom: 2px solid whitesmoke;
  }
}
</style>