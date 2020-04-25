<template>
  <div ref="myDropzone" class="dropzone" v-bind:style="{'background-color': group.color}">{{group.name}}</div>
</template>
<script>
import interact from "interactjs";

export default {
  name: "Dropzone",
   props: [
       'group'
    ],
  mounted: function() {
    console.log("Mounted?");
    const myDropzone = this.$refs.myDropzone;
    this.initInteract(myDropzone);
  },
  methods: {
    initInteract: function(selector) {
      interact(selector).dropzone({
        // only accept elements matching this CSS selector
        accept: "#yes-drop",
        // Require a 75% element overlap for a drop to be possible
        overlap: 0.75,
        ondropactivate: this.onDropActiveListener,
        ondragenter: this.onDragEnterListener,
        ondragleave: this.onDragLeaveListener,
        ondrop: this.onDropListener,
        ondropdeactivate: this.onDropDeactivateListener
      });
    },
    onDropActiveListener: function(event) {
      console.log("OndropAcivate", event);
      event.target.classList.add("drop-active");
    },
    onDragEnterListener: function(event) {
      console.log("onDragEnterListener", event);
      const draggableElement = event.relatedTarget;
      const dropzoneElement = event.target;

      // feedback the possibility of a drop
      dropzoneElement.classList.add("drop-target");
      draggableElement.classList.add("can-drop");
      draggableElement.textContent = "Dragged in";
    },
    onDragLeaveListener: function(event) {
      console.log("onDragLeaveListener", event);
      // remove the drop feedback style
      event.target.classList.remove("drop-target");
      event.relatedTarget.classList.remove("can-drop");
      event.relatedTarget.textContent = "Dragged out";
    },
    onDropListener: function(event) {
      console.log("onDropListener", event);
      event.relatedTarget.textContent = "Dropped";
    },
    onDropDeactivateListener: function(event) {
      console.log("onDropDeactivateListener", event);
      // remove active dropzone feedback
      event.target.classList.remove("drop-active");
      event.target.classList.remove("drop-target");
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
}

.drop-active {
  border-color: #aaa;
}

.drop-target {
  background-color: #29e;
  border-color: #fff;
  border-style: solid;
}

.drag-drop {
  display: inline-block;
  min-width: 40px;
  padding: 2em 0.5em;

  color: #fff;
  background-color: #29e;
  border: solid 2px #fff;

  touch-action: none;
  -webkit-transform: translate(0px, 0px);
  transform: translate(0px, 0px);

  transition: background-color 0.3s;
}

.drag-drop.can-drop {
  color: #000;
  background-color: #4e4;
}
</style>
