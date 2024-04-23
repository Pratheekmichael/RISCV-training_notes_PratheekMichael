# Digital VLSI SoC design and Planning, RISCV-training_notes_PratheekMichael
*Digital VLSI SoC design and Planning,RISCV training and lab details
*Note: All snapshots are taken from the workshop with permission given by Mr. Kunal Ghosh.

## Day 1 - Inception of open-source EDA, openlane and Sky130PDK

### D1_SK1 How to talk to computers

#### L1 Intro to QFN-48 package, chip, die, pad, core, IP.

- The take away is that the processor is the brain of the arduino or an FPGA board and it needs to communicate with the interfaces. The chip design is the main focus of the course, a package can be synonymously called as a chip, so when QFN-48 Quad Flat No lead package is spoken about, we can understand this lingo. the package pins can be decided by the application board, io chip enables communication with outside signal to the chip. IO pins are connected to pads.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/52501a88-d0ea-4c2d-b2d1-a598e531a845)
Foundry IP, is Foundry intellectual property which is protected by the respective foundry. VLSI ENGs needs to communicate with the foundry, to build the chip. Chip contains Macros and Foundry IP.

#### L2- RSIC V architecture

- Assembly lang program which is used to talk to computers. Code on computer is complied in RISCV assembly Language program which is converted to RTL which converts it to machine language which will be processed by the computer in order to test the layouts which are being designed. 
In short, 

``` RISC V architecture --> implementation (picorv32 cpu core) (RTL) --> layout (qflow) desing & test ```

#### L3- software apps to Hardware

```txt
Apps --> system software (OS (program lang)-->
compilter (RISC-V/ARM, ISA)--> assembler (converts to Machine language program)-->
Hardware (RISCV CPU core or ARM CPU)
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/c7b06a4e-a82d-43e6-a0a1-61e052cb3e7d)

### D1-SK2 - SoC Design and Openlane

#### L1 - Intro to components of open-source digital ASIC design

In 1979,
- Lynn Conway & Carver Mead seperated the design from technology and pioneered Structure design methodology which is based on Lambda design rules --> this led to Fabless company.
PDK = process design kit which comprises of DRC, LVS, device models, Standard cell libraries, IO libraries which can be used by EDA to model a fabrication process during IC design.
Google has made agreement with Skywater to release open source PDK on June 2020, i.e., sky 130nm process.
To understand why 130nm is exiting when we have reached less than 10nm tech, see the market share distribution.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3ffc1ffb-cbbb-47eb-bcbb-9e6131badfb6)
130nm is 6% which is almost $4bn market share.
ASIC flow objective is to take RTL (register transfer level) to GDSII, aka, Automated PnR or physical implementation.

#### L2 - Simplified RTL2GDS flow

``` RTL --> synthesis --> floor planning or power planning --> placement --> Clock tree synthesis (goal: min skew) --> routing --> signoff --> GDSII ```
 However, before signoff, we do 
```Physical verifications -- DRC, Layout vs schematic (LVS) checks and Timing verification (STA - static timing analysis)```

#### L3 - Introduction to openlane & strive chipsets

- Openlane is Open source ASIC design implementation flow. Goal of this is to produce a clean GDSII with no human intervention, that means no voilations in DRC, LVS, or Timing. Two modes of operation: automonomous and interactive. 

#### L4 - Intro to openlane detailed ASIC flow

```
Design RTL --> RTL synthesis --> STA --> Design for Test (DFT) -->  physical implementation (using operoad app)
 --> Logic equivalence check --> Detailed routing -->
if needed can deal with antenna rule voilation by insertion of fake antenna diode next to every cell input to check voilation
(if yes, a diode can be palced to mitigate this) (magic can be used) -->
STA, DRC & LVS (magic can be used for DRC and spice extraction)
```
### D1-SK3 Opensource EDA tools

#### L1 - Openlane directory structure

Ubuntu help
```bash 
ls --help
```
will list all commands and what do they do.

- `Sky130A --> libs.tech & libs.ref` which has files associated with technology and process respectively. we will be working with  sky130_fd_sc_hd file `{130nm, fd-skywater foundary, sc-standard cell, hd-high density}`

#### L2- Design prep step,

- The highest priority .tcl file is `sky130A_sky130_fd_sc_hd_config.tcl` which will overwrite the `config.tcl` settings, ex: clock period.

In the openlane directory, which is 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
docker
#docker apparently is a short for the command docker run -it -v $(pwd_:/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PRDK_ROOT -u $(id -u $USER):$(id -g $USER) openlane:rc2
(not verified as copied from video, version might be v0.21)

```
the next command is 
```tcl
./flow.tcl -interactive
```

