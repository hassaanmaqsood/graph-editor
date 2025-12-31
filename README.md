# Graph Editor Web Component

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![No Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)](package.json)
[![Bundle Size](https://img.shields.io/badge/size-~30KB-blue.svg)](graph-editor-unified.js)

A lightweight, powerful, and fully customizable node-based graph editor built with vanilla JavaScript Web Components. Zero dependencies, complete control.

![Graph Editor Demo](https://via.placeholder.com/800x400/222/fff?text=Graph+Editor+Demo)

## ✨ Features

- 🚀 **Zero Dependencies** - Pure vanilla JavaScript
- 🎨 **Fully Customizable** - 40+ CSS variables, custom stylesheets, theme support
- 📱 **Touch Optimized** - Works seamlessly on mobile and tablets
- ⌨️ **Keyboard Navigation** - Comprehensive shortcuts for power users
- 🔌 **Event-Driven** - Rich event system for complete control
- 🎯 **Multi-Selection** - Select and manipulate multiple nodes at once
- 📊 **Import/Export** - JSON-based graph serialization
- 🔍 **Smart Zoom** - Zoom to mouse cursor or canvas center
- 📐 **Grid & Snapping** - Optional grid with customizable snap radius
- 🎭 **Multiple Edge Styles** - Bezier curves, straight lines, or step connections
- 💾 **15KB minified** - Tiny footprint, huge capabilities

## 🚀 Quick Start

### Installation

**Option 1: Direct Download**
```bash
# Download the component
curl -O https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/graph-editor-unified.js
```

**Option 2: CDN (via jsDelivr)**
```html
<script src="https://cdn.jsdelivr.net/gh/hassaanmaqsood/graph-editor@main/graph-editor-unified.js"></script>
```

**Option 3: npm (coming soon)**
```bash
npm install @hassaanmaqsood/graph-editor
```

### Basic Usage

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    graph-editor {
      width: 100vw;
      height: 100vh;
      display: block;
    }
  </style>
</head>
<body>
  <graph-editor id="editor"></graph-editor>
  
  <script src="https://cdn.jsdelivr.net/gh/hassaanmaqsood/graph-editor@main/graph-editor-unified.js"></script>
  <script>
    const editor = document.getElementById('editor');
    
    // Configure editor
    editor.setConfig({
      gridSize: 20,
      showGrid: true,
      zoomToMouse: true,
      enableKeyboardShortcuts: true
    });
    
    // Add a node
    editor.addNode({
      title: 'Start Node',
      position: { x: 100, y: 100 },
      inputs: [{ id: 'in', label: 'Input', type: 'any' }],
      outputs: [{ id: 'out', label: 'Output', type: 'any' }]
    });
  </script>
</body>
</html>
```

## 📖 Documentation

### Creating Nodes

```javascript
const node = editor.addNode({
  id: 'node-1',              // Optional: auto-generated if not provided
  title: 'Math Operation',
  position: { x: 100, y: 200 },
  inputs: [
    { id: 'a', label: 'A', type: 'number' },
    { id: 'b', label: 'B', type: 'number' }
  ],
  outputs: [
    { id: 'result', label: 'Result', type: 'number' }
  ],
  content: '<div>Custom HTML</div>',  // Optional
  color: '#ff6b6b'                    // Optional
});
```

### Creating Connections

```javascript
// Connect nodes by their IDs and port IDs
editor.addEdge('node-1', 'node-2', 'output-port', 'input-port');
```

### Configuration Options

```javascript
editor.setConfig({
  snapRadius: 30,                    // Port connection snap distance
  gridSize: 20,                      // Grid cell size in pixels
  showGrid: true,                    // Show/hide background grid
  edgeStyle: 'bezier',               // 'bezier' | 'straight' | 'step'
  theme: 'dark',                     // 'dark' | 'light'
  zoomSpeed: 0.1,                    // Zoom sensitivity (0.05 - 0.3)
  minZoom: 0.1,                      // Minimum zoom level (10%)
  maxZoom: 3,                        // Maximum zoom level (300%)
  zoomToMouse: true,                 // Zoom to cursor vs canvas center
  enableKeyboardShortcuts: true      // Enable keyboard navigation
});
```

### Keyboard Shortcuts

| Action | Shortcut | Description |
|--------|----------|-------------|
| Delete | `Del` / `Backspace` | Delete selected elements |
| Pan Canvas | `←` `↑` `↓` `→` | Pan the canvas |
| Move Nodes | `←` `↑` `↓` `→` | Move selected nodes |
| Fast Move | `Shift` + Arrows | 2.5x faster panning/movement |
| Zoom In | `+` / `=` | Increase zoom |
| Zoom Out | `-` / `_` | Decrease zoom |
| Reset Zoom | `0` | Reset to 100% |
| Fit View | `F` | Fit all nodes in viewport |
| Select All | `Ctrl/Cmd` + `A` | Select all nodes |
| Multi-Select | `Shift` + Click | Add to selection |
| Toggle Select | `Ctrl/Cmd` + Click | Toggle node selection |
| Tab Navigation | `Tab` | Cycle through nodes |
| Reverse Tab | `Shift` + `Tab` | Cycle backwards |
| Clear Selection | `Esc` | Deselect all |

### Events

```javascript
// Node events
editor.addEventListener('node-added', (e) => {
  console.log('Node added:', e.detail.nodeId);
});

editor.addEventListener('node-removed', (e) => {
  console.log('Node removed:', e.detail.nodeId);
});

editor.addEventListener('node-moved', (e) => {
  console.log('Node moved:', e.detail.position);
});

// Edge events
editor.addEventListener('edge-added', (e) => {
  console.log('Edge created:', e.detail);
});

editor.addEventListener('edge-removed', (e) => {
  console.log('Edge removed:', e.detail.edgeId);
});

// Selection events
editor.addEventListener('selection-changed', (e) => {
  console.log('Selection changed:', e.detail);
});

// View events
editor.addEventListener('zoom-changed', (e) => {
  console.log('Zoom level:', e.detail.scale);
});
```

### Import/Export

```javascript
// Export graph to JSON
const data = editor.exportGraph();
localStorage.setItem('my-graph', JSON.stringify(data));

// Import graph from JSON
const saved = localStorage.getItem('my-graph');
if (saved) {
  editor.importGraph(JSON.parse(saved));
}
```

### View Control

```javascript
// Center view
editor.centerView();

// Fit all nodes in viewport
editor.fitToView();

// Set zoom level
editor.setZoom(1.5); // 150%

// Get canvas center coordinates
const center = editor.getCenter();
```

## 🎨 Styling & Themes

### CSS Variables

Customize the editor appearance with CSS custom properties:

```css
graph-editor {
  /* Colors */
  --background-color: #222;
  --grid-color: #333;
  --node-bg: #111;
  --node-text: #f5f6f8;
  --edge-color: #474bff;
  --port-color: #474bff;
  --selected-color: #ff6b6b;
  
  /* Sizes */
  --edge-width: 2;
  --port-size: 1rem;
  --node-min-width: 200px;
  --node-border-radius: 15px;
  
  /* Effects */
  --node-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  --port-hover-scale: 1.2;
  --transition-duration: 0.2s;
}
```

### Method 1: Inline Stylesheet

```html
<graph-editor id="editor">
  <style slot="stylesheet">
    :host {
      --edge-color: #ff6b6b;
      --port-color: #4ecdc4;
    }
    .node .title {
      color: gold;
      font-weight: bold;
    }
  </style>
</graph-editor>
```

### Method 2: External CSS File

```html
<graph-editor id="editor">
  <link slot="stylesheet" href="https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/cyberpunk.css">
</graph-editor>
```

### Method 3: JavaScript

```javascript
// Apply CSS as string
editor.setStylesheet(`
  :host {
    --edge-color: #00ffff;
    --port-color: #ff00ff;
  }
`);

// Load from URL
await editor.loadStylesheetFromURL('https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/neon.css');
```

### Pre-made Themes

Check out the `/themes` directory for ready-to-use themes:
- [Cyberpunk](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/cyberpunk.css) - Hot pink & cyan with glow effects
- [Minimal Light](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/minimal-light.css) - Clean professional light theme
- [Neon](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/neon.css) - Full cyberpunk glow with animations
- [Blueprint](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/themes/blueprint.css) - Technical drawing style

## 📦 API Reference

### Methods

| Method | Description |
|--------|-------------|
| `setConfig(config)` | Update editor configuration |
| `addNode(nodeData)` | Add a new node |
| `removeNode(nodeId)` | Remove a node |
| `addEdge(source, target, sourcePort, targetPort)` | Create connection |
| `removeEdge(edgeId)` | Remove edge |
| `getNodes()` | Get all nodes |
| `getEdges()` | Get all edges |
| `exportGraph()` | Export to JSON |
| `importGraph(data)` | Import from JSON |
| `clearGraph()` | Remove all nodes and edges |
| `centerView()` | Reset view to center |
| `fitToView()` | Fit all nodes in viewport |
| `setZoom(scale)` | Set zoom level |
| `setStylesheet(css)` | Apply custom CSS |
| `loadStylesheetFromURL(url)` | Load external stylesheet |

[Full API Documentation](https://github.com/hassaanmaqsood/graph-editor/blob/main/docs/API.md)

## 🎯 Examples

### Simple Flow Editor

```javascript
const editor = document.getElementById('editor');

// Create a simple 3-node flow
const start = editor.addNode({
  title: 'Start',
  position: { x: 100, y: 200 },
  outputs: [{ id: 'out', label: 'Next', type: 'flow' }],
  color: '#4ecdc4'
});

const process = editor.addNode({
  title: 'Process',
  position: { x: 400, y: 200 },
  inputs: [{ id: 'in', label: 'In', type: 'flow' }],
  outputs: [{ id: 'out', label: 'Out', type: 'flow' }],
  color: '#ff6b6b'
});

const end = editor.addNode({
  title: 'End',
  position: { x: 700, y: 200 },
  inputs: [{ id: 'in', label: 'In', type: 'flow' }],
  color: '#9b59b6'
});

// Connect them
editor.addEdge(start.dataset.nodeId, process.dataset.nodeId, 'out', 'in');
editor.addEdge(process.dataset.nodeId, end.dataset.nodeId, 'out', 'in');
```

### Live Demos

- [Basic Usage](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/demo-unified.html)
- [Keyboard & Zoom Controls](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/keyboard-zoom-demo.html)
- [Custom Styling](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/styling-demo.html)
- [Stylesheet Methods](https://raw.githubusercontent.com/hassaanmaqsood/graph-editor/main/stylesheet-methods-demo.html)

## 🏗️ Project Structure

```
graph-editor/
├── graph-editor-unified.js      # Main component (~30KB)
├── themes/                      # Pre-made themes
│   ├── cyberpunk.css
│   ├── minimal-light.css
│   ├── neon.css
│   └── blueprint.css
├── demos/                       # Example implementations
│   ├── demo-unified.html
│   ├── keyboard-zoom-demo.html
│   ├── styling-demo.html
│   └── stylesheet-methods-demo.html
├── docs/                        # Documentation
│   ├── API.md
│   ├── STYLING.md
│   └── EXAMPLES.md
├── package.json
├── LICENSE
└── README.md
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built as a lightweight alternative to Rete.js and React Flow
- Inspired by node-based editors in Unreal Engine, Blender, and Houdini
- No dependencies - pure vanilla JavaScript and Web Components

## 📊 Comparison

| Feature | Graph Editor | Rete.js | React Flow |
|---------|--------------|---------|------------|
| Bundle Size | ~30KB | ~500KB+ | ~200KB+ |
| Dependencies | 0 | Many | React |
| Framework | Vanilla | Vanilla | React Only |
| Touch Support | ✅ Native | ⚠️ Partial | ✅ Good |
| Keyboard Nav | ✅ Full | ❌ No | ⚠️ Limited |
| Customization | ✅ Full CSS | ⚠️ Limited | ⚠️ Theme-based |
| Learning Curve | Low | Medium | Medium |
| License | MIT | MIT | MIT |

## 🔮 Roadmap

- [ ] TypeScript definitions
- [ ] Vue/React/Svelte wrappers
- [ ] Minimap component
- [ ] Group nodes
- [ ] Copy/paste support
- [ ] Alignment guides
- [ ] Auto-layout algorithms
- [ ] Plugin system

## 💬 Community

- [GitHub Issues](https://github.com/hassaanmaqsood/graph-editor/issues) - Bug reports and feature requests
- [GitHub Discussions](https://github.com/hassaanmaqsood/graph-editor/discussions) - Questions and community chat

## 🌟 Star History

[![Star History Chart](https://api.star-history.com/svg?repos=hassaanmaqsood/graph-editor&type=Date)](https://star-history.com/#hassaanmaqsood/graph-editor&Date)

---

**Built with ❤️ by [Hassaaan](https://github.com/hassaanmaqsood)**

[⬆ Back to top](#graph-editor-web-component)
