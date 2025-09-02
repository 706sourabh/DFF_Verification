# DFF_Verification

# 🔬 D Flip-Flop Verification Environment (SystemVerilog)

This repository contains a **SystemVerilog Verification Environment** for a **D Flip-Flop (DFF)**.  
The environment is built using **transaction-level modeling** with components like **generator, driver, monitor, scoreboard, and environment class**.  
It demonstrates a simple but structured verification flow that resembles **UVM-style testbenches**.

## Project Structure

## Design Overview
### D Flip-Flop (`design/design.sv`)
- Implements a **positive-edge triggered D Flip-Flop** with synchronous reset.
- Uses a **SystemVerilog interface (`dff_if`)** to bundle signals:
  - `clk` : Clock
  - `rst` : Reset
  - `din` : Data input
  - `dout`: Data output

Behavior:
- On reset (`rst=1`), `dout = 0`.
- Otherwise, `dout` follows `din` on each positive clock edge.

---

## Testbench Architecture (`tb/testbench.sv`)
The verification environment is built from modular components:

- **Transaction**  
  Defines input (`din`) and output (`dout`) fields. Supports randomization and copy.

- **Generator**  
  Randomizes transactions, sends them to both **driver** and **scoreboard reference mailbox**.

- **Driver**  
  Drives `din` into the DUT via the interface. Also performs reset at start.

- **Monitor**  
  Samples DUT outputs (`dout`) and sends them to the scoreboard.

- **Scoreboard**  
  Compares monitored outputs against reference transactions from generator.  
  Reports **MATCH** or **MISMATCH**.

- **Environment**  
  Instantiates and connects all components. Controls simulation flow (`pre_test`, `test`, `post_test`).

- **Top Testbench (`tb`)**  
  - Instantiates DUT (`dff`).
  - Generates clock.
  - Runs the environment.
  - Dumps waveforms (`dump.vcd`).

---

## Running the Simulation

### Option 1: Run on [EDA Playground](https://www.edaplayground.com/)
1. Copy `design/design.sv` into the **Design** section.  
2. Copy `tb/testbench.sv` into the **Testbench** section.  
3. Select a simulator (e.g., *Icarus Verilog*, *ModelSim*).  
4. Run → View waveforms in the viewer.  

---

### Option 2: Run Locally with Icarus Verilog
Install [Icarus Verilog](http://iverilog.icarus.com/) and [GTKWave](http://gtkwave.sourceforge.net/).  

```bash
# Compile design and testbench
iverilog -o dff_tb design/design.sv tb/testbench.sv

# Run simulation
vvp dff_tb

# View waveforms
gtkwave dump.vcd