once open lane launches, we import packages required to run this and the command is 
```tcl
package required openlane 0.9
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/186f215d-ad18-4148-87de-31c24bf7e069)
the next step is to prep design, to have design specific files generated in the `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` folder. the command for  this is 
```tcl
prep -design picorv32a
```

#### L3- Review files post design prep and run synthesis

- So in the `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` a new runs folder got created due to the design prep command we executed, our later synthesis file will be in this folder.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3304a0b9-a439-4f02-9fae-c3a5fb160e88)
now we can use the command
```tcl
run_synthesis
```
to perform the first step in the physical implementation.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0fd62705-190e-4159-a762-dc3effebbc53)
openalne efabless information is available on github at [github efabless link] https://github.com/efabless/openlane2

Once the synthesis is succesfully completed, we can use 
```tcl
exit
`to exit from openlane`
exit
`to exit the docker`
```
this will ensure nothing is active in the background.


#### L4-openlane project link 

- To install the openlane in ubuntu or VB from scratch, we first install git
  ```bash
  sudo apt install git
  ```
  and use the git clone
  ```bash
  [url](https://github.com/efabless/OpenLane.git)
  ```
command to copy over the openlane files from the github link, [github efabless link] https://github.com/efabless/openlane

Open lane flow is shown in the greyed box of this flow
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/52a2c902-4b57-4e18-9708-00a56b575cd2)

command structure has to follow a flow, ` synthesis-->floorplan-->placement-->cts-->routing `, if this is not followed the flow will fail to generate output. 

A fully automation of these commands can be done by the command 
```tcl
./flow.tcl -design picorv32a
```
the `right part-picorv32a` of the command is the design name.

On youtube, it is recommended to watch the `fossi-dialup` videos by `Tim Ansell`, and `Mohamed Shalan` which is regarding Skywater PDK and Open lane respectively.

#### L5: Steps to characterize the synthesis results

The task is to find the 
```math 
Flop\ ratio = \frac{Number\ of\ D\ flip\ flops}{\ Number\ of\ cells}
```

In the synthesis which was run the ratio is 10.84%.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0a3e46ac-d981-4db9-9cf6-96b04c8f7d0b)
The results folder will have the synthesized checklist
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8070899f-678d-4821-8c18-efc125cfe2df)
The synthesis statistic report gives us the info about the flop ratio again.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/4a3402eb-878f-4fba-ac5e-0533096ccf0d)

## Day 2 - Good floorplan vs bad floorplan and introduction to library cells

### Sk1 - chip floor planning considerations

#### L1 - Utilization factor and aspect ratio

Dimension of the standard cells are the feature which are of interest, to find the total area of the standard cells inside the netlist. A silicon wafer will comprise of multiple die, a die in turn has a core which has the logic built on it. 
```math
Utilization\ factor\ UF = \frac{area\ occupied\ by\ the\ netlist}{area\ of\ the\ core}
```
If `UF=1` then core is completely utilized and additional logic cannot be necessary. 
```math
Aspect\ Ratio = \frac{Height\ of\ the\ die}{width\ of\ die}
```
if `AR 1`, then its a square. example `width of core is 4 unit and length is 2 unit, UF=0.5, AR=0.5`

#### L2 - Concept of preplaced cells.

Preplaced cells, a combinational logic output can be implemented in the form of gates, split the gates into blocks,now the io for these blocks need to be connected to regenerate the combination logic, example for this is memory, clock gating cells, comparator, MUX, these are called preplaced cells. These IPs are palced in user defined location prior to the automated PNR flow, hence they are called preplaced cells. The automated PNR tool cannot move preplaced cells, the PNR tool has to go around these cells to do its function.

#### L3 - De-coupling capacitors

Why do we need Decoup capacitor? When a gate switches, the supply voltage needs to provide the charge required, the VDD needs to provide this, VSS needs to take in the discharge current. The VDD can have multiple voltage drop due to practical factors like resistance and inductance, therefore, VDD will now have diminished to VDD' which is not the same as VDD. if the VDD' is in the undefined region of the noise margin, then there is a problem for the circuit to design what charge it is getting. Therefore, we add decoupling capacitor to decouple the the circuit from the power supply, this will avoid voltage drop. After the decoup is added, when there is switching activity the decoup cap provides the charge and will replinsh itself when the activity is not done. Therefore, decoupling capacitor is placed in between preplaced cells.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/e7fd48f7-936d-4887-83ec-6d72d49568a6)

#### L4 - Power planning

Macros needing power which is tapped from VDD. When a 16 bit bus discharges all at once, there is a ground bounce. When these 16 bit bus caps require to charge at once, then it will need power all at once, which will cause voltage droop. Therefore, instead of one single power supply point, we can setup multiple power supply pins or points which is similar to,
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a0547956-feff-4cc4-b791-317dff2b15e7)

#### L5 - Pin placement and logical cell palcement

- `Netlist` - connectivity information between different gates which is defined in verilog HDL. The pins are placed in the die area outside the core and it will be fixed in this place. Frontend team - netlist connectivity and backend team - pin placement, therefore a proper design knowledge is needed between both teams. Clk ports are a bit thicker, to get least resistance. A logical cell blockage is done in order to make sure once the io pins are placed, nothing is placed in that area by the PNR s/w.

#### L6 - steps to run floorplan using OPENLANE

After synthesis the next step is the flooplan. 
`PNR flow` is an `iterative flow` which needs `certain parameters` to be `active` at `certain steps`, therfore it is the `ENGs decision` to make use of `appropriate parameters`. 
In `Desktop/work/tools/openlane_working_dir/openlane/configuration` folder there is a `readme.md` which will explain about all the parameters.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f23bd0ad-55fc-4d1a-9a4f-204ecc51df21)
The `.tcl` priority for `floorplan.tcl` (system default) will be the least and will be superceeded design by `config.tcl` and the `sky130A_sky130_fd_sc_hd_config.tcl`

The next step is the run the floorplan, in the openlane directory, which is 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
we invoke the command 
```tcl
docker
```
The next command is 
```tcl
./flow.tcl -interactive
```
once Open lane launches, we import packages required to run this and the command is 
```tcl
package required openlane 0.9
```
The next step is to prep design, to have design specific files generated in the `Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a` folder. the command for  this is 
```tcl
prep -design picorv32a
run_synthesis
run_floorplan
```

![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f6bbbed9-5f6d-49f9-a435-ced986510fb8)

#### L7, L8- REview flooplan layout in Magic

- the `flooplan.def `(design exchange format) is populated in the results folder.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9a677e8d-0235-4231-a339-fcc8ca986396)
To see actual layout in magic, go to the respective directory
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_23-16/results/floorplan/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &" to get the layout after floorplan sim
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/86b5adc4-b228-4e3a-b9c2-e0fca66dbece)
to zoom, use the left and right click of the mouse to define a box and z to zoom in and to select a cell use s after highlighting on the cell use the command 
```bash
what
```
in the tkcon window to see what is the selected cell.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9542f146-da54-4783-8ded-c04e05233449)

### SK2- Library Binding and placement

#### L1 - Netlist binding and initial place design
In theory the AND gate may look different than when it is designed in layout, the layout has dimensions which can be used by ENGs. These netlist blocks can be found in `Standard cell library`, the library will also have various falvors of each cell, i.e., bigger dimensions of the same cells, these bigger cells might improve the design at the cost of the area utilized.
Placement will be with respect the input and output pins and also the netlist priority i.e., how much loss can be afforded when cells are have distance between them.

#### L2- Optimize placement using estimated wire-length and capacitance
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/61900166-e631-443f-922c-2cba3cf8e41f)
The wire that is needed to connect these cells will have Cap associated with them, the slew gets implemented because of this, therefore repeaters are needed to maintain signal integrity, ENG needs to decide on how many repeaters/buffers are needed. 

#### L3- final placement optimization, discusses the buffer placement by estimated wire length

#### L4- Need for libraries and characterization

`Logic synthesis --> import netlist of LS and do --> floorplanning --> placement (optimization of netlist placemenent) --> clock tree synthesis --> routing stage (maze routing) --> Static timing analysis (setup time and hold time) --> sign off`
Collection of gates are called library.

#### L5 - Congestion aware placement using replace

When run placement command is executed, a global placement happens, which has HPWL half parameter wirelength which needs to be optimized.
the next command in the flow will be 
```tcl
run_placement
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/28a38c0e-bb50-4e58-8047-8d332bdf20e3)
In order to load the gnerated file go to the directory 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-04_01-39/results/placement/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
to view layout post placement
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d6ef537a-1b57-4ad3-ab8e-896905df9fa4)
Placed cells view
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/cc183e4d-91ac-4a06-a451-bbe56b3faf45)
we have to exit the openlane and the docker view in order to avoid acitivity.

