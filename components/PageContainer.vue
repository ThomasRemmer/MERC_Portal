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
  >
    <slot></slot>
    <div class="resize-handle" @mousedown.stop="startResize"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      position: { x: 100, y: 100 }, // Initial position of the window
      size: { width: 400, height: 300 }, // Initial size of the window
      isDragging: false,
      isResizing: false,
      offset: { x: 0, y: 0 },
      resizeStart: { width: 0, height: 0, x: 0, y: 0 },
    };
  },
  methods: {
    startDrag(event) {
      this.isDragging = true;
      this.offset.x = event.clientX - this.position.x;
      this.offset.y = event.clientY - this.position.y;
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.stopDrag);
    },
    onDrag(event) {
      if (this.isDragging) {
        this.position.x = event.clientX - this.offset.x;
        this.position.y = event.clientY - this.offset.y;
      }
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
    startResize(event) {
      this.isResizing = true;
      this.resizeStart.width = this.size.width;
      this.resizeStart.height = this.size.height;
      this.resizeStart.x = event.clientX;
      this.resizeStart.y = event.clientY;
      document.addEventListener("mousemove", this.onResize);
      document.addEventListener("mouseup", this.stopResize);
    },
    onResize(event) {
      if (this.isResizing) {
        this.size.width =
          this.resizeStart.width + (event.clientX - this.resizeStart.x);
        this.size.height =
          this.resizeStart.height + (event.clientY - this.resizeStart.y);
      }
    },
    stopResize() {
      this.isResizing = false;
      document.removeEventListener("mousemove", this.onResize);
      document.removeEventListener("mouseup", this.stopResize);
    },
  },
};
</script>

<style>
.page-container {
  position: absolute;
  background-color: white;
  border: 1px solid black;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  cursor: move;
  display: flex;
  flex-direction: column;
  user-select: none;
}

.resize-handle {
  position: absolute;
  width: 10px;
  height: 10px;
  background-color: gray;
  bottom: 0;
  right: 0;
  cursor: se-resize;
}
</style>
