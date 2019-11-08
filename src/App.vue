<template>
  <div>
    <h1>Vue.js ◯✕ゲーム</h1>
    <table id="table">
      <tr v-for="(row, rowsIndex) in states" :key="rowsIndex">
        <td v-for="(state, colsIndex) in row" :key="colsIndex" @click="Onselect(rowsIndex, colsIndex)">
          <div style="color:#f00;" v-if="state==1">○</div>
          <div style="color:#00f;" v-if="state==2">✕</div>
        </td>
      </tr>
    </table>

    <div style="text-align:center;">
      <div style="color:#f00;" v-if="playerId==1">○プレイヤーさん、マスを選んでください</div>
      <div style="color:#00f;" v-if="playerId==2">✕プレイヤーさん、マスを選んでください</div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return{
      states: [
        [-1, -1, -1],
        [-1, -1, -1],
        [-1, -1, -1],
      ],
      playerId: 1 //○のプレイヤーから開始
    }
  },
  methods: {
    Onselect: function(rowsIndex, colsIndex){
      // console.log(rowsIndex,colsIndex)
      if(this.states[rowsIndex][colsIndex] != -1){
        alert('そのマスはすでに選択されています。')
      } else {
        var states = JSON.parse(JSON.stringify(this.states))
        states[rowsIndex][colsIndex] = this.playerId
        this.states = states
        this.playerId = (this.playerId == 1) ? 2 : 1

        var winnerId = this.getWinnerId()

        if(winnerId != -1){
          this.states = [[-1, -1, -1],[-1, -1, -1],[-1, -1, -1]]
          var playerIds = {1: '◯',2: '✕'}
          alert(playerIds[winnerId] + "さんの勝ちです。おめでとうございます。")

        } else if(this.isDraw()){

          this.states = [
            [-1, -1, -1],
            [-1, -1, -1],
            [-1, -1, -1]
          ]
          alert("引き分けです!")
        }
      }
    },
    getWinnerId: function(){
      for(var i = 0;i < 3; i++ ){

        // 横の例
        var row = this.states[i]
        if(this.isStatesFilled(row)){ return row[0] }

        // 縦の例
        var col = [
          this.states[0][i],
          this.states[1][i],
          this.states[2][i] 
        ]
        if(this.isStatesFilled(col)){ return this.states[0][i]}

      }

      // ななめ
      var skew1 = [
        this.states[0][0],
        this.states[1][1],
        this.states[2][2]
      ]
      if(this.isStatesFilled(skew1)){ return this.states[0][0]}

      var skew2 = [
        this.states[0][2],
        this.states[1][1],
        this.states[2][0]
      ]
      if(this.isStatesFilled(skew2)){ return this.states[0][2]}

      return -1

    },

    isStatesFilled: function(states){
      return(
        states[0] != -1 &&
        states[0] == states[1] &&
        states[1] == states[2]
      )
    },
    isDraw: function(){
      for(var i in this.states){
        var row = this.states[i]
        for(var j in row){
          var state = row[j]
          if(state == -1){
            return false
          }
        }
      }
      
      return true
    }
  }
}
</script>

<style lang="scss">
#table {
  margin:30px auto;
  border-collapse: collapse;
  border:3px solid #ccc;
}

#table td {
  border:1px solid #ccc;
  height: 80px;
  width:80px;
  text-align:center;
  vertical-align: middle;
  font-size: 60px;
  cursor:pointer;
}


</style>
