# Architecture

## Overview

REFLEX_CHAIN_BUILDER is a single-file application. All code — HTML, CSS, JavaScript, and Python — lives in `index.html`. No build step. No dependencies (except Pyodide CDN).

## Data Flow

```
User Interaction
      |
      v
+-------------+
|  JS UI Layer |  <-- Drag, drop, connect, select
+-------------+
      |
      |---> Canvas rendering (DOM + SVG)
      |---> Inspector updates
      |---> Chain validation
      |
      v
+-------------+
| Pyodide WASM |  <-- Chain finding, payload generation
|   (Python)   |
+-------------+
      |
      v
+-------------+
|   Fallback   |  <-- Embedded JS data when WASM unavailable
+-------------+
```

## Components

### HTML Structure

```html
<div class="topbar">       <!-- Status, buttons -->
<div class="main">
  <div class="palette">    <!-- Draggable node list -->
  <div class="canvas-wrap"> <!-- SVG graph + nodes -->
  <div class="inspector">  <!-- Flow, helper, encyclopedia, payloads -->
<div class="console">      <!-- System log -->
```

### CSS Architecture

CSS custom properties (variables) define the 13th Hour color scheme:
- `--bg`: #0a0a0a (main background)
- `--border`: #00ff41 (primary accent)
- `--text`: #00ff41 (primary text)
- `--danger`: #ff0044 (errors)
- `--warning`: #ffaa00 (highlights)
- `--info`: #00aaff (info)

### JavaScript Modules

| Function | Purpose |
|----------|---------|
| `initPyodide()` | Load Pyodide, fallback to JS data |
| `buildPalette()` | Render categorized node list |
| `buildPresets()` | Render preset chain buttons |
| `buildEncyclopedia()` | Render encyclopedia cards |
| `createNode()` | Add node to canvas |
| `renderNode()` | Create DOM element for node |
| `handlePortClick()` | Start/complete connections |
| `redrawConnections()` | Render SVG bezier curves |
| `highlightChainPath()` | Amber highlight on hover |
| `updateFlowViz()` | Horizontal flow diagram |
| `updateHelper()` | Smart recommendations |
| `validateAll()` | Type-check all connections |
| `generatePayload()` | Delegate to Python chain finder |
| `loadPreset()` | Auto-load preset chains |

### Python Module

Embedded as base64 and decoded at runtime:

```python
NODE_DEFS      # 40 node definitions
CATEGORIES     # 11 category definitions
PRESETS        # 10 preset chains
PAYLOADS       # 10 payload generators

def get_node_defs()       # -> JSON
def get_categories()      # -> JSON
def get_presets()         # -> JSON
def find_chains()         # DFS chain discovery
def format_chain_report() # Human-readable output
```

## Type System

The graph is validated against a type system with 30+ types:

```
text -> markdown_parser -> markdown_ast -> code_extractor -> code
                                                          -> template_engine -> command
                                                                             -> javascript
```

Connection validity: `fromType === toType`

## Chain Discovery

Python DFS from nodes with no incoming connections:

```python
def dfs(nid, path, cpath):
    nxt = [c for c in conns if c["from"] == nid]
    if not nxt and len(path) > 1:
        chains.append({"nodes": path, "connections": cpath})
    for c in nxt:
        dfs(c["to"], path + [n], cpath + [c])
```

## Security Considerations

- **No server required**: Runs entirely client-side
- **No data exfiltration**: No network requests except Pyodide CDN
- **Payloads are static**: Generated from hardcoded templates, not executed
- **Sandboxed**: Pyodide runs in browser WASM sandbox
