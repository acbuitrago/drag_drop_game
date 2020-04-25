<template>
  <div id="app">
    <div class="side">
      <Draggable v-for="item in game.options" :key="item.id" />
    </div>
    <div class="side classes">
      <Dropzone v-for="item in game.groups" :key="item.id" :group="item"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Draggable from "./components/Draggable.vue";
import Dropzone from "./components/Dropzone.vue";
import json from "./assets/data.json";

@Component({
  components: {
    Draggable,
    Dropzone
  }
})
export default class App extends Vue {
  data() {
    return {
      game: json
    };
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  height: 100%;
  width: 100%;
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
  height: 100%;
  width: 100%;
  margin: 0;
}
</style>
