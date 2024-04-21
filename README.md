# RISCV-training_notes_PratheekMichael
RISCV training and lab details

Day 1 - Inception of open-source EDA, openlane and Sky130PDK
D1_SK1 How to talk to computers
L1 Intro to QFN-48 package, chip, die, pad, core, IP
The take away is that the processor is the brain of the arduino or an FPGA board and it needs to communicate with the interfaces. The chip design is the main focus of the course, a package can be synonymously called as a chip, so when QFN-48 Quad Flat No lead package is spoken about, we can understand this lingo. the package pins can be decided by the application board, io chip enables communication with outside signal to the chip. IO pins are connected to pads.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/52501a88-d0ea-4c2d-b2d1-a598e531a845)
Foundry IP, is Foundry intellectual property which is protected by the respective foundry. VLSI ENGs needs to communicate foundry, to build the chip. Chip contains Macros and Foundry IP.
L2- RSIC V architecture
Assembly lang program which is used to talk to computers. Code on computer is complied in RISCV assembly Language program which is converted to RTL which converts it to machine language which will be processed by the computer in order to test the layouts which are being designed. 
In short, RISC V architecture --> implementation (picorv32 cpu core) (RTL) --> layout (qflow) desing & test.

L3- software apps to Hardware
apps --> system software (OS (program lang)--> compilter (RISC-V/ARM)--> assembler (converts to Machone language program)) --> Hardware (RISCV CPU core or ARM CPU)

Day 2

Day 3

Day 4

Day 5