### SK3 Cell design and characterization

#### L1- Inputs for cell design flow, L2- circuit design step, L3 - Layout design design step, L4-typical characterization flow
Library is a place where we keep all standard cells, which are buff, FF, AND, OR gates, these cells can be of varying dimesions which will vary the cells drive strength. The dimesions will cause the keep parameter changes, example, threshold change.

The instructors has courses on (1) `circuit design/analysis and spice simulation`, (2) `custom layout has more information about the cell design flow`.
A rough idea for Cell design flow has `(1) inputs: PDKS, DRC, LVS, library and user defined specs, SPICE model, (ENG: choose proper library cell)` --> `(2) Design steps: Circuit design, layout design, characterization (output of circuit design is circuit description language, output of layout design is GDS)), LEF, extracted spice netlist)` --> `(3) Ouput (given in the paranthesis of previous step)`
`GUNA` software generated timing, noise, power.libs

### SK4 General timing characterization paramters

#### L1 - Timing threshold definition

`Slew low rise threshold: 20% of the max voltage`
`slew high rise threshold: 80% of the max voltage`
similar terminology exists for the falling waveform or output waveform
`50% input waveform - 50% ouput waveform ==> rise delay`

#### L2 - Propagation delay and transition time

Choice of threshold can avoid negative delays, negative delays can also be caused by wire delays.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d07afa91-8061-465d-9e3d-724166fdf0a2)

