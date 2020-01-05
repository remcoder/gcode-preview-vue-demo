<template>
  <div id="app">
    <h1>GCode Preview Vue Demo</h1>
    <GCodePreview ref="preview" :gcode="file" />
  </div>
</template>

<script>
import GCodePreview from './components/GCodePreview.vue';

export default {
  components: {
    GCodePreview
  },

  data() {
    return {
      file : ''
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
    let c = 0;
    const preview = this.$refs.preview;

    function loadProgressive() {
      const start = c*chunkSize;
      const end = (c+1)*chunkSize;
      const chunk = lines.slice(start, end);
      preview.processGCode(chunk)
      c++;
      if (c*chunkSize < lines.length) { 
        setTimeout(loadProgressive, 500);
      }
    }
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
