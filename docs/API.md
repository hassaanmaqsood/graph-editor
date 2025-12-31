# API Documentation

Complete API reference for Graph Editor Web Component.

## Table of Contents

- [Configuration](#configuration)
- [Node Management](#node-management)
- [Edge Management](#edge-management)
- [Selection Management](#selection-management)
- [View Control](#view-control)
- [Graph Operations](#graph-operations)
- [Styling](#styling)
- [Events](#events)
- [Types](#types)

---

## Configuration

### `setConfig(config: Object)`

Update the editor configuration.

**Parameters:**
- `config.snapRadius` (number) - Port connection snap distance in pixels. Default: `30`
- `config.gridSize` (number) - Grid cell size in pixels. Default: `20`
- `config.showGrid` (boolean) - Show/hide background grid. Default: `true`
- `config.edgeStyle` (string) - Edge rendering style: `'bezier'`, `'straight'`, or `'step'`. Default: `'bezier'`
- `config.theme` (string) - Color theme: `'dark'` or `'light'`. Default: `'dark'`
- `config.zoomSpeed` (number) - Zoom sensitivity (0.05 - 0.3). Default: `0.1`
- `config.minZoom` (number) - Minimum zoom level. Default: `0.1`
- `config.maxZoom` (number) - Maximum zoom level. Default: `3`
- `config.zoomToMouse` (boolean) - Zoom to cursor vs canvas center. Default: `true`
- `config.enableKeyboardShortcuts` (boolean) - Enable keyboard navigation. Default: `true`

**Example:**
```javascript
editor.setConfig({
  gridSize: 25,
  showGrid: true,
  zoomToMouse: true,
  enableKeyboardShortcuts: true,
  edgeStyle: 'bezier'
});
```

---

## Node Management

### `addNode(nodeData: Object): HTMLElement`

Add a new node to the graph.

**Parameters:**
- `nodeData.id` (string, optional) - Unique identifier. Auto-generated if not provided
- `nodeData.title` (string) - Display title. Default: `'Node'`
- `nodeData.position` (Object) - Canvas position `{x, y}`. Defaults to center if not provided
- `nodeData.inputs` (Array) - Input ports. Default: `[]`
  - `id` (string) - Port identifier
  - `label` (string) - Port display label
  - `type` (string) - Port data type
- `nodeData.outputs` (Array) - Output ports. Default: `[]`
  - `id` (string) - Port identifier
  - `label` (string) - Port display label
  - `type` (string) - Port data type
- `nodeData.content` (string, optional) - HTML for display area
- `nodeData.color` (string, optional) - CSS color for ports

**Returns:** The created node HTML element

**Events:** Fires `node-added` event

**Example:**
```javascript
const node = editor.addNode({
  id: 'math-node-1',
  title: 'Add',
  position: { x: 100, y: 200 },
  inputs: [
    { id: 'a', label: 'A', type: 'number' },
    { id: 'b', label: 'B', type: 'number' }
  ],
  outputs: [
    { id: 'result', label: 'Result', type: 'number' }
  ],
  content: '<div style="text-align: center;">+</div>',
  color: '#ff6b6b'
});
```

### `removeNode(nodeId: string)`

Remove a node from the graph.

**Parameters:**
- `nodeId` (string) - The node ID to remove

**Events:** Fires `node-removed` event

**Example:**
```javascript
editor.removeNode('math-node-1');
```

### `getNodes(): Array`

Get all nodes in the graph.

**Returns:** Array of node objects with structure:
```javascript
[{
  id: string,
  element: HTMLElement,
  data: Object
}]
```

**Example:**
```javascript
const nodes = editor.getNodes();
console.log(`Total nodes: ${nodes.length}`);
```

### `updateNodeContent(nodeId: string, content: string)`

Update a node's display content.

**Parameters:**
- `nodeId` (string) - Node ID
- `content` (string) - HTML content

**Example:**
```javascript
editor.updateNodeContent('math-node-1', '<div>Result: 42</div>');
```

### `updateNodeTitle(nodeId: string, title: string)`

Update a node's title.

**Parameters:**
- `nodeId` (string) - Node ID
- `title` (string) - New title text

**Example:**
```javascript
editor.updateNodeTitle('math-node-1', 'Multiply');
```

### `getNodeData(nodeId: string): Object`

Get node data including current position.

**Parameters:**
- `nodeId` (string) - Node ID

**Returns:** Node data object or `null` if not found

**Example:**
```javascript
const data = editor.getNodeData('math-node-1');
console.log(data.position); // { x: 100, y: 200 }
```

---

## Edge Management

### `addEdge(sourceNodeId: string, targetNodeId: string, sourcePortId: string, targetPortId: string): Object|null`

Create a connection between two nodes.

**Parameters:**
- `sourceNodeId` (string) - Source node ID
- `targetNodeId` (string) - Target node ID
- `sourcePortId` (string) - Source port ID
- `targetPortId` (string) - Target port ID

**Returns:** The created edge data object or `null` if failed

**Events:** Fires `edge-added` event

**Example:**
```javascript
editor.addEdge('node-1', 'node-2', 'output', 'input');
```

### `removeEdge(edgeId: string)`

Remove an edge from the graph.

**Parameters:**
- `edgeId` (string) - The edge ID to remove

**Events:** Fires `edge-removed` event

**Example:**
```javascript
editor.removeEdge('edge-abc123');
```

### `getEdges(): Array`

Get all edges in the graph.

**Returns:** Array of edge objects with structure:
```javascript
[{
  id: string,
  sourceNode: string,
  targetNode: string,
  sourcePort: string,
  targetPort: string,
  element: SVGPathElement
}]
```

**Example:**
```javascript
const edges = editor.getEdges();
console.log(`Total connections: ${edges.length}`);
```

---

## Selection Management

### `clearSelection()`

Clear all selections (nodes and edges).

**Example:**
```javascript
editor.clearSelection();
```

### `deleteSelected()`

Delete all selected elements (nodes and/or edges).

**Example:**
```javascript
editor.deleteSelected();
```

---

## View Control

### `centerView()`

Reset pan and zoom to center with 100% zoom.

**Example:**
```javascript
editor.centerView();
```

### `fitToView()`

Fit all nodes in the viewport with appropriate zoom.

**Example:**
```javascript
editor.fitToView();
```

### `setZoom(scale: number)`

Set the zoom level.

**Parameters:**
- `scale` (number) - Zoom scale (constrained by `minZoom` and `maxZoom`)

**Events:** Fires `zoom-changed` event

**Example:**
```javascript
editor.setZoom(1.5); // 150% zoom
```

### `getCenter(): Object`

Get the canvas center coordinates.

**Returns:** Object with `{x, y}` in canvas coordinates

**Example:**
```javascript
const center = editor.getCenter();
const node = editor.addNode({
  title: 'Centered',
  position: center
});
```

---

## Graph Operations

### `exportGraph(): Object`

Export the graph to JSON format.

**Returns:** Object containing nodes and edges:
```javascript
{
  nodes: [{
    id: string,
    title: string,
    position: {x, y},
    inputs: Array,
    outputs: Array,
    content: string,
    color: string
  }],
  edges: [{
    id: string,
    source: string,
    target: string,
    sourcePort: string,
    targetPort: string
  }]
}
```

**Example:**
```javascript
const data = editor.exportGraph();
localStorage.setItem('my-graph', JSON.stringify(data));
```

### `importGraph(data: Object)`

Import a graph from JSON format.

**Parameters:**
- `data` (Object) - Graph data from `exportGraph()`

**Example:**
```javascript
const saved = localStorage.getItem('my-graph');
if (saved) {
  editor.importGraph(JSON.parse(saved));
}
```

### `clearGraph()`

Remove all nodes and edges from the graph.

**Example:**
```javascript
editor.clearGraph();
```

---

## Styling

### `setStylesheet(css: string)`

Apply a custom stylesheet as a string.

**Parameters:**
- `css` (string) - CSS stylesheet content

**Example:**
```javascript
editor.setStylesheet(`
  :host {
    --edge-color: #ff0000;
    --port-color: #00ff00;
  }
  .node .title {
    color: gold;
    text-transform: uppercase;
  }
`);
```

### `loadStylesheetFromURL(url: string): Promise<void>`

Load a stylesheet from an external URL.

**Parameters:**
- `url` (string) - URL to the CSS file

**Returns:** Promise that resolves when stylesheet is loaded

**Example:**
```javascript
try {
  await editor.loadStylesheetFromURL('themes/cyberpunk.css');
  console.log('Theme loaded!');
} catch (error) {
  console.error('Failed to load theme:', error);
}
```

---

## Events

All events are CustomEvents with details in the `event.detail` property.

### Node Events

#### `node-added`
Fired when a node is added to the graph.

**Detail:**
```javascript
{
  nodeId: string,
  node: HTMLElement,
  data: Object
}
```

#### `node-removed`
Fired when a node is removed from the graph.

**Detail:**
```javascript
{
  nodeId: string
}
```

#### `node-moved`
Fired when a node is moved.

**Detail:**
```javascript
{
  nodeId: string,
  position: {x: number, y: number}
}
```

### Edge Events

#### `edge-added`
Fired when an edge is created.

**Detail:**
```javascript
{
  edgeId: string,
  sourceNodeId: string,
  targetNodeId: string,
  sourcePortId: string,
  targetPortId: string
}
```

#### `edge-removed`
Fired when an edge is removed.

**Detail:**
```javascript
{
  edgeId: string
}
```

### Selection Events

#### `selection-changed`
Fired when selection changes (including when cleared).

**Detail:**
```javascript
// When single node selected
{
  selected: HTMLElement,
  type: 'node',
  nodeId: string
}

// When single edge selected
{
  selected: SVGPathElement,
  type: 'edge',
  edgeId: string
}

// When multiple nodes selected
{
  selected: 'multiple',
  count: number
}

// When selection is cleared
{
  selected: null,
  count: 0
}
```

### View Events

#### `zoom-changed`
Fired when zoom level changes.

**Detail:**
```javascript
{
  scale: number
}
```

#### `editor-ready`
Fired when the editor is initialized.

**Detail:**
```javascript
this // The editor instance
```

### Event Example

```javascript
editor.addEventListener('node-added', (e) => {
  console.log('Node added:', e.detail.nodeId);
  console.log('Position:', e.detail.data.position);
});

editor.addEventListener('selection-changed', (e) => {
  const { selected, type, count } = e.detail;
  
  if (selected === 'multiple') {
    console.log(`Multiple selection: ${count} nodes selected`);
  } else if (selected === null) {
    console.log('Selection cleared');
  } else if (type === 'node') {
    console.log('Node selected:', e.detail.nodeId);
  } else if (type === 'edge') {
    console.log('Edge selected:', e.detail.edgeId);
  }
});

// Example: Update UI based on selection
editor.addEventListener('selection-changed', (e) => {
  const deleteBtn = document.getElementById('delete-btn');
  const propertiesPanel = document.getElementById('properties');
  
  if (e.detail.selected === null) {
    // Nothing selected
    deleteBtn.disabled = true;
    propertiesPanel.innerHTML = '<p>No selection</p>';
  } else if (e.detail.selected === 'multiple') {
    // Multiple nodes
    deleteBtn.disabled = false;
    propertiesPanel.innerHTML = `<p>${e.detail.count} nodes selected</p>`;
  } else {
    // Single element
    deleteBtn.disabled = false;
    if (e.detail.type === 'node') {
      const nodeData = editor.getNodeData(e.detail.nodeId);
      propertiesPanel.innerHTML = `<h3>${nodeData.title}</h3>`;
    } else {
      propertiesPanel.innerHTML = '<p>Edge selected</p>';
    }
  }
});
```

---

## Types

### NodeData

```typescript
interface NodeData {
  id?: string;
  title?: string;
  position?: {x: number, y: number};
  inputs?: Port[];
  outputs?: Port[];
  content?: string;
  color?: string;
}
```

### Port

```typescript
interface Port {
  id: string;
  label: string;
  type: string;
}
```

### GraphData

```typescript
interface GraphData {
  nodes: NodeData[];
  edges: EdgeData[];
}
```

### EdgeData

```typescript
interface EdgeData {
  id?: string;
  source: string;
  target: string;
  sourcePort: string;
  targetPort: string;
}
```

### Config

```typescript
interface Config {
  snapRadius?: number;
  gridSize?: number;
  showGrid?: boolean;
  edgeStyle?: 'bezier' | 'straight' | 'step';
  theme?: 'dark' | 'light';
  zoomSpeed?: number;
  minZoom?: number;
  maxZoom?: number;
  zoomToMouse?: boolean;
  enableKeyboardShortcuts?: boolean;
}
```
