# VSD-Physical-Verification
This repo details the learning in 5-day Physical Verification workshop by VSD from Aug-11 2021 to Aug-15 2021. 

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





















 
