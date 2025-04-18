# pipelined-RISC-V-processor
# Introduction
Pipelining is a technique used to enhance the throughput of a digital system by dividing the processing of instructions into multiple stages. Each stage performs a specific function, enabling simultaneous execution of different instructions. This section outlines the design and operation of a pipelined processor, compares its performance with a single-cycle processor, and discusses challenges like hazards and control synchronization.

# Pipelined Processor Design
The pipelined processor subdivides a single-cycle processor into five stages, namely:
1.	Fetch (IF): Retrieves the instruction from the instruction memory.
2.	Decode (ID): Reads source operands from the register file and generates control signals.
3.	Execute (EX): Performs arithmetic or logical operations using the ALU.
4.	Memory (MEM): Accesses data memory for reading or writing.
5.	Writeback (WB): Writes the result back to the register file.
Each stage performs one operation per clock cycle, enabling the processor to execute multiple instructions concurrently.


![image](https://github.com/user-attachments/assets/bbe98232-8ac8-4105-8c9d-4834fbe59596)

# Key Characteristics
1.	Throughput vs. Latency:
o	Throughput measures the number of instructions completed per second.
o	Latency is the time taken for a single instruction to complete.
o	While the latency of each instruction remains unchanged, the throughput is significantly improved with pipelining, as new instructions enter the pipeline every clock cycle.
![image](https://github.com/user-attachments/assets/ac1289c4-a61d-49d1-bbc4-edc87be0f781)
2.	Performance Metrics:
o	In a single-cycle processor, the clock cycle is determined by the longest operation, resulting in lower throughput.
o	In a pipelined processor, each stage's clock period is defined by the slowest stage, enabling a higher clock frequency. For example:
	Single-cycle latency: 680 ps per instruction.
	Pipelined latency: 1000 ps per instruction (5 stages, 200 ps each).
	Pipelined throughput: 1 instruction every 200 ps (5 billion instructions per second).
3.	Pipeline Stages:
o	Each instruction progresses through the stages sequentially.
o	For example, at cycle 0, the first instruction enters Fetch. By cycle 4, it completes Writeback, while subsequent instructions simultaneously occupy earlier stages.
# Datapath and Control
The pipelined processor datapath is derived by inserting pipeline registers between the stages of a single-cycle datapath. These registers store data and control signals, ensuring synchronization across stages.
1.	Datapath Modifications:
o	Pipeline registers separate Fetch, Decode, Execute, Memory, and Writeback stages.
o	Control signals are pipelined alongside data to ensure proper execution.

![image](https://github.com/user-attachments/assets/da955896-61f8-4e95-8c6e-a99c8a021e0c)

2.	Control Unit:
o	The control unit operates in the Decode stage, generating signals based on the instruction's opcode and function fields.
o	Signals like RegWrite and destination registers are pipelined to ensure they reach the appropriate stage at the correct time.

![image](https://github.com/user-attachments/assets/ad65270f-0ebd-4f3a-9ff3-ec1be80c74c1)

# Performance Analysis
•	The pipelined processor improves throughput but incurs additional overhead from pipeline registers and hazard mitigation techniques. For example:
o	Ideal throughput increase: 5× compared to the single-cycle processor.
o	Real-world improvement: Approximately 3.4× due to overhead and imbalances in stage durations.
# Simulation
![image](https://github.com/user-attachments/assets/dd1f4811-55e6-4616-850b-95ed7d3dea82)
![image](https://github.com/user-attachments/assets/0804336d-bd9f-4ee9-9b75-b5d6dce4ba4c)
![image](https://github.com/user-attachments/assets/3c3bde51-59d8-40de-896c-1b789579b0f6)
# Hazard unit
The hazard unit is a critical module in a pipelined processor, tasked with resolving data hazards through data forwarding (also known as bypassing). This mechanism ensures uninterrupted instruction execution by addressing dependencies between instructions in different stages of the pipeline.

# Functionality
The hazard unit detects scenarios where source registers in the Execute (E) stage depend on the results of instructions in the Memory (M) or Write-back (W) stages. If such dependencies exist, the hazard unit forwards the required data to the Execute stage, eliminating the need for stalls.
![image](https://github.com/user-attachments/assets/6fbed120-5b0c-47fd-898e-796a2e856f6a)
## Forwarding Logic
The module implements a priority-based forwarding logic for both source registers (Rs1_E and Rs2_E).
## Reset Behavior
When rst is active (rst == 1'b0), no forwarding occurs, and the output signals are set to 2'b00.
# Conditions for Forwarding
1.	Forwarding from Memory Stage (2'b10):
o	The instruction in the Memory stage writes to a register (RegWriteM == 1'b1).
o	The destination register in the Memory stage (RD_M) matches the source register in the Execute stage (Rs1_E or Rs2_E).
o	The destination register is not the zero register (RD_M != 5'h00).
2.	Forwarding from Write-back Stage (2'b01):
o	The instruction in the Write-back stage writes to a register (RegWriteW == 1'b1).
o	The destination register in the Write-back stage (RD_W) matches the source register in the Execute stage (Rs1_E or Rs2_E).
o	The destination register is not the zero register (RD_W != 5'h00).
3.	No Forwarding (2'b00):
o	Default condition when none of the above conditions are satisfied.

# Conclusion
Pipelining is an essential technique for modern microprocessor design, offering significant improvements in throughput for minimal cost. Challenges like hazards and control synchronization are mitigated using techniques like forwarding, stalls, and pipelined control. While not achieving ideal performance due to overhead, pipelining remains a cornerstone of high-performance computing.




# Reference:
S. L. Harris and D. M. Harris, Digital Design and Computer Architecture: RISC-V Edition. Cambridge, MA, USA: Morgan Kaufmann, 2021.