## Day 3 - Design Library cell using Magic layout and ngspice characterization

### SK1 - Labs for CMOS inverter ngspice simulation

#### L1- SPICE deck creation for CMOS inverter, L2- SPICE simiulation lab for CMOS inverter

Spice deck is connectivity information about the net list, we need component values, `W/L` of CMOS, `Capacitance` of the load, `VSS`, `VDD`, the next step is to identify the nodes and name them.
* is comments
  ```M1 out in vdd vdd pmos W=xu L=yu```--> `M1 is between out and in nodes and VDD and VDD`
 ![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/44e52652-02fc-4294-a014-41e409669fc5)
`.Lib "tmsc_025um_model.mod: CMOS_MODELS`

`W/L for pmos > W/L for nmos for ideal situation`

#### L3 - Switching threshold Vm, L4- Static and dynamic simulation of CMOS inverter

CMOS is a very robust device, even though we changed the `Wp=2.5xWn`, the waveform is very similar to wp=wn and the only differece will be in the `switching threshold`.
Vm is the point where Vin=Vout, we basically draw a diag line on the output of the DC result and find the diag line intersection with waveform
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/7f6ee0c4-de6e-4f63-8a90-9750c7311d6f)
Rise and fall delay can be calculated by peforming transient analysis which will be done in the labs

#### L5 - lab steps to git clone vsdcelldesign

go to the directory 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
clone the repository, using the command 
```bash
git clone https://github.com/nickson-jose/vsdstdcelldesign
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/bbdb1086-8768-4e45-91e5-933d1b85736f)
go to the vsdstdcelldesign folder by using 
```bash
cd vsdstdcelldesign
```
to copy the tech file, go to 
```bash
cd Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic
```
and use the command 
```bash
cp sky130A.tech /Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/cf934a22-771f-41ad-b405-c2337c7f6c7c)

### Sk2- inception of layout of CMOS fabrication process
#### L1- create active regions, L2-formation of N and P well, L3- formation of gate terminal, L4 - Lightly doped drain, L5- source and drain formation, L7- higher level metal formation

IC fabrication is explained here, 16 mask process
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/718f26ea-321d-4c94-a65c-d6d30cca09aa)

#### L8- Lab intro to Sky130 basic layers layout and LEF using inverter

