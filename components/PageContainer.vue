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
    <!-- Top bar -->
    <div class="top-bar" @mousedown.stop="startDrag">
      <span class="title">{{ title }}</span>
      <button class="close-button" @click="closeContainer">✕</button>
    </div>

    <!-- Content slot -->
    <slot></slot>

    <!-- Resize handle -->
    <div class="resize-handle" @mousedown.stop="startResize"></div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: "Page Container", // Default title if none is provided
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
      this.isDragging = true;
      this.offset.x = event.clientX - this.position.x;
      this.offset.y = event.clientY - this.position.y;
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.stopDrag);
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
    onDrag(event) {
      if (this.isDragging) {
        this.position.x = event.clientX - this.offset.x;
        this.position.y = event.clientY - this.offset.y;
      }
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
    stopResize() {
      this.isResizing = false;
      document.removeEventListener("mousemove", this.onResize);
      document.removeEventListener("mouseup", this.stopResize);
    },
    onResize(event) {
      if (this.isResizing) {
        this.size.width = this.resizeStart.width + (event.clientX - this.resizeStart.x);
        this.size.height = this.resizeStart.height + (event.clientY - this.resizeStart.y);
      }
    },
    closeContainer() {
      this.$emit("close"); // Emit a close event to the parent
    },
  },
};
</script>

<style>
.page-container {
  position: absolute;
  border: 1px solid #ccc;
  background-color: #fff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

.top-bar {
  background-color: #0078d7;
  color: white;
  padding: 8px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  cursor: move;
}

.title {
  font-size: 14px;
  font-weight: bold;
}

.close-button {
  background: none;
  border: none;
  color: white;
  font-size: 16px;
  cursor: pointer;
}

.close-button:hover {
  color: #ff5c5c;
}

.resize-handle {
  width: 10px;
  height: 10px;
  background-color: #ccc;
  position: absolute;
  bottom: 0;
  right: 0;
  cursor: se-resize;
}
</style>