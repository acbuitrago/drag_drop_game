<template>
  <div class="score">
      <div class="title">Puntaje</div>
      {{score}}
  </div>
</template>
<script>
export default {
  name: "Score",
  props: ["score"]
};
</script>
<style scoped lang="scss">
.score {
  background-color: #bbb;
  border-radius: 4px;
  margin: 10px;
  padding: 10px;
  transition: background-color 0.3s;
  color: white;
  user-select: none;
  position: absolute;
  right: 0;
  top: 0;

  .title {
      font-weight: bold;
      text-transform: uppercase;
      color: #fff;
  }
}

</style>
