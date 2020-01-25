<template>
  <div>
    <div id="gcode-preview"></div>
  </div>
</template>

<script>
'use strict'

import { WebGLPreview } from 'gcode-preview';
import * as THREE from 'three';

let preview;
export default {
  props: {
    topLayerColor: String,
    lastSegmentColor: String,
    upperLayerLimit: Number,
    lineWidth: Number
  },

  data() {
    return {
      layerCount: 0
    }
  },

  mounted() {

    preview = new WebGLPreview({
      targetId: 'gcode-preview',
      limit: this.upperLayerLimit,
      topLayerColor: new THREE.Color(this.topLayerColor).getHex(),
      lastSegmentColor: new THREE.Color(this.lastSegmentColor).getHex(),
      lineWidth: this.lineWidth
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