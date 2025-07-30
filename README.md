#  RISC-V-Based MYTH Workshop

This repository documents my hands-on learning journey through the [RISC-V MYTH Workshop](https://www.vlsisystemdesign.com/myth/), conducted by **VLSI System Design (VSD)** and **Redwood EDA**. Over five  days, I learned to build a pipelined RISC-V CPU, starting from C programming and ending with RTL simulation using TL-Verilog. 

## 📌 Introduction

The RISC-V MYTH Workshop was a deep dive into microprocessor architecture and implementation. I started by writing basic C programs and simulating them with the RISC-V Spike simulator. Then, I gradually transitioned to understanding number systems and RISC-V assembly. Eventually, I moved into RTL hardware design using TL-Verilog on the Makerchip platform, where I constructed a complete pipelined CPU from the ground up.

This workshop provided hands-on exposure to:

* RISC-V Instruction Set Architecture (ISA)
* Writing and debugging C and assembly code
* Simulating programs with Spike and analyzing binaries using objdump
* Hardware design with TL-Verilog
* Register file, ALU, control logic, and pipeline design

---

##  RISC-V Instruction Set Architecture (ISA)

### 2.1 Overview of RISC-V ISA

* Open-source and modular ISA
* Supports 32-bit and 64-bit architectures
* Simplified and regular instruction formats

### 2.2 Instruction Formats

| Type | Usage                | Structure                                       | Example             |
| ---- | -------------------- | ----------------------------------------------- | ------------------- |
| R    | Register ops         | opcode, rd, funct3, rs1, rs2, funct7            | `add x1, x2, x3`    |
| I    | Immediate ops, loads | opcode, rd, funct3, rs1, imm\[11:0]             | `addi x1, x2, 10`   |
| S    | Stores               | opcode, imm\[11:5], rs2, rs1, funct3, imm\[4:0] | `sw x1, 0(x2)`      |
| B    | Branches             | opcode, imm, rs2, rs1, funct3                   | `bne x1, x2, label` |
| U    | Upper immediates     | opcode, rd, imm\[31:12]                         | `lui x1, 0x10000`   |
| J    | Jumps                | opcode, rd, imm                                 | `jal x1, label`     |

### 2.3 Pseudo-Instructions

| Pseudo           | Real Equivalent  | Notes             |
| ---------------- | ---------------- | ----------------- |
| `li x1, 5`       | `addi x1, x0, 5` | Load immediate    |
| `mv x1, x2`      | `addi x1, x2, 0` | Register move     |
| `mul x1, x2, x3` | Not in RV64I     | Needs M-extension |

These pseudo-instructions simplify programming and let you focus more on logic than syntax.

---

## 📅 Day-Wise Lab Progress

### Day 1 

#### 1 C Program: Sum of First N Natural Numbers

```c
#include <stdio.h>
int main() {
    int i,n, sum = 0;
    printf("Enter value of N: ");
    scanf("%d",&n);
    for(i = 0; i <= 10; i++) {
        sum = sum + i;
    }
    printf("Sum of numbers 1 to %d: %d", n, sum);
    return 0;
}
```

#### 2 Compilation for RISC-V

```bash
riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o sum.o sum.c
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o sum.o sum.c
```

**Flags:**

* `-O1` : Optimization level 1
* `-Ofast` : Fastest optimization
* `-mabi=lp64` : 64-bit data model
* `-march=rv64i` : Base 64-bit RISC-V ISA

#### 3 Disassembly with objdump

```bash
riscv64-unknown-elf-objdump -d sum.o | less
```

This displayed the binary in assembly, giving me a deeper look into how C code translates into machine-level instructions.

#### 4 Spike Simulation

```bash
spike pk sum.o
```

#### 5 Spike Debug Mode

```bash
spike -d pk sum.o
```

Useful commands:

* `reg 0 a0` : View return value
* `reg 0 sp` : View stack pointer

**Insight:** Debugging with Spike showed how memory and registers interact in real time.


<img width="1920" height="1440" alt="Day1Code" src="https://github.com/user-attachments/assets/4c05f52d-39fb-4d4d-a912-f3063da713a8" />
<img width="1920" height="1413" alt="Day1Compile" src="https://github.com/user-attachments/assets/c3867c64-c5b6-4f35-943e-8d43f07f1d91" />
<img width="1920" height="1413" alt="Day1ObjdumpLess" src="https://github.com/user-attachments/assets/3ad3adf4-1304-414e-92c2-4d9acc8f1c2d" />
<img width="1920" height="1413" alt="Day1ObjDumpLessMain" src="https://github.com/user-attachments/assets/96daf942-edf5-4a29-aa62-1e3c52db4265" />
<img width="1920" height="1413" alt="Day1ObjDumpOfast" src="https://github.com/user-attachments/assets/b7944bd5-a5a2-437c-9903-0cf52bab2fe3" />
<img width="1920" height="1413" alt="Day1SpikeDebug" src="https://github.com/user-attachments/assets/c67fa75d-0816-459f-91d7-5de9bb983fb5" />

---

### Day 2

* Learned TL-Verilog syntax and Makerchip usage
* Wrote basic arithmetic operations using pipeline stages like `@1`

**Sample TLV Code:**

```tlv
@1
   $val1[31:0] = $rand;
   $val2[31:0] = $rand2;
   $sum[31:0] = $val1 + $val2;
```
<img width="1920" height="1413" alt="Day2labLoad" src="https://github.com/user-attachments/assets/59a7ead3-99b8-42eb-9238-4c3e05b6651e" />
<img width="1920" height="1413" alt="Day2LabSumCustom" src="https://github.com/user-attachments/assets/7e9df216-8f75-4a4b-b283-a79a7ab29bb5" />
<img width="1920" height="1413" alt="Day2Objdump" src="https://github.com/user-attachments/assets/b395c713-a253-4aae-9d81-88636430f25e" />
<img width="1920" height="1413" alt="Day2ScriptsFinalHex" src="https://github.com/user-attachments/assets/cd26d58b-a401-440e-adce-214adb6ac306" />
<img width="1920" height="1413" alt="Day2SpikeSim" src="https://github.com/user-attachments/assets/659b236d-19d7-464b-97b7-9a4266e20883" />

---


### Day 3 

* Learned to design IF stage with PC and memory
* Learned to create ID stage: opcode parsing, instruction type detection, control signals

**Concepts:**

* TL-Verilog Macros: `@n`, `?`, `*reset`
<img width="2557" height="1443" alt="Day3Lab1CombASol" src="https://github.com/user-attachments/assets/b5fb43fd-743a-43be-906c-74458cc8dc82" />
<img width="2558" height="1445" alt="Day3Lab1CounterSol" src="https://github.com/user-attachments/assets/82824b94-8963-4930-8c0d-146ce00d8ffa" />
<img width="2558" height="1436" alt="Day3Lab1CombCalulatorSol" src="https://github.com/user-attachments/assets/ed834b7b-2d32-4c77-a401-536ce066e1d7" />
<img width="2558" height="1435" alt="Day3Lab1Cycle2Sol" src="https://github.com/user-attachments/assets/182e40f4-46e5-405b-8b0e-040aa22ef4fa" />
<img width="2558" height="1432" alt="Day3Lab1CycleCounter2Sol" src="https://github.com/user-attachments/assets/e9839262-e967-4898-a2ec-dc1fff16349e" />
<img width="2558" height="1436" alt="Day3Lab1MuxSol" src="https://github.com/user-attachments/assets/279f599f-0594-4c03-8a94-31a1eeb07751" />
<img width="2558" height="1432" alt="Day3Lab1PipelineSol" src="https://github.com/user-attachments/assets/740c8e8b-9a33-4c59-b411-3f3dd72da21b" />
<img width="2557" height="1437" alt="Day3Lab1SeqCalcSol" src="https://github.com/user-attachments/assets/ced041b1-41f4-44da-9b19-d9527ccc36d4" />
<img width="2558" height="1430" alt="Day3Lab1VectorSol" src="https://github.com/user-attachments/assets/5cf7397d-30ef-4d5d-97ff-7cd95bf0add5" />
<img width="2558" height="1437" alt="Day3Lab1DistanceLabSol" src="https://github.com/user-attachments/assets/25fae323-b3e6-4401-bc8d-a1c75c0b5cfb" />



---

### Day 4 

Day 4 focused on building the essential compute and control path of the RISC-V processor using TL-Verilog. The key components implemented included the **ALU**, **register file connectivity**, and **branching logic**.

####  Implemented 

* **Instruction Decode Logic**

  * Extracted fields: `opcode`, `funct3`, `funct7`, `rs1`, `rs2`, `rd`
  * Differentiated between instruction types: R-type, I-type, B-type, etc.

* **Immediate Generation**

  * Parsed immediate values based on format:

    * I-type → sign-extended bits \[31:20]
    * B-type → concatenation and extension of bits \[31], \[7], \[30:25], \[11:8]

* **Register File**

  * Read values from `rs1` and `rs2`
  * Write-back to `rd` (except x0)
  * Ensured writes occurred only on valid instructions

* **ALU**

  * Performed operations based on instruction type:

    ```tlv
    $result = $is_addi ? $src1 + $imm :
              $is_add  ? $src1 + $src2 :
              $is_sub  ? $src1 - $src2 : 32'bx;
    ```

* **Branch Logic**

  * Implemented comparison checks:
    * `beq`, `bne`, `blt`, `bge`, `bltu`, `bgeu`
  * Controlled PC update with branching:

    ```tlv
    $pc = $taken_br ? $pc + $imm : $pc + 4;
    ```

#### Simulation Outcomes
* Verified register updates and ALU operations
* Observed correct branching behavior
* All instructions fetched, decoded, and executed successfully
  
#### Instructions Handled
| Type | Instructions Covered                       |
| ---- | ------------------------------------------ |
| R    | `add`, `sub`                               |
| I    | `addi`                                     |
| B    | `beq`, `bne`, `blt`, `bge`, `bltu`, `bgeu` |

---
Let me know if you'd like this version adapted further or formatted into your working README.

* Implemented ALU operations: add, sub, addi
* Built branch logic: beq, bne, etc.
* Designed PC update mechanism

**Branch Logic Example:**

```tlv
?$br_taken
   @3
      $pc = $pc + $imm;
```

---

###  Day 5 

* Integrated IF → ID → EX → MEM → WB
* Built register file and writeback logic
* Simulated and verified the full pipeline

**Register File Example:**

```tlv
@5
   ?$rd_we
      $rf[$rd] = $alu_result;
```

---



---

## 🔧 Tools and Platforms Used

| Tool                        | Purpose                           |
| --------------------------- | --------------------------------- |
| riscv64-unknown-elf-gcc     | Compile C to RISC-V ELF           |
| riscv64-unknown-elf-objdump | Disassembly                       |
| Spike                       | ISA simulation                    |
| Spike -d                    | Debugging                         |
| TL-Verilog                  | RTL design                        |
| Makerchip                   | TLV simulation + waveform viewing |
| Verilog, GTKWave, Icarus    | RTL simulation (later stages)     |

---

## 💡 Technical Insights

* 32 general-purpose 64-bit registers in RV64I
* Register `x0` is always 0
* Binary representation: 6-bit unsigned = 0–63; 7-bit 2's comp = -64 to 63
* Data sizes: Byte = 8b, Word = 32b, DWord = 64b
* Pipeline stages: Fetch → Decode → Execute → Memory → Writeback

---

## 🙏 Credits

Special thanks to:

* **Kunal Ghosh** (VSD)
* **Steve Hoover** (Redwood EDA)

for designing and mentoring this workshop. All open-source resources and lab materials are available on GitHub.

---

Happy learning!
