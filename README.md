# Synchronous-RAM

## 📌 Project Title
✅ **Parameterized Single-Port Synchronous RAM (Clocked Read/Write)**

---

## 📝 Description
This module implements a **single-port synchronous RAM** where both **read and write operations occur on the rising edge of the clock** ⏱️.

- ✍️ When `w_r = 1`, the RAM performs a **WRITE**
- 📖 When `w_r = 0`, the RAM performs a **READ**
- 🔄 Reset (`rst`) clears the memory and output

This design is useful for basic memory blocks in FPGA/ASIC designs where read data is registered (synchronous read).

---

## ⭐ Key Features
- ⚙️ Parameterized configuration:
  - `WIDTH` = data width (default **8-bit**)
  - `DEPTH` = number of memory locations (default **16**)
  - `ADDR = $clog2(DEPTH)` = address width
- ⏱️ **Synchronous write**
- ⏱️ **Synchronous read** (registered output)
- 🔄 Reset initializes **memory contents** and `rdata_out` to `0`

---

## 🔌 Module Interface

### 📄 Declaration
```verilog
module synchronous_RAM #(parameter WIDTH=8, DEPTH=16, ADDR=$clog2(DEPTH)) ( ... );
```

### 🧾 Ports
| Signal      | Dir | Width | Description |
|------------|-----|-------|-------------|
| `clk`      | In  | 1     | ⏱️ Clock |
| `rst`      | In  | 1     | 🔄 Reset (clears memory + output) |
| `w_r`      | In  | 1     | ✍️ Write=1 / 📖 Read=0 |
| `addr`     | In  | `ADDR` | 📍 Address |
| `wdata_in` | In  | `WIDTH` | 📨 Write data |
| `rdata_out`| Out | `WIDTH` | 📤 Read data (registered) |

---

## 🧠 Functional Behavior

### 🔄 Reset
On the rising edge of `clk`, if `rst = 1`:
- `rdata_out <= 0`
- All memory locations cleared:
  - `mem[i] <= 0`

### ✍️ Write Operation
On `posedge clk`, if `rst = 0` and `w_r = 1`:
- `mem[addr] <= wdata_in`

### 📖 Read Operation
On `posedge clk`, if `rst = 0` and `w_r = 0`:
- `rdata_out <= mem[addr]`

✅ Read data becomes valid **after the clock edge** (registered output).

---

## ⏱️ Timing Notes
- **Write** updates memory on clock edge.
- **Read** updates `rdata_out` on clock edge.
- If `addr` changes, `rdata_out` does **not** change until the next `posedge clk`.

---

## ⚙️ Parameter Example
Example: **32-depth RAM with 16-bit data**
```verilog
synchronous_RAM #(
  .WIDTH(16),
  .DEPTH(32)
) u_ram (
  .clk(clk),
  .rst(rst),
  .w_r(w_r),
  .addr(addr),
  .wdata_in(wdata_in),
  .rdata_out(rdata_out)
);
```

---

## 📁 Files
- 📄 `synchronous_RAM.v` — Synchronous single-port RAM module

