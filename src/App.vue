<script setup>
import { ref, computed } from 'vue'


const columnsCount = ref(3)
const rowsCount = ref(2)
const gridGap = ref(16)
const containerBg = ref('#0f172a')
const itemColor = ref('#38bdf8')


const dynamicGridStyle = computed(() => {
  return {
    display: 'grid',
    gridTemplateColumns: `repeat(${columnsCount.value}, 1fr)`,
    gridTemplateRows: `repeat(${rowsCount.value}, minmax(80px, auto))`,
    gap: `${gridGap.value}px`,
    backgroundColor: containerBg.value,
    padding: '20px',
    borderRadius: '12px',
    transition: 'all 0.15s linear'
  }
})


const compiledCssCode = computed(() => {
  return `.grid-container {\n` +
         `  display: grid;\n` +
         `  grid-template-columns: repeat(${columnsCount.value}, 1fr);\n` +
         `  grid-template-rows: repeat(${rowsCount.value}, 1fr);\n` +
         `  gap: ${gridGap.value}px;\n` +
         `}`
})


const copyStatus = ref('📋 Copy Layout CSS')
const copyToClipboard = () => {
  navigator.clipboard.writeText(compiledCssCode.value)
  copyStatus.value = 'Copied to Clipboard! ⚡'
  setTimeout(() => {
    copyStatus.value = '📋 Copy Layout CSS'
  }, 2000)
}
</script>

<template>
  <div class="app-wrapper">
    
    <header class="app-header">
      <h1> DevGrid Architect</h1>
      <p>An interactive Vue 3 Single-File sandbox designed to prototype and compute dynamic CSS Grid structures.</p>
    </header>

    <main class="studio-desk">
      
      <section class="control-panel">
        <h3>Grid Matrix Settings</h3>
        
        <div class="control-group">
          <label>Column Track Splitting ({{ columnsCount }})</label>
          <input type="range" min="1" max="6" v-model.number="columnsCount">
        </div>

        <div class="control-group">
          <label>Row Track Splitting ({{ rowsCount }})</label>
          <input type="range" min="1" max="5" v-model.number="rowsCount">
        </div>

        <div class="control-group">
          <label>Grid Track Gap ({{ gridGap }}px)</label>
          <input type="range" min="0" max="40" step="4" v-model.number="gridGap">
        </div>

        <div class="color-picker-row">
          <div class="control-group flex-1">
            <label>Canvas BG</label>
            <input type="color" v-model="containerBg">
          </div>
          <div class="control-group flex-1">
            <label>Card Accent</label>
            <input type="color" v-model="itemColor">
          </div>
        </div>

        <div class="code-export-box">
          <div class="code-header">
            <span>Generated Production CSS</span>
            <button @click="copyToClipboard" class="copy-btn">{{ copyStatus }}</button>
          </div>
          <pre><code>{{ compiledCssCode }}</code></pre>
        </div>
      </section>

      <section class="canvas-panel">
        <h3 class="panel-title">Interactive Layout Render Canvas</h3>
        
        <div :style="dynamicGridStyle" class="interactive-grid-container">
          <div 
            v-for="index in (columnsCount * rowsCount)" 
            :key="index" 
            class="grid-item-card"
            :style="{ borderColor: itemColor, color: itemColor }"
          >
            <span class="item-index">Box {{ index }}</span>
          </div>
        </div>
      </section>

    </main>
  </div>
</template>

<style scoped>
.app-wrapper {
  max-width: 1200px;
  margin: 40px auto;
  padding: 0 24px;
  font-family: system-ui, -apple-system, sans-serif;
  color: #f8fafc;
}

.app-header {
  border-bottom: 2px solid #1e293b;
  padding-bottom: 20px;
  margin-bottom: 35px;
}

.app-header h1 {
  margin: 0;
  font-size: 30px;
  color: #38bdf8;
  letter-spacing: -0.5px;
}

.app-header p {
  margin: 4px 0 0 0;
  color: #64748b;
  font-size: 14px;
}

.studio-desk {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(360px, 1fr));
  gap: 40px;
}

.control-panel {
  background-color: #0f172a;
  border: 1px solid #1e293b;
  padding: 25px;
  border-radius: 16px;
  box-shadow: 0 10px 15px -3px rgba(0,0,0,0.3);
}

.control-panel h3 {
  margin-top: 0;
  margin-bottom: 20px;
  font-size: 14px;
  color: #64748b;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.control-group {
  margin-bottom: 22px;
}

.control-group label {
  display: block;
  font-size: 13px;
  font-weight: 700;
  color: #cbd5e1;
  margin-bottom: 8px;
}

.control-panel input[type="range"] {
  width: 100%;
  cursor: pointer;
  accent-color: #38bdf8;
}

.color-picker-row {
  display: flex;
  gap: 15px;
}

.flex-1 { flex: 1; }

.control-panel input[type="color"] {
  width: 100%;
  height: 38px;
  background: none;
  border: 1px solid #1e293b;
  border-radius: 8px;
  cursor: pointer;
  box-sizing: border-box;
}

.code-export-box {
  margin-top: 25px;
  background-color: #020617;
  border: 1px solid #1e293b;
  border-radius: 10px;
  padding: 15px;
}

.code-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 11px;
  color: #64748b;
  font-weight: 700;
  text-transform: uppercase;
  margin-bottom: 10px;
}

.copy-btn {
  background: none;
  border: 1px solid #38bdf8;
  color: #38bdf8;
  padding: 4px 10px;
  border-radius: 6px;
  font-size: 11px;
  font-weight: 700;
  cursor: pointer;
  transition: 0.15s;
}

.copy-btn:hover {
  background-color: #38bdf8;
  color: #020617;
}

.code-export-box pre {
  margin: 0;
  font-family: monospace;
  font-size: 13px;
  color: #34d399;
  line-height: 1.5;
}

.canvas-panel {
  display: flex;
  flex-direction: column;
}

.panel-title {
  margin-top: 0;
  margin-bottom: 15px;
  font-size: 14px;
  color: #64748b;
  text-transform: uppercase;
}

.interactive-grid-container {
  flex-grow: 1;
  min-height: 350px;
  border: 1px dashed #334155;
}

.grid-item-card {
  border: 2px solid transparent;
  background-color: rgba(255, 255, 255, 0.03);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  font-weight: bold;
  box-shadow: inset 0 0 15px rgba(255,255,255,0.01);
}

.item-index {
  font-size: 14px;
  font-family: monospace;
}
</style>