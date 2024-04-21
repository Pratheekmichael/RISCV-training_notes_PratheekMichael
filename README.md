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

Sk1 - chip floor planning considerations

L1 - Utilization factor and aspect ratio
Dimension of the standard cells are the feature which is of interest to find the total area of the standard cells inside the netlist. A silicon wafer will consider multiple die, a die has a core which has the logic built on it. Utilization factor(UF) = area occupied by the netlist/ area of the core. If UF=1 then core is completely utilized and additional logic cannot be necessary. Aspect Ratio = Height of the die/ width of die, if AR 1, then its a square. example width of core is 4 unit and length is 2 unit, UF=0.5, AR=0.5.

L2 - Concept of preplaced cells.
Preplaced cells, a combinational logic output can be implemented in the form of gates, split the gates into blocks,now the io for these blocks need to be connected to regenerate the combination logic, ex for this is memory, clock gating cells, comparator, MUX, these are called preplaced cells. These IPs are palced in user defined location prior to the automated PNR flow, hence they are called preplaced cells. The automated PNR tool cannot move preplaced cells, the PNR tool has to go around these cells to do its function.

L3 - De-coupling capacitors
Why do we need Decoup capacitor? When a gate switches, the supply voltage needs to provide the charge required, the VDD needs to provide this, VSS needs to take in the discharge current. The VDD can have multiple voltage drop due to practical factors like resistance and inductance, therefore, VDD will now have diminished to VDD' which is not the same as VDD. if the VDD' is in the undefined region of the noise margin, then there is a problem for the circuit to design what charge it is getting. Therefore, we add decoupling capacitor to decouple the the circuit from the power supply, this will avoid voltage drop. After the decoup is added, when there is switching activity the decoup cap provides the charge and will replinsh itself when the activity is not done. Therefore, decoupling capacitor is placed in between preplaced cells.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/e7fd48f7-936d-4887-83ec-6d72d49568a6)

L4 - Power planning
Macros needing power which is tapped from VDD. When a 16 bit bus discharges all at once, there is a ground bounce. When these 16 bit bus caps require to charge at once, then it will need power all at once, which will cause voltage droop. Therefore, instead of one single power supply point, we can setup multiple power supply pins or points which is similar to,
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a0547956-feff-4cc4-b791-317dff2b15e7)

L5 - Pin placement and logical cell palcement
Netlist - connectivity information between different gates which is defined in verilog HDL. The pins are placed in the die area outside the core and it will be fixed in this place. Frontend team - netlist connectivity and backend team - pin placement, therefore a proper design knowledge is needed between both teams. Clk ports are a bit thicker, to get least resistance. A logical cell blockage is done in order to make sure once the io pins are placed, nothing is placed in that area by the PNR s/w.

L6 - steps to run floorplan using OPENLANE
After synthesis the next step is the flooplan. PNR flow is an iterative flow which needs certain parameters to be active at certain steps, therfore it is the ENGs decision to make use of appropriate parameters. In Desktop/work/tools/openlane_working_dir/openlane/configuration folder there is a readme.md which will explain about all the parameters.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f23bd0ad-55fc-4d1a-9a4f-204ecc51df21)
The .tcl priority for floorplan.tcl (system default) will be the least and will be superceeded design by config.tcl and the sky130A_sky130_fd_sc_hd_config.tcl
The next step is the run the floorplan, in the openlane directory, which is cd Desktop/work/tools/openlane_working_dir/openlane, we invoke the command "docker". 
The next command is "./flow.tcl -interactive", once Open lane launches, we import packages required to run this and the command is "package required openlane 0.9"
The next step is to prep design, to have design specific files generated in the Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a folder. the command for  this is "prep -design picorv32a", followed by the command "run_synthesis" and the command "run_floorplan"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f6bbbed9-5f6d-49f9-a435-ced986510fb8)

L7, L8- REview flooplan layout in Magic
the flooplan.def (design exchange format) is populated in the results folder.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9a677e8d-0235-4231-a339-fcc8ca986396)
To see actual layout in magic, go to the respective directory
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_23-16/results/floorplan/
and use the command
"magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &" to get the layout after floorplan sim
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/86b5adc4-b228-4e3a-b9c2-e0fca66dbece)
to zoom, use the left and right click of the mouse to define a box and z to zoom in and to select a cell use s after highlighting on the cell use the command "what" in the tkcon window to see what is the selected cell.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9542f146-da54-4783-8ded-c04e05233449)

SK2- Library Binding and placement

