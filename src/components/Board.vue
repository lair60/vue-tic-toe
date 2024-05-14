<script setup>
import Square from "./Square.vue"
import {reactive,watch} from "vue"

const props = defineProps({
    squares : { type :  Array, default : () => [null,null,null,null, null,null,null,null,null]},
    clickHandler : {type:Function}
})
/*
watch(() => props.squares, (newValue, oldValue) => {    
  }
);
*/
</script>

<template>
    <div>
        <div className="board-row">                      
            <Square :value="squares[0]" :clickHandler="() => clickHandler(0)"/>
            <Square :value="squares[1]" :clickHandler="() => clickHandler(1)"/>
            <Square :value="squares[2]" :clickHandler="() => clickHandler(2)"/>          
        </div>
        <div className="board-row">
            <Square :value="squares[3]" :clickHandler="() => clickHandler(3)"/>
            <Square :value="squares[4]" :clickHandler="() => clickHandler(4)"/>
            <Square :value="squares[5]" :clickHandler="() => clickHandler(5)"/>                
        </div>
        <div className="board-row">
            <Square :value="squares[6]" :clickHandler="() => clickHandler(6)"/>
            <Square :value="squares[7]" :clickHandler="() => clickHandler(7)"/>
            <Square :value="squares[8]" :clickHandler="() => clickHandler(8)"/>                 
        </div>
      </div>
</template>

<style>
.board-row:after {
  clear: both;
  content: "";
  display: table;
}
</style>