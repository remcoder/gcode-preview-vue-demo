<template>
  <div id="gcode-preview"></div>
</template>

<script>
import { WebGLPreview } from 'gcode-preview/dist/gcode-preview';

let preview;
export default {
  props: {
    gcode: String
  },

  mounted() {
    preview = new WebGLPreview({
      targetId: 'gcode-preview',
    });

    window.addEventListener('resize', () => {
      preview.resize();
    });

    preview.resize();
  },

  watch : {
    gcode: function() {
      preview.processGCode(this.gcode);
      preview.render();
    }
  }
}
</script>
<style scoped>
  #gcode-preview {
    height: 400px;
  }
</style>