L1 - Netlist binding and initial place design
In theory the AND gate may look different than when it is designed in layout, the layout has dimensions which can be used by ENGs. These netlist blocks can be found in Standard cell library, the library will also have various falvors of each cell, i.e., bigger dimensions of the same cells, these bigger cells might improve the design at the cost of the area utilized.
Placement will be with respect the input and output pins and also the netlist priority i.e., how much loss can be afforded when cells are have distance between them.

L2- Optimize placement using estimated wire-length and capacitance
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/61900166-e631-443f-922c-2cba3cf8e41f)
The wire that is needed to connect these cells will have Cap associated with them, the slew gets implemented because of this, therefore repeaters are needed to maintain signal integrity, ENG needs to decide on how many repeaters/buffers are needed. 

L3- final placement optimization, discusses the buffer placement by estimated wire length

L4- Need for libraries and characterization
Logic synthesis --> import netlist of LS and do --> floorplanning --> placement (optimization of netlist placemenent) --> clock tree synthesis --> routing stage (maze routing) --> Static timing analysis (setup time and hold time) --> sign off
Collection of gates are called library.

L5 - Congestion aware placement using replace
When run placement command is executed, a global placement happens, which has HPWL half parameter wirelength which needs to be optimized.
the next command in the flow will be "run_placement"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/28a38c0e-bb50-4e58-8047-8d332bdf20e3)
In order to load the gnerated file go to the directory cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-04_01-39/results/placement/
and use the command "magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &" to view layout post placement
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d6ef537a-1b57-4ad3-ab8e-896905df9fa4)
Placed cells view
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/cc183e4d-91ac-4a06-a451-bbe56b3faf45)
we have to exit the openlane and the docker view in order to avoid acitivity.

SK3 Cell design and characterization

L1- Inputs for cell design flow, L2- circuit design step, L3 - Layout design design step, L4-typical characterization flow
Library is a place where we keep all standard cells, which are buff, FF, AND, OR gates, these cells can be of varying dimesions which will vary the cells drive strength. The dimesions will cause the keep parameter changes, example, threshold change.

The instructors courses, (1) circuit design/analysis and spice simulation, (2) custom layout has more information about the cell design flow.
A rough idea for Cell design flow has (1) inputs: PDKS, DRC, LVS, library and user defined specs, SPICE model, (ENG: choose proper library cell) --> (2) Design steps: Circuit design, layout design, characterization (output of circuit design is circuit description language, output of layout design is GDS)), LEF, extracted spice netlist) --> (3) Ouput (given in the paranthesis of previous step)
GUNA software generated timing, noise, power.libs

SK4 General timing characterization paramters

L1 - Timing threshold definition
Slew low rise threshold: 20% of the max voltage
slew high rise threshold: 80% of the max voltage
similar terminology exists for the falling waveform or output waveform
50% input waveform - 50% ouput waveform ==> rise delay

L2 - Propagation delay and transition time
Choice of threshold can avoid negative delays, negative delays can also be caused by wire delays.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d07afa91-8061-465d-9e3d-724166fdf0a2)

Day 3 - Design Library cell using Magic layout and ngspice characterization

SK1 - Labs for CMOS inverter ngspice simulation

L1- SPICE deck creation for CMOS inverter, L2- SPICE simiulation lab for CMOS inverter
Spice deck is connectivity information about the net list, we need component values, W/L of CMOS, Capacitance of the load, VSS, VDD, the next step is to identify the nodes and name them.
* is comments
  M1 out in vdd vdd pmos W=xu L=yu--> M1 is between out and in nodes and VDD and VDD
 ![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/44e52652-02fc-4294-a014-41e409669fc5)
.Lib "tmsc_025um_model.mod: CMOS_MODELS

W/L for pmos > W/L for nmos for ideal situation

L3 - Switching threshold Vm, L4- Static and dynamic simulation of CMOS inverter
CMOS is a very robust device, even though we changed the Wp=2.5xWn, the waveform is very similar to wp=wn and the only differece will be in the switching threshold.
Vm is the point where Vin=Vout, we basically draw a diag line on the output of the DC result and find the diag line intersection with waveform
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/7f6ee0c4-de6e-4f63-8a90-9750c7311d6f)
Rise and fall delay can be calculated by peforming transient analysis which will be done in the labs

L5 - lab steps to git clone vsdcelldesign
go to the directory cd Desktop/work/tools/openlane_working_dir/openlane
clone the repository, using the command "git clone https://github.com/nickson-jose/vsdstdcelldesign"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/bbdb1086-8768-4e45-91e5-933d1b85736f)
go to the vsdstdcelldesign folder by using "cd vsdstdcelldesign"
to copy the tech file, go to cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic and use the command "cp sky130A.tech /Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/cf934a22-771f-41ad-b405-c2337c7f6c7c)

