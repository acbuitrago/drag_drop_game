<template>
  <div
    ref="myDraggable"
    class="draggable"
    :style="{ transform: transformString }"
    :class="{isAnimating: isInteractAnimating}"
  >
    <div>{{option.name}}</div>

    <img v-bind:src="require(`@/assets/${option.image}`)" />
  </div>
</template>

<script>
import interact from "interactjs";

export default {
  name: "Draggable",
  props: ["option"],
  data() {
    return {
      interactPosition: {
        x: 0,
        y: 0
      },
      isInteractAnimating: true
    };
  },
  computed: {
    transformString() {
      const { x, y } = this.interactPosition;
      return `translate3D(${x}px, ${y}px, 0)`;
    }
  },
  mounted: function() {
    const myDraggable = this.$refs.myDraggable;
    this.initInteract(myDraggable);
  },
  beforeDestroy() {
    interact(this.$refs.myDraggable).unset();
  },
  methods: {
    initInteract: function(selector) {
      interact(selector).draggable({
        autoScroll: false,
        onmove: this.dragMoveListener,
        onend: this.resetCardPosition
      });
    },
    dragMoveListener: function(event) {
      const x = this.interactPosition.x + event.dx;
      const y = this.interactPosition.y + event.dy;
      this.interactSetPosition({ x, y });
    },
    interactSetPosition: function(coordinates) {
      const { x = 0, y = 0 } = coordinates;
      this.interactPosition = { x, y };
    },
    resetCardPosition: function() {
      this.interactSetPosition({ x: 0, y: 0 });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.draggable {
  width: 150px;
  height: 150px;
  border: solid 3px teal;
  color: black;
  padding: 5px;
  touch-action: none;
  user-select: none;
  -ms-touch-action: none;
  display: flex;
  flex-flow: column nowrap;

  img {
    min-height: 0;
    max-width: 100%;
    max-height: 100%;
    object-fit: scale-down;
    flex: 1;
  }
}
</style>
