<template>
  <div class="app" :class="{gameOver: gameOver}">
    <div class="side">
    <Score :score="score"/>
     <Draggable v-if="optionsArray.length > 0 && !gameOver" :option="optionsArray[currentIndex]" />
    </div>
    <div class="side classes">
      <Dropzone v-for="item in game.groups" :key="item.id" :group="item" v-on:dropped="onItemDropped(item)" />
    </div>
  </div>
</template>

<script>
import Draggable from "./components/Draggable.vue";
import Dropzone from "./components/Dropzone.vue";
import Score from "./components/Score.vue";
import json from "./assets/data.json";
import Config from "./config.ts";

export default {
  components: {
    Draggable,
    Dropzone, 
    Score
  },
  data() {
    return {
      game: json,
      score: 0,
      currentIndex: 0,
      optionsArray: [],
      gameOver: false
    }
  },
  mounted: function() {
    this.optionsArray = this.shuffleArray(this.game.options);
  },
  methods: {
    shuffleArray: function(original) {
      const array = original.slice(0);
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    },
    onItemDropped: function(group){
      if (group.name === this.optionsArray[this.currentIndex].class){
        this.score += Config.rightPoints;
        this.$swal({ icon: 'success', title: 'Correcto',
                    showConfirmButton: false,
                    timer: 1000});
      } else {
        this.$swal({ icon: 'error', title: 'Oops...', text: '¡Respuesta incorrecta!'});
      }
     
      this.currentIndex++;
      if(this.currentIndex === this.optionsArray.length){
        this.gameOver = true;
         this.$swal({ icon: 'info', 
         title: 'Game Over', 
         text: 'Tu puntaje fue: '+ this.score, 
         confirmButtonText: '¡Reiniciar!'}).then((result) => {
            if (result.value) {
              this.restartGame();
            }
         });
      }
    },
    restartGame: function() {
      this.optionsArray = this.shuffleArray(this.game.options);
      this.currentIndex = 0;
      this.gameOver = false;
      this.score = 0;
    }
  }
};
</script>

<style lang="scss">
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100vw;
  transition: 1s filter ease-in-out;
  &.gameOver {
    filter: blur(20px);
  }
}

.side {
  flex-grow: 1;
  padding: 40px;
}
.classes {
  display: flex;
  justify-content: space-around;
}
html,
body {
  margin: 0;
    font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
}

</style>
