<template>
  <div class="home">
    
    <h1 class="custHeader"> Tic Tac Toe </h1>
    <div v-if="!gameOn"><button class="butRegular" @click="whoGoesFirst('x')">Select X</button>  <button class="butRegular" @click="whoGoesFirst('o')">Select O</button> </div>

    <b-container class="bv-example-row" v-if="gameOn">
      <b-row>
        <b-col> <p v-if="moveCount == 0"><b>You go first.</b></p>
          <div v-if="winner != 0">
          <span class="winnerText">The winner is {{winner}}</span><br>
          <button @click="newGame()" class="movesList" >New Game?</button>
        </div>
        <div v-else-if="moveCount == 9">
          <span class="winnerText">It's a draw.</span><br>
          <button @click="newGame()" class="movesList" >New Game?</button>
        </div>
      </b-col>
        <b-col>
          
          <div class="game">
            <div class="gameRow">
              <span class="square"> <button @click="makeMove(0)"  v-text="gridSelection[0].value" :disabled="(gridSelection[0].disabled)?true:false" :style="{color:(gridSelection[0].win)?'red':'black'}"></button></span>
              
              <span class="square vert"><button @click="makeMove(1)" v-text="gridSelection[1].value" :disabled="(gridSelection[1].disabled)?true:false" :style="{color:(gridSelection[1].win)?'red':'black'}"></button></span>
            
              <span class="square"><button @click="makeMove(2)" v-text="gridSelection[2].value" :disabled="(gridSelection[2].disabled)?true:false" :style="{color:(gridSelection[2].win)?'red':'black'}"></button></span>
            </div>
            
            <div class="gameRow">
              <span class="square hori"><button @click="makeMove(3)" v-text="gridSelection[3].value" :disabled="(gridSelection[3].disabled)?true:false" :style="{color:(gridSelection[3].win)?'red':'black'}"></button></span>
              
              <span class="square hori vert"><button @click="makeMove(4)" v-text="gridSelection[4].value" :disabled="(gridSelection[4].disabled)?true:false" :style="{color:(gridSelection[4].win)?'red':'black'}"></button></span>
            
              <span class="square hori"><button @click="makeMove(5)" v-text="gridSelection[5].value" :disabled="(gridSelection[5].disabled)?true:false" :style="{color:(gridSelection[5].win)?'red':'black'}"></button></span>
            </div>

            <div class="gameRow">
              <span class="square"><button @click="makeMove(6)" v-text="gridSelection[6].value" :disabled="(gridSelection[6].disabled)?true:false" :style="{color:(gridSelection[6].win)?'red':'black'}"></button></span>
              
              <span class="square vert"><button @click="makeMove(7)" v-text="gridSelection[7].value" :disabled="(gridSelection[7].disabled)?true:false" :style="{color:(gridSelection[7].win)?'red':'black'}"></button></span>
            
              <span class="square"><button @click="makeMove(8)" v-text="gridSelection[8].value" :disabled="(gridSelection[8].disabled)?true:false" :style="{color:(gridSelection[8].win)?'red':'black'}"></button></span>
            </div>

          </div>

        </b-col>
        <b-col> </b-col>
      </b-row>
    </b-container>


  </div>
</template>

<script>


