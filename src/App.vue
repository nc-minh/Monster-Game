<template>
  <div id="app">
    <div class="figure">
      <div class="figure__item">
        <div class="figure__item__name">
          <h3>You</h3>
        </div>
        <div class="figure__item__img">
          <img src="https://github.com/nguyencongminh-dev/Monster-Game/blob/master/src/assets/you.png?raw=true" alt="" class="figure__item__img--link">
        </div>
        <div class="figure__item__HP">
          <div class="figure__item__HP--percen" :style="{width: playerHP + '%'}"></div>
          <div class="figure__item__HP--percen-details">{{ playerHP }}</div>
        </div>
      </div>
      <div class="figure__item">
        <div class="figure__item__name">
          <h3>Monster</h3>
        </div>
        <div class="figure__item__img">
          <img src="https://github.com/nguyencongminh-dev/Monster-Game/blob/master/src/assets/monster.png?raw=true" alt="" class="figure__item__img--link">
        </div>
        <div class="figure__item__HP">
          <div class="figure__item__HP--percen" :style="{width: monsterHP + '%'}"></div>
          <div class="figure__item__HP--percen-details">{{ monsterHP }}</div>
        </div>
      </div>
    </div>

    <div class="action">
      <div class="action__start" v-if="!isStart">
        <button class="action__start__btn"
        @click="startNewGame"
        >START NEW GAME</button>
      </div>
      <div class="action__skill" v-else>
        <button class="action__skill__item ATTACK"
        @click="ATTACK"
        >ATTACK</button>
        <button class="action__skill__item SPECIAL"
        @click="SPECIAL"
        >SPECIAL ATTACK</button>
        <button class="action__skill__item HEAL"
        @click="HEAL"
        >HEAL</button>
        <button class="action__skill__item GIVE_UP"
        @click="GIVE_UP"
        >GIVE UP</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      isStart: false,
      playerHP: 50,
      monsterHP: 50,
    }
  },
  methods:{
    startNewGame: function(){
      this.isStart = true;
      this.playerHP = 100;
      this.monsterHP = 100;
    },
    ATTACK: function(){
      //check
      this.monsterHP -= this.inputDamage(3, 15);
      
      //player
      this.playerHP -= this.inputDamage(3, 15);

      this.checkPlayerOptions()
    },
    SPECIAL: function(){
      this.monsterHP -= this.inputDamage(15, 20);
      
      //player
      this.playerHP -= this.inputDamage(15, 20);

      this.checkPlayerOptions()
    },
    HEAL: function(){
      if(this.playerHP >= 70){
        alert('HP < 70 mới được hồi!')
        return false;
      }else if(this.playerHP <= 60){
        this.playerHP += 10;
      }else{
        this.playerHP = 70;
      }
      this.playerHP -= this.inputDamage(3, 15);

    },
    GIVE_UP: function(){

    },
    inputDamage: function(minDamage, maxDamage){
      return Math.max(Math.floor(Math.random() * maxDamage) + 1, minDamage)
    },
    checkPlayerOptions: function(){
      if(this.monsterHP <= 0){
        this.monsterHP = 0;
        setTimeout(()=>{
          if(confirm('You won! New game?')){
          this.startNewGame();
          }else{
            this.isStart = false
          }
        }, 100)
      }else if(this.playerHP <= 0){
        this.playerHP = 0;
        setTimeout(()=>{
          if(confirm('You lost! New game?')){
          this.startNewGame();
          }else{
            this.isStart = false
          }
        }, 100)
      }
      return;
    }
  },
  components: {

  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.figure{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.figure__item{
  width: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.figure__item__name{
  font-size: 2rem;
  margin-bottom: 20px;
}

.figure__item__img{
  height: 400px;
}

.figure__item__img--link{
  height: 100%;
}

.figure__item__HP{
  margin-top: 20px;
  width: 200px;
  height: 20px;
  background-color: rgb(133, 144, 163);
  position: relative;
  overflow: hidden;
  color: #fff;
}

.figure__item__HP--percen{
  position: absolute;
  height: 20px;
  background-color: crimson;
  transition: width .5s;
}

.figure__item__HP--percen-details{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.action{
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.action__start,
.action__skill{
  border: 3px solid #ccc;
  width: 600px;
  height: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.action__start__btn,
.action__skill__item{
  background-color: darkgreen;
  color: #fff;
  padding: 10px 20px;
  cursor: pointer;
}

.action__skill__item{
  margin-right: 5px;
  color: #000;
}

.ATTACK{
  background-color: rgb(255, 0, 0);
}

.SPECIAL{
  background-color: gold;
}

.HEAL{
  background-color: rgb(186, 102, 247);
}

.GIVE_UP{
  background-color: ivory;
}
</style>
