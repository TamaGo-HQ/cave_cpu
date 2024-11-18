# CPU Design Documentation  

## Introduction  

### Purpose  
Having very little to no knowledge about computer architecture, I jumped straight into designing a CPU to learn bit by bit along the way. This project is all about exploring and understanding the inner workings of a CPU through hands-on experimentation.  

### Scope  
This is purely an educational project. No fancy high-performance goals here—just building and learning!  

---

## CPU Architecture Overview  

### Architecture  
This is a **RISC CPU** (because simplicity is a good friend when you're starting out). 
Here is the circuit of my little beautiful (though a bit dim) child 
[![8-bit-cave-cpu](https://github.com/user-attachments/assets/e6763a5f-8fe1-49cd-abcc-c03f84832f06)](https://github.com/TamaGo-HQ/cave_cpu/blob/main/assetes/8-bit-cave-cpu.png)

### Block Diagram
<p align="center">
<img src="https://github.com/TamaGo-HQ/cave_cpu/blob/main/assetes/block_diagram.jpg" alt="Description" width="400"/>
</p>
---

## Components and Modules  

### ALU (Arithmetic Logic Unit)  
- **Inputs**: Accepts two 8-bit operands.  
- **Operations**: Can perform the following: AND, OR, XOR, NOR, ADD, SUB.  
- **Outputs**:  
  - The result of the selected operation.  
  - Three flags:  
    - **Zero Flag**: Set when the result is zero.  
    - **Carry Flag**: Indicates carry-out in unsigned arithmetic.  
    - **Sign Flag**: Indicates the sign of the result in signed arithmetic.  

### Control Unit  
- **Instruction Storage**: Instructions are stored in RAM.  
- **Execution Flow**:  
  1. Fetches the opcode and stores it in the **Instruction Register (IR)**.  
  2. Passes the opcode to the **Decoder** for interpretation.  
  3. Depending on the instruction, it may take one or two clock cycles to execute.  
- **Conditional Logic**: Flags (Zero, Carry, Sign) play a role in decoding conditional instructions like jumps and branches.  
- Once decoded, the **Control Unit** sends control signals to the ALU, Registers, and RAM.
- Here is a link for the CPU Control sheet https://docs.google.com/spreadsheets/d/1tAWuyxImDcMiM9DDH_h-L-Kt0MrAdT6J6Xk6zP4RwRw/edit?usp=sharing 

### Registers  
1. **General Purpose Registers**  
   - Two 8-bit registers for general computations.  
2. **Flag Registers**  
   - Zero Flag, Carry Flag, and Sign Flag.  
3. **Instruction Pointer (IP)**  
   - Takes input from the **Program Counter (PC)** or a general-purpose register (for jump instructions).  
4. **Instruction Register (IR)**  
   - Holds the opcode fetched from RAM.  

---

## Instruction Set  
Here’s the minimal set of instructions supported by the CPU:  
- `mov r, r`  
- `mov r, imm`  
- `mov r, [r]`  
- `mov [r], r`  
- `sub r, r`  
- `add r, r`  
- `sub r, imm`  
- `add r, imm`  
- `jxx imm` (Conditional jumps).  

---

## Detailed Gate-Level Design  

### ALU Design  
We started by building a **1-bit ALU**, which was then expanded into an **8-bit ALU** by instantiating eight 1-bit ALU modules.  

- **1-bit ALU**  
  - **Inputs**:  
    - Two operands.  
    - A `SUB` signal (to indicate subtraction).  
    - A `Carry In`.  
    - A 3-bit operation code.  
  - **Outputs**:  
    - Result of the operation.  
    - `Carry Out`.
<div style="display: flex; justify-content: space-around;">
  <img src="https://github.com/TamaGo-HQ/cave_cpu/blob/main/assetes/1_bit_alu.png" alt="Image 2 Description" height = "300" width="300"/>
  <img src="https://github.com/TamaGo-HQ/cave_cpu/blob/main/assetes/8_bit_alu.png" alt="Image 1 Description"  height = "300" width="300"/>
  
</div>

- **8-bit ALU**  
  - Built from 8 instances of the 1-bit ALU.  
  - Outputs include:  
    - Result of the 8-bit operation.  
    - Zero, Carry, and Sign Flags.  

---

## Instruction Cycle  

The **Instruction Cycle** is how the CPU processes instructions. It includes:  

1. **Fetch**  
   - The CPU retrieves the instruction from memory using the Program Counter (PC).  
   - The instruction is loaded into the **Instruction Register (IR)**.  
   - The PC is incremented to point to the next instruction.  

2. **Decode**  
   - The **Control Unit** interprets the opcode from the IR via the **Decoder**.  
   - Control signals are generated to activate components.  

3. **Execute**  
   - The specified operation is performed (e.g., arithmetic, data transfer, or branching).  

4. **Write Back**  
   - If applicable, the result is written to a register or memory.  

**Example: MOV B, IMM**  
1. Fetch the instruction.  
2. Decode the opcode (`03`) and determine that it's a `MOV` instruction.  
3. Fetch the immediate value from RAM (stored in the next address) and pass it to the ALU.
4. ALU adds immediate value to zero.  
5. Store the ALU output -the immediate value- in Register B.  

---

## Testing and Simulation Results  

- The simulation validated the combinational logic of the circuit.  
- Timing delays caused issues in some stages, there still is some weird loops that are visible in step simulation
- Next steps would involve a more rigorous timing analysis and moving the design to a hardware description language (HDL) like Verilog or VHDL for simulation in **Quartus Prime** or **ModelSim**.  

---

## Conclusion and Future Improvements  

This project was an incredible first step into CPU design. It gave me insights into computer architecture and how simple components like flip-flops and multiplexers can control data flow.  

**Future Plans**:  
- Make the design more modular.  
- Separate instruction memory and data memory.  
- Learn real design patterns to improve the architecture.  
- Add more instructions and registers.  
- Transition to HDL for more accurate simulations and expand the project.  

