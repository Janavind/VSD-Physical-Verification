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

## Introduction to Skywater PDK
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

   4. 


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
 
