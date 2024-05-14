<script setup>
import {ref,watch, reactive} from "vue"
import Board from "./components/Board.vue"

let history =  [{squares: [null,null,null,null, null,null,null,null,null]}];

const currentSquares = ref({
  arr : { type :  Array, default : () => [null,null,null,null, null,null,null,null,null]}
});

const stepNumber = ref(0)
const xIsNext = ref(true)
let status = ref('')
let winner = calculateWinner(currentSquares.value.arr);

const moves = ref([0]) 

if (winner) {
    status.value = "Winner: " + winner;
} else {
    status.value = "Next player: " + (xIsNext.value ? "X" : "O");
}

function jumpTo(step) {    
    stepNumber.value = step;
    xIsNext.value =  (step % 2) === 0;
    currentSquares.value.arr = history[stepNumber.value].squares;
    let winner = calculateWinner(history[stepNumber.value].squares);
    if (winner) {
        status.value = "Winner: " + winner;
    } else {
        status.value = "Next player: " + (xIsNext.value ? "X" : "O");
    }
}

function handleClick(i) {
    const historyTemp = history.slice(0, stepNumber.value + 1);
    const current = historyTemp[historyTemp.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = xIsNext.value ? "X" : "O";
    history = [...historyTemp,{
          squares: [...squares]
        }];
    currentSquares.value.arr = [...squares]
    stepNumber.value =  history.length;
    xIsNext.value = !xIsNext.value;
      
    let winner = calculateWinner(squares);
    if (winner) {
        status.value = "Winner: " + winner;
    } else {
        status.value = "Next player: " + (xIsNext.value ? "X" : "O");
    }

    moves.value = history.map((step, move) => {
      return move;
    });
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
/*
watch(currentSquares.value, (newhistory) => {  
  
})
*/
</script>

<template>
    <div className="game">
        <div className="game-board">
          <Board :squares="currentSquares.arr" :clickHandler="(i) => handleClick(i)"/>
        </div>
        <div className="game-info">
          <div>{{status}}</div>
          <ol>
            <li v-for="move in moves" :key="move">
              <button @click="() => jumpTo(move)">{{move ? 'Go to move #' + move :'Go to game start'}}</button>
            </li>
          </ol>
        </div>
      </div>
</template>

<style>
.game {
  display: flex;
  flex-direction: row;
}

.game-info {
  margin-left: 20px;
}

</style>