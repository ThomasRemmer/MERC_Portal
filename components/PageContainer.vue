<template>
  <div
    class="page-container"
    :style="{
      top: position.y + 'px',
      left: position.x + 'px',
      width: size.width + 'px',
      height: size.height + 'px',
    }"
    @mousedown="startDrag"
    @touchstart="startDrag"
  >

    <div
      class="top-bar"
      @mousedown.stop="startDrag"
      @touchstart.stop="startDrag"
    >
      <span class="title">{{ title }}</span>
      <button class="close-button" @click="closeContainer">✕</button>
    </div>


    <slot></slot>


    <div
      class="resize-handle"
      @mousedown.stop="startResize"
      @touchstart.stop="startResize"
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: "Page Container",
    },
  },
  data() {
    return {
      position: { x: 100, y: 100 },
      size: { width: 400, height: 300 },
      isDragging: false,
      isResizing: false,
      offset: { x: 0, y: 0 },
      resizeStart: { width: 0, height: 0, x: 0, y: 0 },
    };
  },
  methods: {
    startDrag(event) {
      event.preventDefault();
      this.isDragging = true;
      const clientX = event.touches ? event.touches[0].clientX : event.clientX;
      const clientY = event.touches ? event.touches[0].clientY : event.clientY;
      this.offset.x = clientX - this.position.x;
      this.offset.y = clientY - this.position.y;
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.stopDrag);
      document.addEventListener("touchmove", this.onDrag, { passive: false });
      document.addEventListener("touchend", this.stopDrag);
    },
    onDrag(event) {
      if (this.isDragging) {
        event.preventDefault();
        const clientX = event.touches
          ? event.touches[0].clientX
          : event.clientX;
        const clientY = event.touches
          ? event.touches[0].clientY
          : event.clientY;
        this.position.x = clientX - this.offset.x;
        this.position.y = clientY - this.offset.y;
      }
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
      document.removeEventListener("touchmove", this.onDrag);
      document.removeEventListener("touchend", this.stopDrag);
    },
    startResize(event) {
      event.preventDefault();
      this.isResizing = true;
      const clientX = event.touches ? event.touches[0].clientX : event.clientX;
      const clientY = event.touches ? event.touches[0].clientY : event.clientY;
      this.resizeStart.width = this.size.width;
      this.resizeStart.height = this.size.height;
      this.resizeStart.x = clientX;
      this.resizeStart.y = clientY;
      document.addEventListener("mousemove", this.onResize);
      document.addEventListener("mouseup", this.stopResize);
      document.addEventListener("touchmove", this.onResize, { passive: false });
      document.addEventListener("touchend", this.stopResize);
    },
    onResize(event) {
      if (this.isResizing) {
        event.preventDefault();
        const clientX = event.touches
          ? event.touches[0].clientX
          : event.clientX;
        const clientY = event.touches
          ? event.touches[0].clientY
          : event.clientY;
        this.size.width =
          this.resizeStart.width + (clientX - this.resizeStart.x);
        this.size.height =
          this.resizeStart.height + (clientY - this.resizeStart.y);
      }
    },
    stopResize() {
      this.isResizing = false;
      document.removeEventListener("mousemove", this.onResize);
      document.removeEventListener("mouseup", this.stopResize);
      document.removeEventListener("touchmove", this.onResize);
      document.removeEventListener("touchend", this.stopResize);
    },
    closeContainer() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.page-container {
  position: absolute;
  border-top: #fff solid 2px;
  border-left: #fff solid 2px;
  border-right: #000 solid 2px;
  border-bottom: #000 solid 2px;
  background-color: #c3c7cb;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  font-family: "Microsoft Sans Serif", Arial, sans-serif;
}

.top-bar {
  background-color: #000080;
  color: white;
  padding: 4px 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: move;
  border: #c3c7cb solid 2px;
}

.title {
  font-size: 12px;
  font-weight: bold;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.close-button {
  background-color: #c3c7cb;
  border: 1px solid #000;
  color: black;
  font-size: 12px;
  font-weight: bold;
  width: 16px;
  height: 16px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  box-shadow: 1px 1px 0px #fff, -1px -1px 0px #000;
}

.close-button:active {
  box-shadow: inset 1px 1px 0px #000, inset -1px -1px 0px #fff;
}

.resize-handle {
  width: 10px;
  height: 10px;
  background-color: #c3c7cb;
  border: 2px solid #000;
  position: absolute;
  bottom: 0;
  right: 0;
  cursor: se-resize;

}
</style>
