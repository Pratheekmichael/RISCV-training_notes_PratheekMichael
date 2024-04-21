# RISCV-training_notes_PratheekMichael
RISCV training and lab details
Note: All snapshots are taken from the workshop with permission from Mr. Kunal Gosh.
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
apps --> system software (OS (program lang)--> compilter (RISC-V/ARM, ISA)--> assembler (converts to Machine language program)) --> Hardware (RISCV CPU core or ARM CPU)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/c7b06a4e-a82d-43e6-a0a1-61e052cb3e7d)

D1-SK2 - SoC Design and Openlane

L1 - Intro to components of open-source digital ASIC design
In 1979 Lynn Conway & Carver Mead seperated the design from technology and pioneered Structure design methodology which is based on Lambda design rules --> this led to Fabless company.
PDK = process design kit which comprises of DRC, LVS, device models, Standard cell libraries, IO libraries which can be used by EDA to model a fabrication process during IC design.
Google has made agreement with Skywater to release open source PDK on June 2020, i.e., sky 130nm process.
To understand why 130nm is exiting when we have reached less than 10nm tech, see the market share distribution.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3ffc1ffb-cbbb-47eb-bcbb-9e6131badfb6)
130nm is 6% which is almost $4bn market share.
ASIC flow objective is to take RTL (register transfer level) to GDSII, aka, Automated PnR or physical implementation.

L2 - Simplified RTL2GDS flow
RTL --> synthesis --> floor planning or power planning --> placement --> Clock tree synthesis (goal: min skew) --> routing --> signoff --> GDSII
Before signoff, we do 
Physical verifications -- DRC, Layout vs schematic (LVS) checks and Timing verification (STA - static timing analysis)

L3 - Introduction to openlane & strive chipsets
Openlane is Open source ASIC design implementation flow. Goal of this is to produce a clean GDSII with no human intervention, that means no voilations in DRC, LVS, or Timing. Two modes of operation: automonomous and interactive. 

L4 - Intro to openlane detailed ASIC flow
Design RTL --> RTL synthesis --> STA --> Design for Test (DFT) -->  physical implementation (using operoad app) --> Logic equivalence check --> Detailed routing --> if needed can deal with antenna rule voilation by insertion of fake antenna diode next to every cell input to check voilation ( if yes, a diode can be palced to mitigate this) (magic can be used) --> STA, DRC & LVS (magic can be used for DRC and spice extraction)

D1-SK3 Opensource EDA tools

L1 - Openlane directory structure
Ubuntu help
ls --help will list all cmds what do they do
Sky130A --> libs.tech & libs.ref which has files associated with technology and process respectively. we will be working with  sky130_fd_sc_hd file {130nm, fd-skywater foundary, sc-standard cell, hd-high density}

L2- Design prep step,
In the openlane directory, which is cd Desktop/work/tools/openlane_working_dir/openlane, we invoke the command "docker". docker apparently is a short for the command docker run -it -v $(pwd_:/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PRDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc2.
the next command is "./flow.tcl -interactive"

Day 2

Day 3

Day 4

Day 5
