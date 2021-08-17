# VSD-Physical-Verification
## **This report details the learning in 5-day Physical Verification workshop by VSD from Aug-11 2021 to Aug-15 2021 with self-made videos and images.**

![image](https://user-images.githubusercontent.com/88816771/129772417-2e48ab37-a17d-42e3-bd6d-db5baa56c0ec.png)


## About the Workshop 
The workshop details the process of Physical verification like DRC and LVS checks during a RTL to GDSII flow using Skywater 130nm Technology. This is very useful for chip fabricatioln, and this lets the participants get ready for the tape out.
The workshop helps in identifying the violatiolns during the physical verification step and rectify the same. The workshop focuses in using open lane intended for physical verification like Magic, Netgen, ngspice, Xschem., etc.,

**Organized by: Kunal Gosh, Co-founder, VSD Corp. Pvt. Ltd.**

**Instructor: Tim Edwards, works for Efabless and a open tools developer**

## Overview
- **Day 1 - Introduction to Skywater PDK**
  - Basics to know
  - Installation
  - Tools
  - Sky130nm Libraries
  - Sky130nm Layers
  - Quick start-up
  
- **Day 2 - Introduction to DRC and LVS**
  - GDS read
  - Understanding Ports
  - Abstaract view
  - Extraction
  - DRC Style
  - LVS
  - XOR comparison
- **Day 3 - Design Rule Checking (DRC)**
  - Basic rules
  - Via rules
  - Minimum area and hole rules
  - Derived layers
  - Parameterized devices
  - Miscellaneous errors
  - Antenna Checks
  - Challenges
- **Day 4 - OpenLane Flow**
  - OpenLane/OpenRoad Automation
  - OpenLane flow - non-interactive
  - OpenLane flow - interactive
  - Common DRC Errors
  - violations - Fixing manually
  
- **Day 5 - LVS**
  - Basics
  - LVS - sub-circuits
  - LVS - Blackbox subcircuits
  - LVS - SPICE low-level components
  - LVS - small Analog blocks
  - LVS - Analog Devices - Power on reset
  - LVS - verilog netlists
  - LVS - Macros
  - LVS - Digital PLL
  - LVS - Property errors

# Day - 1 Introduction to Skywater PDK

## Basics
Mosis Foundation, California made the scalable CMOS rules available in 1980s. Skywater 130nm SCMOS is a fully open source, making all the design rules, device definitions, etc., public. 

The caravel Chip has a pad frame, RISC-V processor and large space for the user to append the design. The design is verified and fabricated by Efabless foundry partnering with Google for free of cost. 

- PDK refers to Process Design Kit
- SKY130nm refers to Skywater technology with a feature size of 130nm which is the minimum length of the transitor.
- Public Repository
  - Documentation
    https://skywater-pdk--136.org.readthedocs.build
  - PDK Library and files
    https://github.com/google/skywater-pdk
  - Community - slack
    https://join.skywater.tools

## Installing Sky130 open-PDK
Following are the steps to install PDK:
1. Clone the repository https://github.com/RTimothyEdwards/open_pdks
2. Run **cd open_pdks**
3. Run configure **--enable-sky130-pdk**
4. Run **make**
5. Run **sudo make install**

## Tools

![image](https://user-images.githubusercontent.com/88816771/129223330-1e44a475-731f-4dfe-a59d-b5a00c131f0d.png)

- Magic
  - Handles GDS DEF and LEF, Does extraction and layouts are created and managed.
  - Mainly used to check DRCs and the violations can be fixed by following the design rules.
  - The layout is drawn based on the parameters used.

To check for the functionality of the tool:
Type Magic
  
![image](https://user-images.githubusercontent.com/88816771/129247063-f92382b9-14db-4acc-b247-ca19b513f4de.png)

Two windows pop-up 
  - tkcon console
  - Layout window

- Klayout - Alternative tool for DRC checks

- Openlane - A flow that supports the OpenRoad tools. This can be used non-interactively by calling individual tools or ./flow.tcl can be used to run the scripts interactively for RTL to GDSII

- Xschem - Third repository for Schematic Editor that is used to create new schematics or can be used to import the schematics and generate nelists out of that. Xschem has symbol libraries to create schematics. It supports both ngspice for analog simulation and gaw for waveform viewing

Type Xschem - schematic window pops-up

![image](https://user-images.githubusercontent.com/88816771/129248019-f0acc516-1e9c-40d4-b804-35be6e71b69e.png)

- Netgen - LVS tool that compares the layout design with its schematic between netlists to verify the geometries

Type netgen - tkcon window opens

![image](https://user-images.githubusercontent.com/88816771/129247949-39752797-bbd7-4f2c-ab63-01f55d5d895e.png)

- Ngspice - Spice simulator, used to study the characteristics of the design

Type ngspice - opens in command prompt 

![image](https://user-images.githubusercontent.com/88816771/129248264-b6b7caec-d05b-48a2-a55b-82e89929556e.png)

Running the tools without console will be like:

![image](https://user-images.githubusercontent.com/88816771/129249002-bc6b2770-6a03-4275-b301-efcf74d41056.png)

## Sky130 Libraries

![image](https://user-images.githubusercontent.com/88816771/129225883-b5825306-a3db-4a1b-a925-3b3c79dbec7b.png)

### Digital standard cell 
contains Standard cell's Layout in GDS form, Liberty timing files, verilog files and netlists
This is how the libraries are named:
sky130_vendor_library-type[_name]
fd stands for foundry
sc stands for standard cells
hd stands for high density_
Example: sky130_fd_sc_hd__nor2_2 the type name is separated by "double underscore"

### I/O cells
- These cells are used as pad frames. It's readily available in Caravel.
- sky130_fd_io
- fd stands for foundry
- I/O stands for input/output

### Primitive devices and models
- Its provided by the foundry that includes transistors, varactors, ESD devices, array of various parallel plate capacitors. 
- Some of the other libraries are SRAM and NVRAM that are under construction.

### Some of the devices are bipolar PNP, bipolar NPN, diffusion resistors, polysilicon resistors, pwell resistors, and etc.,
- The devices have discrete widths ranging from 5.73um to 0.35um.
- Layouts are used for tapeouts.
- There are also hidden layers that the user may not know. 

## Sky130nm Layers
- Min length of FET is 150nm
- 5 Layers of metals
  - Local interconnect connects to neighboring nodes or to metal 1 and not for routing
  - Poly contacts have nitride poly cut layer (NPC) around them
 - Front-end - Metal layers and vias for Diffusion and Ion implantation
 - Back-end - Deposition, MiM cap layers
   - MiM - Metal insulator Metal (Metal plates between two metal layers)
 - Redistribution Layer: Copper is deposited on the fabricated chip and solder bumps are added on top the Redistribution layer. No wire bonding is required.

## Quick Start-ups
### Creating Device Layout
To start with a sample design, say, inverter:
1. make a new directory named inverter
2. Make sub directories: magic, netgen, xschem
3. create shortcut files that supports sky130A technology following the below picture

 ![image](https://user-images.githubusercontent.com/88816771/129250402-e60424e7-1fa5-4ee9-88dc-9d6fe9fb23fd.png)

Now, check the tool's compatibility with sky130

1. xschem should look like:

![image](https://user-images.githubusercontent.com/88816771/129251150-fb357077-4d0d-42e2-a153-dc7463bd1f66.png)

Select a device and press e for enter, to get back, press ctrl+e and exit

2. Magic should look like:

![image](https://user-images.githubusercontent.com/88816771/129251547-683d4be8-5493-4056-a094-347d9c5f7f91.png)

Check the technology and it should be sky130A

**magic -d XR** makes the icon colors more saturated.

![image](https://user-images.githubusercontent.com/88816771/129251860-50ed5b9c-d3d2-4fc1-91cb-5354eff7eb42.png)

3. Into magic

- select the area using key i
- select the color and press 'p', called painting
- select the painted area, select the color and press e to erase
- Adjust the selection box by left and right mouse clicks
- devices can be selected from the device 1 and device 2 tabs
- The parameters can be modified based on the requirement
- The device parameters can be customized using ctrl+p

https://user-images.githubusercontent.com/88816771/129264205-642dd086-2d22-4702-a883-4c160a8352e3.mp4

### Creating schematic in xschem

cd ../xschem
The tool opens

File -> new schematic 
Press insert to choose the symbols from different libraries

Choose primitives from pdk
Choose devices from xschem library
Below video explains in detail

https://user-images.githubusercontent.com/88816771/129267261-84c6aaec-ab16-481a-9887-8b5c73272b86.mp4

Keys to use: 
- "insert" for choosing the symbols from library
- "shift+zoom" for zoom in
- "control+zoom" for zoom out
- "q" for changing the parameters
- "c" for copy
- "m" for move
- "w" for wire


### Schematic to symbol

- make schematic to symbol by following the video
- The voltage sources are drawn from devices
- Make sure to add the "code-shown" from the devices
- Add in,out and gnd pins
- Finally save and exit

https://user-images.githubusercontent.com/88816771/129769082-4c85521f-1b77-4a16-8e30-2df6bd6bb20f.mp4


### Simulating the testbench
The test bench can be simulated and the below is a hint to check for the characteristics fo the deivice

![image](https://user-images.githubusercontent.com/88816771/129381480-00059ed6-40fe-48bb-9d8e-34901b00da06.png)


# Day - 2 Introduction to DRC and LVS

## GDS read
- Create a new project directory
- create a magic directory and copy the sky130 tech files
- open magic and understand different cif styles
  - *cif listall istyle* This lists all the available styles
  - *cif list istyle* This lists the current style
  - *cif istyle random* This lists the style random
  - *cif istyle sky130(vendor) is set*

![image](https://user-images.githubusercontent.com/88816771/129387572-ed6bb52f-7eaf-44a5-b2a1-bf645a628083.png)

- Import gds file into magic by the follwing command
  
https://user-images.githubusercontent.com/88816771/129388609-b6220391-15ec-4d77-af1f-c8bb238e16ae.mp4

- The tool overwrites the gds files imported each time. This can be enabled and disabled by **gds noduplicates** option
  - *gds noduplicated true*
  - *gds noduplicated false*

## Understanding Ports
- Select a port from the layout by selecting the area and pressing s
- Port queries such as port name, index, class, use, etc., can be queried

https://user-images.githubusercontent.com/88816771/129390690-6d4a4e27-58e8-4801-a564-76a43a0aa114.mp4

Port class and port use are default. The actual info can be found by extracting lef files

![image](https://user-images.githubusercontent.com/88816771/129393348-fb9bacd2-5f77-4716-9c9c-3bc25a9ef2e4.png)

- The displayed port names are different from the actual gds file. 

![image](https://user-images.githubusercontent.com/88816771/129392029-12b2893f-0d46-4b1e-8fef-be694876d74e.png)

![image](https://user-images.githubusercontent.com/88816771/129391919-e01a06d1-347c-48c0-8dc4-3143a1db00f6.png)

- This is because the magic doesn't use the exact metadata of the gds file. Alternatively, the metadata can be captured from lef and spice files or cdl netlists.
- Port name still remains the same even after importing the lef files. Thus, spice files can be read to order the ports by using:
  - *readspice /usr/share/pdk/sky130A/libs.ref/sky130_fd_sc_hd/spice/sky130_fd_sc_hd.spice*
  
![image](https://user-images.githubusercontent.com/88816771/129394559-57388e46-af99-4694-a5d4-b438f4e593aa.png)

## Abstract view
- Lef files show abstract view, having no transistors and no lables and is meant for placement and routing.
- The pins of LEF files are out of order compared to the spice file. Thus, importing spice file resolves this issue.

The below figure shows a mismatch

![image](https://user-images.githubusercontent.com/88816771/129397316-d813179d-186d-402f-8e66-5f333e42ffa8.png)

The below figure shows the ports matching
![image](https://user-images.githubusercontent.com/88816771/129398288-6a885b79-c6e1-4260-a8ac-7bbc3f7ad947.png)

To see the full gds, create a new cell called test and import LEF into it using getcell command.

![image](https://user-images.githubusercontent.com/88816771/129408847-15f6346b-4a0c-46e3-8f0c-b089940284ec.png)

Write the test gds file and quit magic

Now open magic again and read the saved test cell. Still the gds view is not seen and this is because the magic doesn't write abstract view into gds. Save the test and quit

![image](https://user-images.githubusercontent.com/88816771/129409580-4c6b34d5-99e9-41a2-ac2d-615c7c740ea2.png)

While reopening magic, loading the test again show the gds view

![image](https://user-images.githubusercontent.com/88816771/129410011-69525a34-cb02-4405-ad7f-380e73f5c1c1.png)

To edit:
*cellname writeable sky130_fd_sc_hd__a2bb2oi_2 true* 
Any changes will not be saved when "gds write" command is used.

View the .mag file in terminal using vi text editor

![image](https://user-images.githubusercontent.com/88816771/129412789-1a2251a1-ed4b-4370-b7a1-f2770a11007e.png)

## Extraction
- The file is extracted to spice and is compared with the original spice file from pdk library and it matched.

![image](https://user-images.githubusercontent.com/88816771/129414440-11d203eb-0bd0-4429-bd61-daf0c05d8f58.png)

### parasitic extraction with capacitance only
- extract all 
- ext2spice lvs
- ext2spice
- ext2spice cthresh 0
- ext2spice
- Check the spice file to view the capacitances being included
![image](https://user-images.githubusercontent.com/88816771/129415738-d056b512-d446-478b-9e60-3d10108f0b33.png)

### Parasitic extraction - RC
Commands "ext2sim labels on" and "ext2sim" generates 2 files: .node and .sim

![image](https://user-images.githubusercontent.com/88816771/129416108-1764b94e-110c-4c45-9da8-c829f4bbc15a.png)

.res.ext file generated, used for modifying some of the extracted values

### Final output
![image](https://user-images.githubusercontent.com/88816771/129416588-483701b8-ddc3-432a-a346-f688a7b540e9.png)

## DRC styles
- There are three different DRC styles:
  -- drc(fast) - Does only the important checks
  -- drc(full) - Check completely for any violations
  -- drc(routing) - checks routing errors
- Other DRC commands can be seen from the below video

https://user-images.githubusercontent.com/88816771/129419870-a624b167-22f7-4beb-8d3f-07ef89b898c5.mp4


Compare the DRC error message with the python script as below:

![image](https://user-images.githubusercontent.com/88816771/129420103-5cc5202e-9527-4554-b9a5-8edec75621d8.png)

Add the tap cells to get rid of DRC errors

![image](https://user-images.githubusercontent.com/88816771/129420771-558fa545-ee90-40d4-9cf2-cbb5ab31adf7.png)


## LVS
![image](https://user-images.githubusercontent.com/88816771/129423516-adb94861-fed0-4689-a016-3649d99a6ab0.png)

The above image shows a mismatch between instances and that can be solved

## XOR comparison

The following images explains the commands to check XOR

![image](https://user-images.githubusercontent.com/88816771/129425258-c0434de7-030b-494a-98e7-86688ff3d141.png)

![image](https://user-images.githubusercontent.com/88816771/129430654-a01a8ba8-524c-4556-95fe-b2496511ecaf.png)


 # Design rule Checking              
 ## Clone the Exercise files from github
 ![image](https://user-images.githubusercontent.com/88816771/129432234-d28bc179-8204-4beb-bce1-dbc997969d5b.png)
 
 ## Basic Rules
 - width-rule
   - select the area and choose DRC report (?). It details the error
   - The error is reported only for the area covered by the box
   - Pressing B key gives the dimensions of the box and grid on option helps view the grid
   - Select the box manually and paint it or use the following commands:
   - *:box width 0.14um*
   - *:paint m2*
   ![image](https://user-images.githubusercontent.com/88816771/129432919-c8ec5f76-4de5-43e4-a4fc-d73b2ccf01ce.png)

- Spacing rule
  - select the area until the white error dots and press key 6 to stretch the box.
  - Use move-stret-move command to manually spilt a single box to satisfy the rules
  ![image](https://user-images.githubusercontent.com/88816771/129433412-52894fca-3de3-4d69-8a10-5c3b7450e472.png)

- wide-spacing rule
  - select the smallest box and move the box by using key 6 and the issue is resolved
  
- notch_rule
  - select the box press key a and move the box by pressing key 8 as many times required

![image](https://user-images.githubusercontent.com/88816771/129433597-cab5d486-e972-4026-a7ea-90b799bd39e2.png)

## Via Rules

### width-rule - both horizontally and vertically
- Select the area vertically and press "a" then press key 6 as many times required  to stretch the box horizontally 
- select the area horizontally and press "a" then press key 8 once and press up arrow as namy times required
- The below video explains in detail:

https://user-images.githubusercontent.com/88816771/129451537-e3b542f9-d92e-420a-9a0c-10781585196e.mp4

### Multiple vias
- Contact cuts can be seen using *cif see contact_name*
- Contacts filt based on the available space
- Details presented in the video below:

https://user-images.githubusercontent.com/88816771/129451602-46ccb1aa-6b8a-43f6-8924-9033ecdf6562.mp4

### Via overlap
- This can be resolved by growing the box in all directions.
- *Box grow c* (centre) doesn't solve the issue completely and hence *box grow e* (east) and *box grow w* 

https://user-images.githubusercontent.com/88816771/129451835-1442873e-1d33-49d4-aa05-2b980ac1649d.mp4

(west) commands are used

### Auto-generate via

use space bar to cahnge the pointer, drag and click the left mouse button. 
To change the metal layer click shift+left mouse button

![image](https://user-images.githubusercontent.com/88816771/129452046-e37d21ea-a80f-48a4-86ec-c046f27600b7.png)

## Minimum area and hole rules
### Minimum area rule
- Select the area to be increased and stretch the box in any direction

https://user-images.githubusercontent.com/88816771/129452564-46e7a441-7802-4f18-82e4-562109331246.mp4

### Minimum hole rule
- Drc(fast) doesn't show the minimum hole error, to do so change the style to drc complete and check drc. 
- Delete a portion of area around the hole to make the hole bigger

https://user-images.githubusercontent.com/88816771/129452710-db6edb87-7f6c-413b-8860-45ac18b71295.mp4

## Derived layers
*cif see layername* can be used to see different layers

https://user-images.githubusercontent.com/88816771/129457887-a86f2142-0fe4-4c0d-ad6e-52926fd3b498.mp4

HVI contacts can be created by adding the mvpcontact and mvpdiffusion layers
![image](https://user-images.githubusercontent.com/88816771/129458235-92e2a894-85da-46ce-941f-e6686faa6e7c.png)

NPC can be seen from the below picture:
![image](https://user-images.githubusercontent.com/88816771/129458331-9922bc64-f2bc-452a-8174-692827ade0b3.png)

## Parameterized devices
- The devices are already designed from foundry but has errors. These can be resolved by painting the metal1 layers as below:

https://user-images.githubusercontent.com/88816771/129459251-f4260318-0a60-4578-a54e-ee9a655def2a.mp4

Some of the edge errors can be removed by erasing the polysilicon layers that form the edges. Diffusion layer is painted in case poly is not erasable

![image](https://user-images.githubusercontent.com/88816771/129460367-5c824d36-cd7b-4adb-9a3a-f2cc36850328.png)

## Miscillaneous Errors
### Scalegrid
The box size can't be scaled down lesser than prescribed by the foundry

![image](https://user-images.githubusercontent.com/88816771/129460533-79c2a577-18bd-4ea1-a2dd-62b4abe59a29.png)

### split and flip
The below image explains the procedure to split and flip the design. The DRC error can be resolved by moving the neighboring cell by 1 unit atleast

![image](https://user-images.githubusercontent.com/88816771/129460753-941e883c-ce1b-4428-9f17-a201f34ffb00.png)

### Local interconnect
Only 90 degree angles are permitted in local interconnects and hence, painting the whole area will resolve the errors.

![image](https://user-images.githubusercontent.com/88816771/129461212-4d593a6c-32e7-40bd-a1e6-62e47f98a0d8.png)

### Metal1 angle error
Only 45 and 90 degrees are allowed whereas, the box doesn't match the required angle size and hence by enlarging and splitting the box, the error gets resolved

![image](https://user-images.githubusercontent.com/88816771/129461285-e4aa912b-7d7f-4c93-9e0f-45b5032330f8.png)


### Poly spacing to diffusion error

Paint the poly to resolve the errors
![image](https://user-images.githubusercontent.com/88816771/129461629-5445c239-0860-4697-929f-17e7c265eddb.png)

### Via2 error
![image](https://user-images.githubusercontent.com/88816771/129461826-6065b52b-10cb-45eb-912e-c92f95ddeee5.png)

## Antenna Checks

### For latch-up errors, add tap cells manually at the end of the design and adjust in such a way to clear off the errors

![image](https://user-images.githubusercontent.com/88816771/129463037-fa3a6f7d-c34a-433d-b901-e26e1f91205b.png)

### Electrical rules can be checked by antenna checks

![image](https://user-images.githubusercontent.com/88816771/129462890-9ebe2afd-6c61-4100-aa74-119c80101e4b.png)

### Adding metal layers and filler cells
- Metal layers can be seen as of the below image

![image](https://user-images.githubusercontent.com/88816771/129463422-c0351fa4-7591-4b94-b01c-86bcd90a8bef.png)

- Filler shapes

![image](https://user-images.githubusercontent.com/88816771/129463455-922ae3c9-6d19-4620-b34a-5d09a553eeb3.png)

## Challenge:
### There were 88 errors initially and it was brought down to 29 errors
- width errors - resolved
- overlap errors- resolved
- tap cells inserted
- Minimum area - resolved
- Here is an image reporting the same

![image](https://user-images.githubusercontent.com/88816771/129464991-9d7c4c9c-ac85-426f-be63-c2983ccb7143.png)

# Day - 4 OpenLane FLow

## OpenLane/OpenRoad Automation

![image](https://user-images.githubusercontent.com/88816771/129779384-f9173bc0-1eb2-4f47-8f3c-9b4ad025e7f0.png)
- Verilog file and sky130nm PDK is given as input to the flow. From here, the files reach each stages of flow like synthesis, floorplan, placament, CTS, routing, STA and finally signoff (DRC and LVS checks) 
- This results in a complete GDSII file that's appropriate for fabrication

## OpenLane flow - non-interactive
To do the open lane flow non-interactively:
- Choose OpenLane -> Designs -> Choose one design of choice
- src folder has the RTL design
- Config.tcl has all the parameters required to design and check the circuit
- Export the path as:
  export PDK_ROOT=/usr/share/pdk and run ./flow.tcl -design  <design name>
- It completes all the steps and generates gds files in the "runs" folder.
  
  I have done a c17 design using the above steps and here is the final picture of the run
  
  ![image](https://user-images.githubusercontent.com/88816771/129781380-665a8f9a-bfd2-47a9-a280-f692a2978c83.png)

## OpenLane flow - interactive
  - run ./flow.tcl -interactive
  - prep -design <design name> -tag <tag name> 
  
 ![image](https://user-images.githubusercontent.com/88816771/129782974-691469d9-2b13-4f51-b139-5bfd3669769b.png)
  
  For synthesis type run_synthesis
  
 ![image](https://user-images.githubusercontent.com/88816771/129783301-0fdc4083-5e80-4856-a4e2-5c5c83b33f94.png)
  
  In the same way other steps can be invoked interactively. This helps to resolve the errors whever possible and saves time by not running the complete flow each time.


## Common DRC Errors
  - Follow the local interconnect rules
  - Plan the density in a appropriate way like, utilizatioln ratio and the cell density
  
    Here is a sample config.tcl parameters I used for the c17 design
  
  ![image](https://user-images.githubusercontent.com/88816771/129784257-cf685388-0603-485e-8b47-0a6c724c6084.png)
 
  
## violations - Fixing manually
  
  - The violations are seen as white dots in the .mag file
  - To fix the DRC violatiolns manually, Choose .mag file of the error reported design and correct the violations by design rules and then save it to GDSII

# Day - 5 LVS
## LVS Basics
Here, a basic comparison between two netlists are made to see for the circuit matching or mismatch errors

### Compare the netlist to check for LVS
- netA.spice and netB.spice are both compared to check for LVS the circuit matches uniquely
- comp.out has the results
- When the netlist topology is modified with one of the pins from any of the netlists, the lvs mismatch is seen

## LVS with subcircuits
- *reinitialize* command can be used to resets the previous netlists from memeory. This command should be used between different runs
- During the LVS check from exercise_2, the result shows "spice doesn't contain devices"
- Invoking "netA.spice test" "netB.spice test" shows the pins matching

### Changing port orders
The result is the same showing pins match as this doesn't consider the ordering of ports

### changing pin orders
The result is the same but shows top level cell failed pin matching

### Batch mode
*netgen -batch lvs "netA.spice test" "netB.spice test" |tee lvs.log*
This generates two files:
- comp.out showing the outputs
- lvs.log showing the errors

### generating script
cat > run_lvs.sh
#!/bin/sh
netgen -batch lvs "netA.spice test" "netB.spice test" /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl exercise_2_comp.out -json|tee lvs.log
echo ""../count_lvs.py exercise_2_comp.json|tee -a lvs.log
chmod a+x run_lvs.sh
./run_lvs.sh

## LVS with Blackboxes Subcircuits

- The netlist has empty subcircuits and a test subcircuit. When changing the order of the pins shows mismatched nets and devices
- when a new pin is replaced in one of the netlists, the result from comp.out shows "no matching pins"
- When a cell name is replaced with a new name, the LVS result shows pins matching but it flattens the unmatched subcells. To resolve this, include -blackbox next to -json in the above command in  run_lvs.sh. Now, the errors are seen

## LVS with SPICE low-level components
- netA.spice has some of the components like resisitors, capacitors and diodes included. It results in error. 
- sky130A_setup.tcl file is copied into the current directory. Now, the errors are seen.
- Permute commands can be included in the setup file to make LVS understand that the pins can be swapped. 
  - *permute "-circuit1 cell1 A C"*
  - *permute "-circuit2 cell2 A C"*

## LVS with Small Analog Block 

- Use xschem to open the caravel wrapper project and select LVS netlist: Top level is a .subcircuit
 ![image](https://user-images.githubusercontent.com/88816771/129491765-5604e02f-8cd6-4956-800c-dcc4e95d98c8.png)
 
 - Open Caravel project in Magic and extract to spice as shown below:  

https://user-images.githubusercontent.com/88816771/129488882-eb39de04-25ea-4879-a702-22c6338a2052.mp4

- Check for LVS, and results in errors.
- The testbench circuit can be seen in xschem and LVS check on the same results in more errors

## LVS with Analog devices-Power on reset
- Check the power vdd3v3 from the caravel and find out the connections relating to that.
- Get the node that connects vdd3v3 

![image](https://user-images.githubusercontent.com/88816771/129492580-eb2a8bd9-e068-4a4c-8d29-e834674d7474.png)

- Use the below commands to paint the layer that separates the power connections

![image](https://user-images.githubusercontent.com/88816771/129492653-196546f5-4224-481a-96e5-480a726417a9.png)

- Add resitor to the schematic for reduced LVS errors

![image](https://user-images.githubusercontent.com/88816771/129492721-c419ac43-44c8-4c84-b05f-162e278ce37f.png)

- The remaining errors would be from vssd nodes

![image](https://user-images.githubusercontent.com/88816771/129492795-b7bcb25b-6ff0-4e5f-af5a-cd55e2ab2f32.png)

## LVS with Verilog netlists

- The circuit is extracted to spice from magic and it is checked for LVS with the verilog netlist. It is found that the filler_0_11 is missing.

![image](https://user-images.githubusercontent.com/88816771/129492987-d073dd08-cf41-440f-95fc-cae9a46491ff.png)

- Add "export MAGIC_EXT_USE_GDS=1" in the run_lvs.sh and this resolves all the errors

## LVS with macros

- Import the management protect cell from magic and extract it to spice as shown below

![image](https://user-images.githubusercontent.com/88816771/129493131-b657afca-1677-400f-974b-d77335d914a6.png)

- check for LVS, still errors exists. Tyo resolve these, use magic to figure out the gnd nets that are connected together as hsown below

 ![image](https://user-images.githubusercontent.com/88816771/129493275-ce372191-27ea-4049-9156-eccbb72edc2d.png)
 
 - Include following commands in to the high voltage management protection files
   - *assign vssa1 = vssd;*
   - *assign vssa2 = vssd;* and the following ones into the management protection verilog files
   - *assign vssa1 = vssd;*
   - *assign vssa2 = vssd;
   - *assign vssd1 = vssd;*
   - *assign vssd2 = vssd;
  
  - This resolves all the errors.

## LVS with digital_pll

- Extract the below circuit to spice

![image](https://user-images.githubusercontent.com/88816771/129491673-bdbebe6c-28bf-45c1-938a-16f9854be19c.png)

- Add "export MAGIC_EXT_USE_GDS=1" to run_lvs.sh and rerun LVS check
- There are missing diodes and, this can be fixed by adding diodes into the spice file. By doing so, the diode errors are fixed
- Errors related to power can be fixed by connecting them to the power supplies

![image](https://user-images.githubusercontent.com/88816771/129493525-ea26f50b-1c68-48e7-a41c-9c737468db3d.png)


## LVS with property Errors

- Extracting the spice from magic and running LVS shows 6 errors relating to the resistior and capacitor values.
- This can be fixed by lowering the resistor and capacitor values as shown below

![image](https://user-images.githubusercontent.com/88816771/129493627-4be8be3f-00fa-4529-b0e9-3972c767412d.png)

- Doing so, brings down the LVS errors to 1 as shown below, which can be solved by doing clean DRC


![image](https://user-images.githubusercontent.com/88816771/129489076-fa0b5dcb-c759-4299-9546-fc91882323ce.png)


