# REFLEX_CHAIN_BUILDER v1.3

> **Chained Reflex Exploitation: Interactive Chain Builder**
>
> A self-contained, browser-based toolkit for constructing, visualizing, and generating proof-of-concept payloads for chained reflex exploitation attacks. Zero dependencies. Zero build step. Open the HTML file and start building exploit chains.

[![License: MIT](https://img.shields.io/badge/License-MIT-00ff41.svg)](LICENSE)
[![Pyodide](https://img.shields.io/badge/Powered%20by-Pyodide-00aaff.svg)](https://pyodide.org/)
[![Size](https://img.shields.io/badge/Size-~80KB-ffaa00.svg)]()

---

## What is Chained Reflex Exploitation?

Each software component has a **reflex** — an automatic, unthinking response to input. A Markdown parser reflexively formats code blocks. A template engine reflexively evaluates directives. A Base64 decoder reflexively decodes strings. Individually, these reflexes are benign. Chained together, they become arbitrary execution vectors.

**The Core Theorem:**

```
Given: Trigger(R) -> Reflex(R) -> Output(R)
If:    Output(Ri) in Trigger(Ri+1)
Then: Chain(R1, R2, ..., Rn) is a valid exploit path
```

This toolkit lets you build, validate, and weaponize those chains interactively.

---

## Features

| Feature | Description |
|---------|-------------|
| **40+ Reflex Nodes** | 11 categories: Format Parser, Encoder, Network, Crypto, Container, Exploit, Data, VCS, Build System, Documentation, Testing |
| **Drag & Drop Graph** | Visual node-based chain builder with SVG connection rendering |
| **Live Type Validation** | Connections turn green when `OUTPUT(Ri) in TRIGGER(Ri+1)`, red when mismatched |
| **Smart Helper** | Recommends next nodes based on output types, prioritizes exploit paths |
| **Flow Visualizer** | Horizontal flow diagram showing START -> STEP -> END with selected node highlighted |
| **Chain Path Highlighting** | Hover any node to highlight its entire upstream/downstream chain |
| **Palette Compatibility** | Hover palette nodes while a canvas node is selected to see MATCH / NO MATCH |
| **10 Preset Chains** | One-click load complete exploit chains from the research arsenal |
| **Payload Generation** | Generates working PoC payloads for all 10 original chains |
| **Chain Encyclopedia** | Detailed cards: target system, severity, CVEs, why it works, prerequisites, mitigations |
| **WASM Backend** | Pyodide-powered Python runtime for advanced chain analysis |
| **Offline Fallback** | Works without network — embedded JS fallback data when Pyodide CDN unavailable |
| **13th Hour Aesthetic** | Dark cyberpunk UI with CRT grid, neon green accents, monospace typography |

---

## Quick Start

### Option 1: Open Directly
```bash
git clone https://github.com/yourusername/reflex-chain-builder.git
cd reflex-chain-builder
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

### Option 2: GitHub Pages
Fork this repo, enable GitHub Pages in Settings -> Pages -> Source: Deploy from branch -> main -> / (root). The tool will be live at `https://yourusername.github.io/reflex-chain-builder/`.

### Option 3: Local Server
```bash
python3 -m http.server 8080
# Open http://localhost:8080
```

---

## Usage

### Building a Chain

1. **Drag** nodes from the left palette onto the canvas
2. **Connect** nodes by clicking an output port (right side), then an input port (left side)
3. **Green** = valid type chain. **Red** = type mismatch
4. Click **VALIDATE** to audit all connections
5. Click **GENERATE** to produce working payloads

### Using the Smart Helper

Select any node to see:
- **THIS NODE PRODUCES** — output type tags
- **INPUT STATUS** — which inputs are connected vs needed
- **RECOMMENDED NEXT NODES** — up to 8 suggestions, exploit nodes prioritized. Click to auto-add and auto-connect
- **CHAIN CONTEXT** — if part of a known preset, shows position, target, and severity

### Loading Presets

Click any preset button in the Inspector panel:
- Chain 1: Markdown -> Code -> Exec
- Chain 2: URL -> Base64 -> JS
- Chain 3: Timestamp -> MIME -> JSON -> Cmd
- Chain 4: Emoji -> Unicode -> ZWJ -> SSTI
- Chain 5: Shebang -> Docker -> Escape
- Chain 6: LaTeX -> TikZ -> SVG -> JS
- Chain 7: JWT -> JSON -> SQL -> Inj
- Chain 8: Unicode -> Homograph -> IDN
- Chain 9: QR -> Base64 -> Pickle -> RCE
- Chain 10: Neural -> LSB -> Stego -> SC

---

## Architecture

```
index.html
├── <style>          # 13th Hour CSS (~4KB)
├── <body>           # UI shell
│   ├── Palette      # 40 draggable nodes
│   ├── Canvas       # SVG graph + node container
│   └── Inspector    # Flow viz, helper, encyclopedia, payloads
├── <script>         # Python module (Pyodide)
└── <script>         # JS UI layer (~45KB)
    ├── Fallback data (embedded)
    ├── Pyodide init
    ├── Palette builder
    ├── Node management
    ├── Connection system
    ├── Chain highlighting
    ├── Flow visualizer
    ├── Smart helper
    ├── Encyclopedia
    └── Payload generation
```

### WASM Backend

The Python module (embedded as base64) provides:
- `get_node_defs()` — JSON of all 40 node definitions
- `get_categories()` — 11 category definitions
- `get_presets()` — 10 preset chain definitions
- `find_chains(nodes, connections)` — DFS chain discovery
- `format_chain_report(chains)` — Human-readable payload report

If Pyodide fails to load (CDN blocked, offline), the tool falls back to embedded JS copies of all data.

---

## Node Categories

| Category | Color | Count | Examples |
|----------|-------|-------|----------|
| Format Parser | #00ff41 | 14 | Markdown, JSON, YAML, LaTeX, SVG, PDF, WASM, Protobuf |
| Encoder | #00aaff | 3 | Base64, MIME, HPACK |
| Network | #ffaa00 | 6 | URL, Data URL, IDN, HTTP, DNS, Homograph |
| Crypto | #ff00ff | 4 | JWT, Hash, PEM, SSH Key |
| Container | #00ffff | 3 | Dockerfile, Compose, Kubectl |
| Exploit | #ff0044 | 8 | Template Engine, Command Injector, SQL Injector, RCE Trigger, Shellcode Builder |
| Data | #ffff00 | 5 | Timestamp, QR, Image, LSB, Stego |
| Version Control | #aa66ff | 2 | Git Diff, Patch |
| Build System | #ff8800 | 2 | Makefile, Package.json |
| Documentation | #66ffaa | 0 | (reserved) |
| Testing | #ff66aa | 0 | (reserved) |

---

## Type System

30+ data types flow through the graph:

`text` · `markdown_ast` · `code` · `command` · `javascript` · `bash` · `html` · `url` · `data_url` · `base64` · `binary` · `json_obj` · `jwt_claims` · `sql` · `latex_ast` · `svg` · `dockerfile` · `yaml` · `unicode` · `zwj` · `idn` · `qr_data` · `image_pixels` · `certificate` · `ssh_key` · `private_key` · `patch` · `k8s_manifest` · `protobuf` · `exploit` · `pickle` · `http_obj` · `stego` · `shellcode`

---

## Encyclopedia Entries

Each of the 10 preset chains has a detailed encyclopedia card:

- **Target System** — What software is vulnerable
- **Severity** — CRITICAL / HIGH / MEDIUM / LOW
- **CVE References** — Known CVEs for this chain
- **Description** — How the chain works step-by-step
- **Why It Works** — The reflexive exploitation theory
- **Prerequisites** — What conditions must exist
- **Mitigation** — How to defend against it

---

## Browser Compatibility

| Browser | Status | Notes |
|---------|--------|-------|
| Chrome 90+ | Full | WebAssembly + Pyodide supported |
| Firefox 88+ | Full | WebAssembly + Pyodide supported |
| Edge 90+ | Full | Chromium-based |
| Safari 14+ | Partial | Pyodide may be slower; fallback works |
| Offline | Degraded | Fallback data only, no payload generation |

---

## File Size Breakdown

| Component | Size |
|-----------|------|
| HTML + CSS | ~5 KB |
| JS UI Layer | ~45 KB |
| Python Module (base64) | ~21 KB |
| Encyclopedia Data | ~8 KB |
| Fallback Data | ~12 KB |
| **Total** | **~80 KB** |

---

## License

MIT License — see [LICENSE](LICENSE)

---

## Acknowledgments

- [Pyodide](https://pyodide.org/) — Python runtime for WebAssembly
- [Chained Reflex Exploitation Research](https://github.com/yourusername/reflex-chain-builder) — The theoretical foundation

---

> *"Every parser is a potential execution context. Every encoder is a potential bypass. Every format boundary is a potential privilege escalation."*