![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d107477b-754d-40f3-bc21-f0f80144badc)
selecting `S` multiple times will show the point connection
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0c65f0a5-3046-4e6b-bac5-310517ebe793)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a49f95f8-432d-46f5-94c9-7f9d247fcabd)
how to build an inverter from scratch is explained in https://github.com/nickson-jose/vsdstdcelldesign
`LEF- Library exchange format`, does not have information about logic part, only the metal layers, PR boundary (VDD line and GND line), LEF protects the IP of the vendor, `LEF is aka, frameview`
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d7243f5b-a94c-461f-9ae8-a52c884b7134)
the github link also explains step by step creation of an inverter in magic

#### L9- Lab steps to create std cells layout and extract spice netlist

tkcon window shows what are the tool DRC error, we need to make sure that final design is `DRC=0`
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/4d59ac2f-1376-45ff-b80b-50713a7f3bfc)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/258c5273-1cda-4c0f-af0e-ab710c447075)

to extract spice file from magic, go to tkcon window, and use 
```tcl
extract all
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/504ac88b-0f94-4b73-a0e8-36e71ca58618)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/34448cd3-4afb-47e7-a719-edb66a479594)
the command to extract parasitics will be 
```tcl
ext2spice cthresh 0 rthresh 0
ext2spice
```
to be executed in the tkcon window
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/2f6cb829-9beb-4be8-a347-a9b084d7fa6b)
the extracted spice file is shown below
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3dbf6a30-3683-40d7-9da3-158dbfc8efc8)

### SK3 - Sky130 Tech file labs

#### L1- Labs steps to create final spice deck using sky130tech, L2- Lab steps to characterize inverter using sky130 model files

pmos has model name as pshort
nmos has model name as nshort
grid value which was specified in the layout is
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8e3783c3-f097-429f-aa23-e811ca3a5c86)
define pmos and nmos from lib file
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/38296049-4e1d-47f6-97d3-0a0fba19bf93)
to run the spice we go to the respective folder ```/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/``` and run the command 
```bash
ngspice sky130_inv.spice
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/30265be9-0d69-4a27-bc53-4c209e1d6981)
we can plot the output using 
```bash
plot y vs time a
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/12829363-72b1-4c0b-97f2-b13dce7edb03)
the transition times are calculated from the plot
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/3d82e807-2872-4de7-b62b-a259229aaa47)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/7c132afa-4dcb-43f3-a10b-bcbbd1804b6d)


#### L3- Lab introduction to magic tool options and DRC rules, L4- Lab introduction to sk130 pdks and steps to download labs, L5- Lab introduction to magic and steps to load sky130 tech rules, L6- Lab exercise to fix poly.9 error in sky130 tech file, L7-lab exercise to implement poly resistor spacing to diff and tap,L8-lab challenge exercise to describe DRC error as geometrical construct, L9

Magic DRC engine
Introduction to google/skywater pdk

[magic link] opencircuitdesign.com/magic --> using magic --> using technology files. Tutorial 2 and Tutorial 6 are important. Technology section --> Cifout and DRC section.
Cif is nothing but Caltech intermediate format which is a human readable ASCI format, cif can be interchangably used with Tech files.

pdks from google that is open source is found in [skywater pdk link] https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

github link for google sw pdk is [skywater pdk github link] https://github.com/google/skywater-pdk

we need to get some layouts which instructor is talking about, we can download it once we are in the home directory and using the command 
```bash
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
once we download it, we can go into that directory by 
```bash
cd drc_tests
```
the technology file is in the same folder in the drc_tests
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/09f15216-8d72-4c61-9002-5aa91e18a1c5)

```tcl
snap int
```
command to be used in the tkcon window to snap the box edges to smaller nm ranges
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/6d7a9164-c8a2-4f92-af92-91bfb56b2663)