export default {
  data(){
    return{
      gridValue: [0,0,0,0,0,0,0,0,0],
      gridSelection: [
      {id: 0, value: "", disabled: 0, win: 0},
      {id: 1, value: "", disabled: 0, win: 0},
      {id: 2, value: "", disabled: 0, win: 0},
      {id: 3, value: "", disabled: 0, win: 0},
      {id: 4, value: "", disabled: 0, win: 0},
      {id: 5, value: "", disabled: 0, win: 0},
      {id: 6, value: "", disabled: 0, win: 0},
      {id: 7, value: "", disabled: 0, win: 0},
      {id: 8, value: "", disabled: 0, win: 0},
      ],
      winAi: 0,
      winPlayer: 0,
      aiValue: 0,
      whosMove: null,
      compMove: null,
      gameOn: 0,
      moveCount: 0,
      winner: 0,
      first: null,
      row1: null,
      row2: null,
      row3: null,
      col1: null,
      col2: null,
      col3: null,
      dia1: null,
      dia2: null,
    }
  },
  methods:{
    whoGoesFirst(selection){
      this.whosMove = selection
      this.first = selection
      if(selection == 'x'){
        this.compMove = 'o'
        this.aiValue = 5
        this.winAi = 10
        this.winPlayer = 2
      }else{
        this.compMove = 'x'
        this.aiValue = 1
        this.winAi = 2
        this.winPlayer = 10
      }
      this.gameOn = 1
    },
    makeMove(index){
      this.moveCount++
      this.gridSelection[index].value = this.whosMove
      this.gridSelection[index].disabled = 1
      
      if(this.whosMove == 'x'){
        this.gridValue[index] = 1
        this.whosMove = 'o'
      }else{
        this.gridValue[index] = 5
        this.whosMove = 'x'
      }

      this.calculateWin()

      if (this.moveCount > 3){
        this.checkWinner()
      }

      //AI MOVES

      if(this.compMove == this.whosMove && this.moveCount < 9 && this.winner == 0){
        this.aiMove();
        this.checkWinner()
      }
    },
    calculateWin(){
      this.row1 = this.gridValue[0] + this.gridValue[1] + this.gridValue[2]
      this.row2 = this.gridValue[3] + this.gridValue[4] + this.gridValue[5]
      this.row3 = this.gridValue[6] + this.gridValue[7] + this.gridValue[8]
      this.col1 = this.gridValue[0] + this.gridValue[3] + this.gridValue[6]
      this.col2 = this.gridValue[1] + this.gridValue[4] + this.gridValue[7]
      this.col3 = this.gridValue[2] + this.gridValue[5] + this.gridValue[8]
      this.dia1 = this.gridValue[0] + this.gridValue[4] + this.gridValue[8]
      this.dia2 = this.gridValue[2] + this.gridValue[4] + this.gridValue[6]
    },
    checkWinner(){
      this.winner = 0
      if(this.row1 == 3 || this.row1 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[0].win = 1
        this.gridSelection[1].win = 1
        this.gridSelection[2].win = 1
      }else if(this.row2 == 3 || this.row2== 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[3].win = 1
        this.gridSelection[4].win = 1
        this.gridSelection[5].win = 1
      }else if(this.row3 == 3 || this.row3 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[6].win = 1
        this.gridSelection[7].win = 1
        this.gridSelection[8].win = 1
      }else if(this.col1 == 3 || this.col1 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[0].win = 1
        this.gridSelection[3].win = 1
        this.gridSelection[6].win = 1
      }else if(this.col2 == 3 || this.col2 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[1].win = 1
        this.gridSelection[4].win = 1
        this.gridSelection[7].win = 1
      }else if(this.col3 == 3 || this.col3 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[2].win = 1
        this.gridSelection[5].win = 1
        this.gridSelection[8].win = 1
      }else if(this.dia1 == 3 || this.dia1 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[0].win = 1
        this.gridSelection[4].win = 1
        this.gridSelection[8].win = 1
      }else if(this.dia2 == 3 || this.dia2 == 15 ){
        this.winner= (this.whosMove == 'x')?'o':'x'
        this.gridSelection[2].win = 1
        this.gridSelection[4].win = 1
        this.gridSelection[6].win = 1
      }
      if(this.winner != 0){
        this.gridSelection.forEach(element => {
          element.disabled = 1
        });
      }
    },//end the winner
    newGame(){
      window.location.reload()
    },
    aiMove(){
      // First Move
      if(this.moveCount < 3){
        let okNum = true
        if(this.gridValue[4] == 0){
          this.gridValue[4] = this.aiValue
          this.gridSelection[4].value = this.compMove
          this.gridSelection[4].disabled = 1
        }else{
          let rand = _.random(0, 2);
          if(rand == 0){
            this.gridValue[0] = this.aiValue
            this.gridSelection[0].value = this.compMove
            this.gridSelection[0].disabled = 1
          }else if (rand == 1){
            this.gridValue[8] = this.aiValue
            this.gridSelection[8].value = this.compMove
            this.gridSelection[8].disabled = 1
          }else{
            this.gridValue[2] = this.aiValue
            this.gridSelection[2].value = this.compMove
            this.gridSelection[2].disabled = 1
          }
          // // RANDOM MOVE // //
          // while(okNum){
          //   let rand = _.random(0, 8);
          //   if(this.gridValue[rand] == 0){
          //     this.gridValue[rand] = this.aiValue
          //     this.gridSelection.forEach(element => {
          //       if(element.id == rand){
          //         element.disabled = 1
          //         element.value = this.compMove
          //       }
          //     });
          //     okNum = false
          //   }
          // }
        }
      //end first move
      }else{
        this.calculateWin()
        //check if can win x
        let aiHasWon = this.checkAiWin()
        console.log(aiHasWon)
        //then block
        if(!aiHasWon){
          this.aiBlock()
        }
      }

      if(this.winner == 0){
        this.calculateWin()
        this.checkWinner()
      

        this.moveCount++
        if(this.compMove == 'x'){
          this.whosMove = 'o'
        }else{
          this.whosMove = "x"
        }
      }
    },
    checkAiWin(){
      let i = 0
      if(this.row1 == this.winAi){
        for(i=0; i < 3; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }else if(this.row2 == this.winAi){
        for(i=3; i < 6; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }if(this.row3 == this.winAi){
        for(i=6; i < 9; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }
      if(this.col1 == this.winAi){
        for(i=0; i < 7; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }if(this.col2 == this.winAi){
        for(i=1; i < 8; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }if(this.col3 == this.winAi){
        for(i=2; i < 9; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }if(this.dia1 == this.winAi){
        for(i=0; i < 9; i+=4){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }if(this.dia2 == this.winAi){
        for(i=2; i < 7; i+=2){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      } 
      //end check ai Win
    },
    aiBlock(){
      let i = 0
      if(this.row1 == this.winPlayer){
        for(i=0; i < 3; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.row2 == this.winPlayer){
        for(i=3; i < 6; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.row3 == this.winPlayer){
        for(i=6; i < 9; i++){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.col1 == this.winPlayer){
        for(i=0; i < 7; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.col2 == this.winPlayer){
        for(i=1; i < 8; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.col3 == this.winPlayer){
        for(i=2; i < 9; i+=3){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.dia1 == this.winPlayer){
        for(i=0; i < 9; i+=4){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
          }
        }
      }else if(this.dia2 == this.winPlayer){
        for(i=2; i < 7; i+=2){
          if(this.gridValue[i] == 0){
            this.gridValue[i] = this.aiValue
            this.gridSelection[i].disabled = 1
            this.gridSelection[i].value = this.compMove
            return true
          }
        }
      }else{
        let okNum = true
        while(okNum){
          let rand = _.random(0, 8);
          if(this.gridValue[rand] == 0){
            this.gridValue[rand] = this.aiValue
            this.gridSelection.forEach(element => {
              if(element.id == rand){
                element.disabled = 1
                element.value = this.compMove
              }
            });
            okNum = false
          }
        }
      }
      //end block
    },

  }//END METHODS
}
</script>
