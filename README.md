# semicolab-precheck-template

Template for SemiCoLab IP tile repositories. Fork this repo to submit a tile to the SemiCoLab shuttle.

---

## Getting started

### 1. Create your repo from this template

Click **Use this template** → **Create a new repository**.

Name your repo following the convention:

```
ip_<design_name>
```

Examples: `ip_adder_tile`, `ip_shift_register`, `ip_accumulator`

### 2. Add your RTL

Place your Verilog source files in `src/`. One of the files must be named after your `top_module`.

```
src/
└── your_tile.v
```

### 3. Fill in tile_config.yaml

Open `tile_config.yaml` and fill in all fields. The `top_module` field must match your RTL filename exactly.

```yaml
tile_name: "My Tile"
tile_author: "Your Name"
top_module: "my_tile"
shuttle: ""
```

### 4. Push to main

Every push to `main` triggers the SemiCoLab Precheck automatically:

- ✅ Connectivity check — verifies your RTL connects correctly to the SemiCoLab port interface
- ✅ Synthesis — validates your RTL synthesizes without errors or inferred latches
- 📄 Generates a datasheet, netlist diagram and results report

If the precheck passes, your tile is ready for shuttle submission.

---

## SemiCoLab Port Convention

Your tile must implement these 9 ports:

| Port | Direction | Width | Description |
|---|---|---|---|
| `clk` | input | 1 | Clock |
| `arst_n` | input | 1 | Async reset, active low |
| `csr_in` | input | 16 | Control/Status Register input |
| `data_reg_a` | input | 32 | Operand A |
| `data_reg_b` | input | 32 | Operand B |
| `data_reg_c` | output | 32 | Result |
| `csr_out` | output | 16 | Control/Status Register output |
| `csr_in_re` | output | 1 | CSR input read enable |
| `csr_out_we` | output | 1 | CSR output write enable |

---

## What gets generated

After each push, the CI generates and commits:

```
docs/
├── datasheet.pdf    ← tile datasheet
├── netlist.svg      ← synthesized netlist diagram
└── results.json     ← structured output for shuttle integration
logs/
├── connectivity.log
└── synth.log
```

The README is updated automatically with your tile info, badge and precheck results.

---

## Troubleshooting

**Connectivity check failed**
- Verify all 9 ports are declared in your RTL with correct names and widths
- Check `logs/connectivity.log` for the exact error

**Synthesis failed**
- Look for inferred latches — make sure all `if` statements have an `else`
- Check `logs/synth.log` for details

**top_module not found**
- Make sure `top_module` in `tile_config.yaml` matches your RTL filename exactly

---

## Part of the SemiCoLab ecosystem

```
TileWizard      → wraps generic IP RTL into a SemiCoLab-compatible tile
VeriFlow        → local RTL verification with waveforms
Precheck (CI)   → connectivity check + synthesis gate for shuttle submission
```
