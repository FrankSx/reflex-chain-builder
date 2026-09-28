# Contributing to REFLEX_CHAIN_BUILDER

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/reflex-chain-builder.git`
3. Open `index.html` in your browser
4. Make changes and test

## Adding New Nodes

Nodes are defined in the Python module (embedded as base64 in the JS) and mirrored in the JS fallback data.

### Python Definition
```python
"node_id": {
    "name": "Display Name",
    "cat": "category_key",
    "icon": "AB",
    "desc": "What this node does.",
    "inputs": ["type_a", "type_b"],
    "outputs": ["type_c"]
}
```

### JS Fallback Mirror
```javascript
"node_id": {
    "name": "Display Name", "cat": "category_key", "icon": "AB",
    "desc": "What this node does.",
    "inputs": ["type_a", "type_b"], "outputs": ["type_c"]
}
```

### Categories
Add to both `CATEGORIES` (Python) and `FALLBACK_CATEGORIES` (JS):
```python
"new_cat": {"label": "New Category", "color": "#ff00ff"}
```

## Adding New Chains

1. Define the chain in `PRESETS` (both Python and JS)
2. Add payload generator to `PAYLOADS` (Python)
3. Add encyclopedia entry to `ENCYCLOPEDIA` (JS)

## Code Style

- **JS**: No template literals (backticks). Use string concatenation with `+`.
- **Python**: Use double quotes for strings, `chr(10)` for newlines in payloads.
- **CSS**: Single-line declarations, kebab-case variables.

## Testing

Test these scenarios before submitting:
- [ ] Pyodide loads successfully (online)
- [ ] Fallback data loads (offline / CDN blocked)
- [ ] Drag and drop works
- [ ] Port connections work
- [ ] Validation shows correct colors
- [ ] Payload generation works
- [ ] All preset chains load
- [ ] Encyclopedia toggles

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