```tcl
loading poly.mag
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9f3ac97d-7b2b-4b2a-96ca-ded525fff4d3)
this had to show rules voilation but did not
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/ca929961-6dbc-42b0-bc12-8d09dbe7d490)
the rule can be added in the tech file by addition of poly voilation
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8c34dcdb-6e7e-4161-8e7d-15fd11d308be)

copy the 3 polyres and check the rules
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/33486f88-581b-40e7-964a-530aededc245)

## Day 4 - Pre layout timing analysis and importance of good clock tree

### Sk1- Timing modelling using delay tables

#### L1- lab steps to convert grid info to track info, L2 - Lab steps to convert magic layout to std cell LEF, L3 - Introduction to timing libs and steps to include new cell in synthesis

We have done design `setup, synthesis, floorplan, extracted spice model`
2 rules to follow,
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/2a44b14e-0979-48fd-964a-966b52152882)
go to folder ```Desktop/work/tools/openlane_working_dir/openlane/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/```
if we see the file ```tracks.info```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/1e35ee11-6705-4e40-93fe-9a8a10c84547)
tracks are used for routing stages, routes can go over the tracks. we need to see that the inverter layout pins are on the tracks definted, for that we have to change the grid spacing to the one shown in the track info, and check if the pins are on the intersection,
in the tkcon window, use 
```tcl
grid 0.46um 0.34um 0.23um 0.17um
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/0bc1c8b0-0945-4bc2-b6ff-5f25b31328be)
the image shows that the input and output pins of the inverter are on the horizontal and vertical intersection of the grid which was spaced per tracks info.
the width of the inverter is in odd multiple of the x pitch, from the image it shows 3 boxes.
Port definition needs to be done next, did not implement because its already done by the instructor, instruction is given in his github link
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f3edae8a-c092-42db-bd0c-ed7750114553)
Next is to set the port class and port use attribute for the layout, select the port, S, in this example, select output Y, and check what is it connected to by using the ```what``` command in the tkcon window, use the command ```port class output``` ```port use signal``` to denote it as output for signal, similarly we do it for input port.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a96be4ab-ceb0-4ddc-9114-6339d8f3dcc4)
we use the command 
```tcl
save sky130_vsdinv.mag
```
to generate the lef file, we launch magic 
```tcl
magic -T sky130A.tech sky130_vsdinv.mag &
lef write
```
in the tkcon window
copy the lef to picorv32a folder, the basic idea is to add our custom cell to the openlane flow
```bash 
cp sky130_vsdinv.lef /Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```
copy the library files
```bash 
cp libs/sky130_fd_sc_hd__* /Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/
```

