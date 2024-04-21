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
The highest priority .tcl file is sky130A_sky130_fd_sc_hd_config.tcl which will overwrite the config.tcl settings, ex: clock period.
In the openlane directory, which is cd Desktop/work/tools/openlane_working_dir/openlane, we invoke the command "docker". docker apparently is a short for the command docker run -it -v $(pwd_:/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PRDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc2 (not verified as copied from video, version might be v0.21)
the next command is "./flow.tcl -interactive", once Open lane launches, we import packages required to run this and the command is "package required openlane 0.9"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/186f215d-ad18-4148-87de-31c24bf7e069)
the next step is to prep design, to have design specific files generated in the Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a folder. the command for  this is "prep -design picorv32a"

L3- Review files post design prep and run synthesis
so in the Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a a new runs folder got created due to the design prep command we executed, our later synthesis file will be in this folder.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3304a0b9-a439-4f02-9fae-c3a5fb160e88)
now we can use the command "run_synthesis" to perform the first step in the physical implementation.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0fd62705-190e-4159-a762-dc3effebbc53)
openalne efabless information is available on github at https://github.com/efabless/openlane2
Once the synthesis is succesfully completed, we can use "exit" command to exit from openlane and "exit" to exit the docker, this will ensure nothing is active in the background.


L4-openlane project link 
To install the openlane in ubuntu or VB from scratch, we first install git "sudo apt install git" and use the git clone "[url](https://github.com/efabless/OpenLane.git)" command to copy over the openlane files from the github link, https://github.com/efabless/openlane
Open lane flow is shown in the greyed box of this flow
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/52a2c902-4b57-4e18-9708-00a56b575cd2)
command structure has to follow a flow, synthesis-->floorplan-->placement-->cts-->routing, if this is not followed the flow will fail to generate output. A fully automation of these commands can be done by the command "./flow.tcl -design picorv32a" the right part of the command is the design name.
On youtube, it is recommended to watch the fossi-dialup videos by Tim Ansell, and Mohamed Shalan which is regarding Skywater PDK and Open lane respectively.

L5: Steps to characterize the synthesis results
The task is to find the flop ratio = # of D flip flops/ # of cells. In the synthesis which was run the ratio is 10.84%.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0a3e46ac-d981-4db9-9cf6-96b04c8f7d0b)
The results folder will have the synthesized checklist
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8070899f-678d-4821-8c18-efc125cfe2df)
The synthesis statistic report gives us the info about the flop ratio again.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/4a3402eb-878f-4fba-ac5e-0533096ccf0d)

Day 2 - Good floorplan vs bad floorplan and introduction to library cells

SK1 - chip floor planning considerations

L1 - Utilization factor and aspect ratio


Day 3

Day 4

Day 5
