# Usage Guide

## Basic Workflow

### 1. Start with a Preset

Click any preset button in the Inspector panel. This auto-loads a complete chain with valid connections.

### 2. Build Custom Chains

Drag nodes from the palette onto the canvas. Connect them by clicking output ports (right side) then input ports (left side).

**Valid connection**: Green line. Type match: `OUTPUT(Ri) in TRIGGER(Ri+1)`.
**Invalid connection**: Red line. Type mismatch.

### 3. Use the Smart Helper

Select any node to see:
- What outputs it produces
- Which inputs are connected vs needed
- Recommended next nodes (exploit nodes prioritized)
- One-click add with auto-connect

### 4. Validate

Click **VALIDATE** to audit all connections. The status bar shows valid/invalid counts.

### 5. Generate Payloads

Click **GENERATE** to traverse all complete chains and emit working proof-of-concept payloads.

## Advanced Features

### Chain Path Highlighting
Hover over any canvas node to highlight its entire upstream/downstream chain in amber.

### Palette Compatibility Preview
Hover over palette nodes while a canvas node is selected. Compatible nodes glow green with "MATCH" badge. Incompatible ones dim with "NO MATCH".

### Flow Visualizer
The Inspector shows a horizontal flow diagram: START -> STEP -> END. The selected node is highlighted.

### Encyclopedia
Click **ENCYCLOPEDIA** to toggle detailed cards for each preset chain. Each card includes:
- Target system
- Severity (CRITICAL/HIGH/MEDIUM/LOW)
- CVE references
- Full description
- Why it works (reflexive exploitation theory)
- Prerequisites
- Mitigation strategies

## Keyboard Shortcuts

| Action | Shortcut |
|--------|----------|
| Delete selected node | Delete key |
| Clear canvas | Click CLEAR button |
| Validate chains | Click VALIDATE button |
| Generate payloads | Click GENERATE button |

## Troubleshooting

### Blank Page
- Check browser console (F12) for errors
- Ensure JavaScript is enabled
- Try opening via local server: `python3 -m http.server 8080`

### Pyodide Fails to Load
- The tool will automatically fall back to embedded JS data
- Payload generation will be unavailable
- All other features (building, validating, visualizing) still work

### Nodes Not Draggable
- Ensure you are clicking the node body, not a port
- Ports are for connections, not dragging