we need to modify the config.tcl
```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib
set ::env(EXTRA_LEFS) {glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/d49acc09-228e-4962-81d1-82892c911dba)
we need to go to the openlane directory, 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane
```
we invoke the docker, 
```tcl
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/dfea9a28-3a63-4557-b0d0-b1c6180c93c8)

#### L4- Introduction to delay tables L5- Delay table usage part 1, L6- Delay table usage part 2

Different sized buffer has different delay tables, whose values are characterized and can be extrapolated.
Observations: 1) 2 levels of buffering, 2) at every level, each node should drive same load 3) every level should have identical buffer.

#### L7- Lab steps to configure synthesis settings to fix slack and include vsdinv

![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/878b6bc7-b0b4-456f-b93c-0d89655af640)
when we look at `merged.lef` folder, we can see that the custom inverter cell was placed in the design
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/aa5f0980-bb0c-4346-9754-f081922ecd73)
reran synthesis with ```set ::env(SYNTH_STRATEGY) "DELAY 3"``` & ```set ::env(SYNTH_SIZING) 1```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9fe16df4-4b33-4ea7-bd21-f83139de71c8)
Since ```run_floorlpan``` gave error, followed the following command which is equivalent, 
```tcl
init_floorplab
place_io
tap_decap_or
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/6907689e-4a4f-45b4-a6ac-da757246cf95)
next is to 
```tcl
run_placement
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/04db3166-0c67-48e5-a202-8085f5f8d926)
when I attempted to open magic and look at the placement def file, i got error so I reran the steps above
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/dff2fa52-53de-4c6c-aeac-8a2ac3f622df)
going to the 
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-04_16-20/results/placement
```
opening up magic using 
```bash
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/51a2706b-ff26-49c2-92b1-a5db0227be86)
we can use the command expand in the tkcon window and see the LEF, this shows that the metal 1 line from the library and the custom inverter cell is aligned correctly
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9e0b8066-5e9b-44a2-a5ab-ad064d307f60)
another view
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/8ef0ee59-4c73-4306-aba8-6610a9e3747a)

### SK2- timing analysis with ideal clocks using open STA
#### L1-setup timing analysis and introduction to flip flop setup time, L2-Introduction to clock jitter and uncertainity

Combinational delay < (T- setup time)
jitter: temporary variation of clock period, adding uncertainity to the above equation
combinational delay <(T- setup time -setup uncertainity)
Combinational delay will involves the cell delay+wire length delay as well

#### L3- Lab steps to configure open STA for post -synth timing analysis, L4-lab steps to optimize synthesis to reduce setup voilation, L5- Lab steps to do basic timing ECO

Defining the `pre_sta.conf` (picture collected post simulation)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/504d0626-86fc-446d-9481-c233cbfe7acc)
`my_base.sdc` definition in the folder ``Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src``
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f6bbedfe-917e-4964-8a6d-79e5e6fa7c6a)
if we run the sta analysis from the folder openlane, 
```bash
sta pre_sta.conf
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f145cbff-b1e9-478c-9fa4-a5ae4897478d)
we got a voilated slack
Delay of any cell =Fn(input slew, output capacitance),
We can try to optimize out fan out value
In the openlane flow, we set, 
```tcl
set ::env(SYNTH_MAX_FANOUT) 4
run_synthesis
*Note PNR is an iterative flow as long as you dont exit the flow you can iterate again with new values
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9c8a6649-0a42-48d4-9e26-474ec085b63d)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/321598ff-889e-4741-b06b-4d7251b773e0)
trying to see if replacing a particular gate cell will reduce the slack
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/2719f8a6-21a5-437c-ab7d-1515ad1fc122)
slack seemed to have reduced slightly
attempted few more iterations
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a1563f18-fb8a-4dbb-92d2-a4b22ae99835)
Using the delay 3 will reduce the tns and wns and avoid slack

### SK3 - CTS triton CTS and signal integrity
#### L1 - Clock tree routing and buffering using H tree algorithm, L2-cross talk and clock net shielding

we expect skew ~0 or close to 0
H tree uses a midpoint strategy and splits, in order to give the timing requirement for the flops at the same time
Since the clock is signal is provided by wires, we need to add buffers, for signal integrity,
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/48fcbc30-b70f-4596-8a68-065335f4b5ef)
Even though we add buffer, we need shielding to protect the integrity of these clock nets, therefore we need clock net shielding, this is to avoid glitch and delta delay. Giltch can reset memory chip therefore need to be addressed. we have to shield couple of other wiring which are important as well in over to prevent the glitch or cross talk
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/15fb841b-deb7-435d-ba05-9fb854976984)

#### L3 lab steps to run CTS using Triton CTS L4 Lab steps to verify CTS runs

We can save our old verilog file and make a copy of it,
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/2916069d-b565-4e90-bb67-571914232aea)
we can 
```bash
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/20-04_16-58/results/synthesis/picorv32a.synthesis.v
```
As the previous design had better numbers we can use the old settings with SYNTH_STRATEGY having DELAY 3 and, in order to overwrite the result in the present folder, we can always use the command 
```tcl
prep -design picorv32a -tag 20-04_16-58 -overwrite
```
and use the lefs, rerun synthesis, rerun floorplan, and run CTS
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/09536521-5558-4cec-9278-7a3d0173bb4f)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/9f65d826-c7c3-4749-b40a-09bb1f653bd3)

### SK3- Timing analysis with real clocks
#### L1-setup timing analysis using real clock, L2-Hold time analysis using real clocks

combinational delay + launch flop clock network delay < T + capture flop clock network delay - setup time - uncertainity
right hand side of the equation is data required time, left side is data arrival time, subtraction of these will give slack, which is expected to be 0 or positive

combinational delay > hold delay (MUX2 delay)

combinational delay + launch flop delay > Hold delay + capture flop delay + hold uncertainity

left hand side of the equation is data arrival time and right hand side is data required time, slack should be 0 or positive

#### L3 lab steps to analyze timing with real clocks using open STA, L4- lab steps to execute openSTA with right timing libraries and CTS assignment, L5- lab steps to observe impact of bigger CTS buffer on setup and hold timing

once in openlane folder, we can run 
```tcl
openroad
# we have to read the lef,
read_lef /openLANE_flow/designs/picorv32a/runs/20-04_16-58/tmp/merged.lef
#we have to read the def file,
read_def /openLANE_flow/designs/picorv32a/runs/20-04_16-58/results/cts/picorv32a.cts.def
#we create a db,
write_db pico_cts.db
#we read the db,
read_db pico_cts.db
#we read the verilog netlist,
read_verilog /openLANE_flow/designs/picorv32a/runs/20-04_16-58/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
#to the library
#link the sdc created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path _delay min_max -format full_clock_expanded -digits 4
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/85868157-481b-4a9e-8d83-463b460f064f)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a74ff9d0-9c1b-45d8-9b4f-a6ef1e9fd23f)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/523fdcd8-d049-41ab-93c0-a06ef564fb72)
insturctor informed that triton CTS is analyzing for min and max columns instead of a simple column. we can rerun the result by removing buffer 1
```tcl
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/220-04_16-58/results/placement/picorv32a.placement.def
```
use the read lef and def from above, write the database to a new file and read it, and read the verilog file, repeat the steps from above
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/b6f83eb6-7186-4a76-ab52-ac4dc9cbc8a1)
reinsert `clock buff 1`
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/00d10064-b02e-4f65-9197-70754512cdb8)

