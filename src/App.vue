<template>
  <div id="app">
    <h1>GCode Preview Vue Demo</h1>
    <GCodePreview ref="preview" 
      :upperLayerLimit="Infinity"
      :topLayerColor="'lime'"
      :lastSegmentColor="'red'"
    />
    <div># layers loaded: {{ layersLoaded }}</div>
  </div>
</template>

<script>
import GCodePreview from './components/GCodePreview.vue';
let layersLoaded = 0;
export default {
  components: {
    GCodePreview
  },

  data() {
    return {
      layersLoaded : layersLoaded
    }
  },

  async mounted() {
    const response = await fetch('/benchy.gcode');

    if (response.status !== 200) {
      throw new Error(`status code: ${response.status}`);
    }

    this.file = await response.text();
    const lines = this.file.split('\n');
    const chunkSize = 500;
    const preview = this.$refs.preview;

    let c = 0;
    const loadProgressive = () => {
      const start = c*chunkSize;
      const end = (c+1)*chunkSize;
      const chunk = lines.slice(start, end);
      preview.processGCode(chunk)
      this.layersLoaded = preview.layerCount;
      c++;
      if (c*chunkSize < lines.length) { 
        window.__animationTimer__ = setTimeout(loadProgressive, 500);
      }
    }
    // cancel loading process if one is still in progress
    // mostly when hot reloading
    window.clearTimeout(window.__animationTimer__);
    loadProgressive();
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