Sk2- inception of layout of CMOS fabrication process
L1- create active regions, L2-formation of N and P well, L3- formation of gate terminal, L4 - Lightly doped drain, L5- source and drain formation, L7- higher level metal formation
IC fabrication is explained here, 16 mask process
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/718f26ea-321d-4c94-a65c-d6d30cca09aa)

L8- Lab intro to Sky130 basic layers layout and LEF using inverter
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d107477b-754d-40f3-bc21-f0f80144badc)
selecting S multiple times will show the point connection
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0c65f0a5-3046-4e6b-bac5-310517ebe793)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a49f95f8-432d-46f5-94c9-7f9d247fcabd)
how to build an inverter from scratch is explained in https://github.com/nickson-jose/vsdstdcelldesign
LEF- Library exchange format, does not have information about logic part, only the metal layers, PR boundary (VDD line and GND line), LEF protects the IP of the vendor, LEF is aka, frameview
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d7243f5b-a94c-461f-9ae8-a52c884b7134)
the github link also explains step by step creation of an inverter in magic

L9- Lab steps to create std cells layout and extract spice netlist
tkcon window shows what are the tool DRC error, we need to make sure that final design is DRC=0
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/4d59ac2f-1376-45ff-b80b-50713a7f3bfc)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/258c5273-1cda-4c0f-af0e-ab710c447075)

to extract spice file from magic, go to tkcon window, and use "extract all"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/504ac88b-0f94-4b73-a0e8-36e71ca58618)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/34448cd3-4afb-47e7-a719-edb66a479594)
the command to extract parasitics will be "ext2spice cthresh 0 rthresh 0" & "ext2spice"to be executed in the tkcon window
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/2f6cb829-9beb-4be8-a347-a9b084d7fa6b)
the extracted spice file is shown below
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3dbf6a30-3683-40d7-9da3-158dbfc8efc8)

SK3 - Sky130 Tech file labs

L1- Labs steps to create final spice deck using sky130tech, L2- Lab steps to characterize inverter using sky130 model files
pmos has model name as pshort
nmos has model name as nshort
grid value which was specified in the layout is
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8e3783c3-f097-429f-aa23-e811ca3a5c86)
define pmos and nmos from lib file
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/38296049-4e1d-47f6-97d3-0a0fba19bf93)
to run the spice we go to the respective folder /Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/ and run the command "ngspice sky130_inv.spice"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/30265be9-0d69-4a27-bc53-4c209e1d6981)
we can plot the output using "plot y vs time a"
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/12829363-72b1-4c0b-97f2-b13dce7edb03)
the transition times are calculated from the plot
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3d82e807-2872-4de7-b62b-a259229aaa47)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/7c132afa-4dcb-43f3-a10b-bcbbd1804b6d)


L3- Lab introduction to magic tool options and DRC rules, L4- Lab introduction to sk130 pdks and steps to download labs, L5- Lab introduction to magic and steps to load sky130 tech rules, L6- Lab exercise to fix poly.9 error in sky130 tech file, L7-lab exercise to implement poly resistor spacing to diff and tap,L8-lab challenge exercise to describe DRC error as geometrical construct, L9

Magic DRC engine
Introduction to google/skywater pdk

opencircuitdesign.com/magic --> using magic --> using technology files. Tutorial 2 and Tutorial 6 are important. Technology section --> Cifout and DRC section.
Cif is nothing but Caltech intermediate format which is a human readable ASCI format, cif can be interchangably used with Tech files.

pdks from google that is open source is found in https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

github link for google sw pdk is https://github.com/google/skywater-pdk

we need to get some layouts which instructor is talking about, we can download it once we are in the home directory and using the command "wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz", once we download it, we can go into that directory by "cd drc_tests", the technology file is in the same folder in the drc_tests
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/09f15216-8d72-4c61-9002-5aa91e18a1c5)

"snap int" command to be used in the tkcon window to snap the box edges to smaller nm ranges
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/6d7a9164-c8a2-4f92-af92-91bfb56b2663)

loading poly.mag
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9f3ac97d-7b2b-4b2a-96ca-ded525fff4d3)
this had to show rules voilation but did not
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/ca929961-6dbc-42b0-bc12-8d09dbe7d490)
the rule can be added in the tech file by addition of poly voilation
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8c34dcdb-6e7e-4161-8e7d-15fd11d308be)

copy the 3 polyres and check the rules
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/33486f88-581b-40e7-964a-530aededc245)


Day 4

Day 5
