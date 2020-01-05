<template>
  <div>
    <div id="gcode-preview"></div>
    <div># layers: {{ layerCount }}</div>
  </div>
</template>

<script>
import { WebGLPreview } from 'gcode-preview';

let preview;
export default {
  props: {
    gcode: String
  },

  data() {
    return {
      layerCount: 0
    }
  },

  mounted() {
    preview = new WebGLPreview({
      targetId: 'gcode-preview',
      limit: 200
    });

    window.addEventListener('resize', () => {
      preview.resize();
    });

    preview.resize();
  },

  methods: {
    processGCode(gcode) {
      preview.processGCode(gcode);
      this.layerCount = preview.layers.length;
    }
  }
}
</script>
<style>
  #gcode-preview {
    height: 400px;
  }
</style>