## Day 5- Final steps for RTL2GDS using TritonRoute and openSTA
### SK1-Routing and design
#### L1-into to maze routing, Lees algorithm,L2- continuation of L1, L3-DRC

best route without lot of bends are chosen
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f1f933a5-a25a-4ae7-8983-4cfe6f0b9d2d)
DRC has main limitation from Photolithography,therefore the wire width and pitch should be defined by PL, to avoid shorts we can take metals at different levels.
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/4b0d2839-6bdc-4b46-ad4f-01c5f433697d)
with new metal layer we have to add via to connect the metal lines
Parasitic extraction is the resistance and capacitance extraction out of the netlist
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/60f73183-a767-4ceb-a2e7-65fccf0c51c1)

### SK2- power distribution network and routing
#### L1- lab to build power distribution network, L2-lab steps from power straps to std cell power, L3-basics of global and detailed routing and configure triton route

![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/00bf5453-1c37-49b5-a849-3e75dd683113)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/6f0cfa77-9f0e-45e3-a7bb-8f72b10373cb)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/bfb79771-869f-4106-9b41-b398c163d219)
what is the last step of placement?
``echo $::env(CURRENT_DEF)`` will give the value of the last step
use command, 
```tcl
gen_pdn
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/701b4cc7-d9f9-4648-bf86-869fdcf72d44)
strap explaination
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/c98ffd1f-d479-4cff-89f6-fe39226c3daa)
we can view the layout after this step and see the staps placed
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/a3cfd0c9-af50-4a97-a1c8-759f50043cf1)
use the command, 
```tcl
run_routing
```
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f03cb3fa-d71f-489d-b528-f7a46ed2847d)
routing takes the `longest time` to complete
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/f6c16719-651a-4146-b49b-04a42eb33de5)

### SK3-Triton route
#### L1, features, preprocessed route guide, L2, interguide connectivity and intra and inter layer routing, L3 triton route method to handle connectivity,L4 routing topology algorithm and final files list post route

Triton route needs `LEF, DEF< preprocessed route guides`, it outputs detailed routing solution with optimized wire length and via count, the constraints are route guide honoring connecitivy constraints and design rules
once the routing has been done we can see the layout using magic, from the latest results file after routing, and launching magic
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/5864cbe9-f0ae-49c5-a5f3-17e7b8fb77c2)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/1feacf8b-bbe5-45b2-b45d-0479ea4b3c9f)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/e1d44d9d-7503-426e-aabd-17a7092fc78e)
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/e4052912-92e8-497f-9551-b5b36169c918)

I did not have the spef extractor to tried to download and run the command to extract the files, however, it failed
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/c0eb9559-ab36-455a-bb3a-3933b5c9a497)
I was informed that after the pdn we get the spef, so i used that
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/add870a4-7216-4e39-bd2b-41532a752147)
and used that to perform the post route STA analysis and got the following slack (MET:))
![image](https://github.com/Pratheekmichael/RISCV-training_notes_PratheekMichael/assets/166673625/72a4b11d-418d-4bd0-8f95-7ed3a3752a17)


# POST WORKSHOP ACKNOWLEDGMENT
- I want to thank the instructors, Mr.Kunal Ghosh, Mr.Nickson Jose, Mr.Mohamad Shalan & Mr.Tim Edwards for conveying this valuable information in this workshop.
