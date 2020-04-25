<template>
  <div ref="myDropzone" class="dropzone"
  v-bind:style="{'background-color': group.color}"
  :class="{'drop-active': dropActive, 'drop-target': dragIn}"
  >
    {{group.name}}
  </div>
</template>
<script>
import interact from "interactjs";

export default {
  name: "Dropzone",
  props: ["group"],
   data() {
    return {
      dropActive: false,
      dragIn: false
    }
  },
  mounted: function() {
    const myDropzone = this.$refs.myDropzone;
    this.initInteract(myDropzone);
  },
  methods: {
    initInteract: function(selector) {
      interact(selector).dropzone({
        accept: ".draggable",
        ondropactivate: this.onDropActiveListener,
        ondragenter: this.onDragEnterListener,
        ondragleave: this.onDragLeaveListener,
        ondrop: this.onDropListener,
        ondropdeactivate: this.onDropDeactivateListener
      });
    },
    onDropActiveListener: function(event) {
      this.dropActive = true;
    },
    onDragEnterListener: function(event) {
      console.log("onDragEnterListener", event);
      this.dragIn = true;
      const draggableElement = event.relatedTarget;
      const dropzoneElement = event.target;
    },
    onDragLeaveListener: function(event) {
      console.log("onDragLeaveListener", event);
      this.dragIn = false;
    },
    onDropListener: function(event) {
      console.log("onDropListener", event);
      this.$emit('dropped');
      this.dragIn = false;
    },
    onDropDeactivateListener: function(event) {
      console.log("onDropDeactivateListener", event);
      // remove active dropzone feedback
      this.dropActive = false;
    }
  }
};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.dropzone {
  background-color: #ccc;
  border: dashed 4px transparent;
  border-radius: 4px;
  margin: 10px;
  padding: 10px;
  transition: background-color 0.3s;
  flex-grow: 1;
  color: white;
  user-select: none;
}

.drop-active {
  border-color: #aaa;
}

.drop-target {
  opacity: 0.5;
  border-color: #fff;
  border-style: solid;
}

</style>
