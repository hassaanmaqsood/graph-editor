# Graph Editor - Custom Styling Guide

The Graph Editor supports extensive customization through CSS Custom Properties (CSS Variables) and custom stylesheets. You can style the editor without touching the component code!

## 📋 Table of Contents

1. [Quick Start](#quick-start)
2. [Stylesheet Methods](#stylesheet-methods)
3. [All CSS Variables](#all-css-variables)
4. [Styling Methods](#styling-methods)
5. [Theme Examples](#theme-examples)
6. [Advanced Techniques](#advanced-techniques)

---

## Quick Start

### Method 1: Inline Styles (HTML)

```html
<graph-editor id="editor" style="
  --background-color: #1a1a2e;
  --edge-color: #ff6b6b;
  --port-color: #4ecdc4;
  --node-bg: #16213e;
"></graph-editor>
```

### Method 2: CSS Stylesheet

```css
#editor {
  --background-color: #1a1a2e;
  --edge-color: #ff6b6b;
  --port-color: #4ecdc4;
  --node-bg: #16213e;
}
```

### Method 3: JavaScript

```javascript
const editor = document.getElementById('editor');
editor.style.setProperty('--edge-color', '#ff6b6b');
editor.style.setProperty('--port-size', '1.5rem');
editor.style.setProperty('--node-border-radius', '20px');
```

---

## Stylesheet Methods

The Graph Editor supports four different methods to apply custom stylesheets, giving you complete flexibility in how you organize and apply your styles.

### Method 1: Inline Style Slot (HTML)

Embed CSS directly in your HTML using the `slot="stylesheet"` attribute:

```html
<graph-editor id="editor">
  <style slot="stylesheet">
    :host {
      --edge-color: #ff6b6b;
      --port-color: #4ecdc4;
      --node-bg: #2a2a3e;
    }
    
    .node .title {
      color: #ffd700;
      font-weight: bold;
    }
    
    .edge {
      filter: drop-shadow(0 0 4px var(--edge-color));
    }
  </style>
</graph-editor>
```

**Pros:** 
- Self-contained
- No external files needed
- Great for component-specific styles

**Cons:**
- Not reusable across multiple editors
- Can clutter HTML

### Method 2: External CSS File Slot (HTML + File)

Link to an external CSS file:

```html
<graph-editor id="editor">
  <link slot="stylesheet" href="themes/cyberpunk.css">
</graph-editor>
```

**Pros:**
- Reusable across multiple editors
- Clean HTML
- Easy to swap themes
- Can be cached by browser

**Cons:**
- Requires file management
- Additional HTTP request

### Method 3: JavaScript setStylesheet() Method

Apply CSS programmatically as a string:

```javascript
const editor = document.getElementById('editor');

const customCSS = `
  :host {
    --background-color: #0d1117;
    --edge-color: #58a6ff;
    --port-color: #f78166;
  }
  
  .node .title {
    color: #58a6ff;
    text-transform: uppercase;
  }
  
  .port:hover {
    animation: pulse 0.5s infinite;
  }
  
  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.2); }
  }
`;

editor.setStylesheet(customCSS);
```

**Pros:**
- Dynamic theme generation
- Can use template literals with variables
- Perfect for user customization features
- Can be stored in database/localStorage

**Cons:**
- CSS as strings can be less maintainable
- No syntax highlighting in editor

### Method 4: Load from URL (JavaScript + File)

Load an external stylesheet programmatically:

```javascript
const editor = document.getElementById('editor');

// Returns a promise
await editor.loadStylesheetFromURL('themes/neon.css');

// Or with error handling
try {
  await editor.loadStylesheetFromURL('themes/custom-theme.css');
  console.log('Theme loaded successfully');
} catch (error) {
  console.error('Failed to load theme:', error);
}
```

**Pros:**
- Reusable CSS files
- Can load themes dynamically based on user choice
- Promise-based for async loading
- Easy theme switching

**Cons:**
- Requires network request
- Need to handle loading errors

### Combining Methods

You can use multiple methods together. For example:

```html
<graph-editor id="editor">
  <!-- Base theme from file -->
  <link slot="stylesheet" href="themes/base.css">
</graph-editor>

<script>
  // Add customizations via JavaScript
  const editor = document.getElementById('editor');
  
  editor.setStylesheet(`
    /* Override specific colors */
    :host {
      --edge-color: ${userPreferences.edgeColor};
      --port-size: ${userPreferences.portSize}rem;
    }
  `);
</script>
```

### Best Practices for Stylesheet Methods

1. **Use External Files for Themes**: Keep complete themes in separate CSS files
2. **Use setStylesheet() for User Customization**: Let users pick colors dynamically
3. **Use Inline Styles for Quick Prototypes**: Fast testing without extra files
4. **Use loadStylesheetFromURL() for Theme Switchers**: Easy dynamic theme changes

---

## Creating Custom Stylesheet Files

When creating external stylesheet files, follow this structure:

```css
/* my-theme.css */

/* 1. Define CSS variables */
:host {
  --background-color: #1a1a2e;
  --node-bg: #16213e;
  --edge-color: #ff6b6b;
  /* ... more variables */
}

/* 2. Customize component internals */
.node {
  /* Your styles */
}

.node .title {
  /* Your styles */
}

.edge {
  /* Your styles */
}

.port {
  /* Your styles */
}

/* 3. Add animations/effects */
@keyframes myAnimation {
  /* ... */
}

.node:hover {
  animation: myAnimation 1s ease;
}
```

### Available Selectors

You can target these internal elements:

- `.graph-editor` - Main container
- `.canvas` - The panning/zooming canvas
- `.nodes-layer` - Container for all nodes
- `.edges-layer` - SVG container for edges
- `.node` - Individual node
- `.node .block` - Node main section (draggable area)
- `.node .title` - Node title text
- `.node .display` - Node display/preview area
- `.port` - Connection port
- `.port.input` - Input ports
- `.port.output` - Output ports
- `.port.hover` - Port being hovered during connection
- `.ports` - Port container
- `.ports.inputs` - Input ports container
- `.ports.outputs` - Output ports container
- `.edge` - Connection line (SVG path)
- `.edge.temporary` - Temporary edge while dragging
- `.edge.selected` - Selected edge
- `.node.selected` - Selected node

---

## All CSS Variables

### Color Variables

| Variable | Default (Dark) | Default (Light) | Description |
|----------|---------------|-----------------|-------------|
| `--background-color` | `#222` | `#fff` | Canvas background |
| `--grid-color` | `#333` | `#e0e0e0` | Grid line color |
| `--node-bg` | `#111` | `#f5f5f5` | Node background |
| `--node-border` | `#333` | `#ddd` | Node border (unused by default) |
| `--node-text` | `#f5f6f8` | `#000` | Node text color |
| `--edge-color` | `#474bff` | `#474bff` | Connection line color |
| `--edge-hover-color` | `#66ff66` | `#66ff66` | Port hover indicator |
| `--selected-color` | `#ff6b6b` | `#ff6b6b` | Selected element color |
| `--port-color` | `#474bff` | `#474bff` | Port dot color |
| `--edge-temporary-color` | `#666` | `#666` | Temporary edge while dragging |

### Size Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--port-size` | `1rem` | Port circle diameter |
| `--edge-width` | `2` | Edge stroke width (px) |
| `--selected-width` | `3` | Selected edge stroke width (px) |
| `--node-min-width` | `200px` | Minimum node width |
| `--node-border-width` | `2px` | Node border thickness |

### Layout Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--node-border-radius` | `15px` | Node corner roundness |
| `--node-padding` | `1rem 0.5rem` | Node internal padding |
| `--node-gap` | `1rem` | Gap between node parts |
| `--port-border-radius` | `50%` | Port shape (50% = circle) |
| `--input-port-offset` | `-50%` | Input port X offset |
| `--output-port-offset` | `50%` | Output port X offset |

### Typography Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--node-title-size` | `1rem` | Node title font size |
| `--node-title-weight` | `500` | Node title font weight |
| `--node-title-max-width` | `15ch` | Max title width (characters) |

### Effect Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--node-shadow` | `0 2px 8px rgba(0,0,0,0.3)` | Node drop shadow |
| `--port-shadow` | `0 0 8px currentColor` | Port glow on hover |
| `--port-hover-scale` | `1.2` | Port scale on hover |
| `--edge-temporary-dash` | `5, 5` | Dash pattern for temp edges |

### Transition Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `--transition-duration` | `0.2s` | Animation duration |
| `--transition-timing` | `ease` | Animation timing function |

---

## Styling Methods

### 1. Global Styles (All Editors)

```css
graph-editor {
  --edge-color: #00ffff;
  --port-color: #ff00ff;
}
```

### 2. Specific Editor (By ID)

```css
#my-editor {
  --background-color: #1a1a1a;
  --node-bg: #2a2a2a;
}
```

### 3. Multiple Editors (By Class)

```html
<graph-editor class="theme-dark"></graph-editor>
<graph-editor class="theme-light"></graph-editor>
```

```css
.theme-dark {
  --background-color: #000;
  --node-bg: #111;
}

.theme-light {
  --background-color: #fff;
  --node-bg: #f5f5f5;
}
```

### 4. Dynamic Styling (JavaScript)

```javascript
function setTheme(editor, theme) {
  const themes = {
    dark: {
      '--background-color': '#222',
      '--node-bg': '#111',
      '--edge-color': '#474bff'
    },
    light: {
      '--background-color': '#fff',
      '--node-bg': '#f5f5f5',
      '--edge-color': '#2196f3'
    },
    cyberpunk: {
      '--background-color': '#0d0221',
      '--node-bg': '#1a0b2e',
      '--edge-color': '#ff006e',
      '--port-color': '#00fff5'
    }
  };
  
  Object.entries(themes[theme]).forEach(([prop, value]) => {
    editor.style.setProperty(prop, value);
  });
}

// Usage
setTheme(document.getElementById('editor'), 'cyberpunk');
```

---

## Theme Examples

### Cyberpunk Theme

```css
graph-editor.cyberpunk {
  --background-color: #0d0221;
  --grid-color: #ff006e;
  --node-bg: #1a0b2e;
  --node-text: #00fff5;
  --node-border-radius: 8px;
  --node-shadow: 0 0 20px rgba(255, 0, 110, 0.3);
  --edge-color: #ff006e;
  --edge-width: 3;
  --port-color: #00fff5;
  --port-size: 1.2rem;
  --selected-color: #ffbe0b;
}
```

### Minimal Light Theme

```css
graph-editor.minimal {
  --background-color: #fafafa;
  --grid-color: #e0e0e0;
  --node-bg: #ffffff;
  --node-text: #333;
  --node-border-radius: 12px;
  --node-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  --edge-color: #666;
  --edge-width: 2;
  --port-color: #2196f3;
  --port-size: 0.8rem;
  --selected-color: #ff9800;
}
```

### Neon Glow Theme

```css
graph-editor.neon {
  --background-color: #000000;
  --grid-color: #1a1a1a;
  --node-bg: #0a0a0a;
  --node-text: #fff;
  --node-border-radius: 20px;
  --node-shadow: 0 0 30px rgba(0, 255, 255, 0.5);
  --edge-color: #00ffff;
  --edge-width: 3;
  --edge-hover-color: #ff00ff;
  --port-color: #00ffff;
  --port-size: 1.5rem;
  --port-hover-scale: 1.5;
  --port-shadow: 0 0 15px currentColor;
  --selected-color: #ff00ff;
  --selected-width: 4;
}
```

### Blueprint Theme

```css
graph-editor.blueprint {
  --background-color: #0c4a6e;
  --grid-color: #075985;
  --node-bg: rgba(7, 89, 133, 0.3);
  --node-text: #ffffff;
  --node-border-radius: 2px;
  --node-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
  --edge-color: #ffffff;
  --edge-width: 2;
  --port-color: #ffffff;
  --port-size: 0.6rem;
  --selected-color: #fbbf24;
}
```

### Pastel Theme

```css
graph-editor.pastel {
  --background-color: #fff5f8;
  --grid-color: #ffd6e0;
  --node-bg: #ffffff;
  --node-text: #5a5a5a;
  --node-border-radius: 20px;
  --node-shadow: 0 4px 12px rgba(255, 182, 193, 0.3);
  --edge-color: #ffb6c1;
  --edge-width: 3;
  --port-color: #98d8c8;
  --port-size: 1rem;
  --selected-color: #f7b7d0;
}
```

---

## Advanced Techniques

### Theme Switcher

```javascript
class ThemeManager {
  constructor(editor) {
    this.editor = editor;
    this.themes = {
      dark: { /* ... */ },
      light: { /* ... */ },
      cyberpunk: { /* ... */ }
    };
  }
  
  apply(themeName) {
    const theme = this.themes[themeName];
    if (!theme) return;
    
    Object.entries(theme).forEach(([prop, value]) => {
      this.editor.style.setProperty(prop, value);
    });
    
    // Redraw grid with new color
    if (this.editor.config.showGrid) {
      this.editor.drawGrid();
    }
  }
  
  addTheme(name, properties) {
    this.themes[name] = properties;
  }
}

// Usage
const themeManager = new ThemeManager(editor);
themeManager.apply('cyberpunk');
```

### Animated Theme Transitions

```css
graph-editor {
  transition: all 0.3s ease;
}

/* Smooth color transitions */
graph-editor * {
  transition: 
    background-color 0.3s ease,
    border-color 0.3s ease,
    color 0.3s ease;
}
```

### Conditional Styling Based on State

```javascript
editor.addEventListener('node-added', () => {
  if (editor.getNodes().length > 10) {
    // Reduce visual complexity for many nodes
    editor.style.setProperty('--node-shadow', 'none');
    editor.style.setProperty('--port-size', '0.8rem');
  }
});
```

### Responsive Themes

```css
/* Desktop */
@media (min-width: 1024px) {
  graph-editor {
    --port-size: 1rem;
    --edge-width: 2;
  }
}

/* Tablet */
@media (max-width: 1023px) {
  graph-editor {
    --port-size: 1.2rem;
    --edge-width: 3;
  }
}

/* Mobile */
@media (max-width: 768px) {
  graph-editor {
    --port-size: 1.5rem;
    --edge-width: 4;
    --node-min-width: 150px;
  }
}
```

### Per-Node Custom Colors

While CSS variables apply globally, you can set per-node colors via the API:

```javascript
editor.addNode({
  title: 'Special Node',
  color: '#ff0000', // This sets --port-color for this node's ports
  // ... other properties
});
```

### CSS Classes for Node Types

You can add custom classes to nodes and style them differently:

```javascript
const node = editor.addNode({ /* ... */ });
node.classList.add('input-node');
```

```css
graph-editor .node.input-node .block {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

---

## Tips & Best Practices

1. **Use CSS Variables**: They provide live updates without component reload
2. **Test Contrast**: Ensure text is readable on backgrounds
3. **Consider Accessibility**: Use sufficient color contrast ratios
4. **Mobile First**: Larger touch targets on smaller screens
5. **Performance**: Avoid complex shadows/filters with many nodes
6. **Theme Consistency**: Keep related colors harmonious
7. **Test Edge Visibility**: Ensure edges are visible against backgrounds

---

## Browser Compatibility

CSS Custom Properties are supported in:
- Chrome/Edge 49+
- Firefox 31+
- Safari 9.1+
- All modern mobile browsers

---

## Need Help?

Check out `styling-demo.html` for live examples of all these themes!
