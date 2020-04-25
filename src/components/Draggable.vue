<template>
  <div ref="myDraggable" class="draggable"
  :style="{ transform: transformString }"
  :class="{isAnimating: isInteractAnimating}">
       {{option.name}}
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
      const x = this.interactPosition.x + event.dx
      const y = this.interactPosition.y + event.dy
      this.interactSetPosition({ x, y });
    },
    interactSetPosition: function(coordinates) {
      const { x = 0, y = 0 } = coordinates;
      this.interactPosition = { x, y };
    },
    resetCardPosition: function() {
      this.interactSetPosition({ x: 0, y: 0 })
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.draggable {
  width: 100px;
  height: 100px;
  background-color: teal;
  color: #fff;
  padding: 5px;
  position: absolute;
  touch-action: none;
  user-select: none;
    -ms-touch-action: none;
}
</style>
