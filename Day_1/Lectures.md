# Lecture 1: Introduction to Skywater PDK
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

# Lecture 2: Intro - Open source EDA tools
![image](https://user-images.githubusercontent.com/88816771/129223330-1e44a475-731f-4dfe-a59d-b5a00c131f0d.png)

## Installing Sky130 open-PDK
1. Clone the repository https://github.com/RTimothyEdwards/open_pdks
2. Run **cd open_pdks**
3. Run configure **--enable-sky130-pdk**
4. Run **make**
5. Run **sudo make install**

## Tools:
- Magic
  - Handles GDS DEF and LEF, Does extraction and layouts are created and managed.
  - Mainly used to check DRCs and the violations can be fixed by following the design rules.

- Klayout - Alternative tool for DRC checks

- Openlane - A flow that supports the OpenRoad tools. This can be used interactively or ./flow.tcl can be used to run the scripts for RTL to GDSII

- Xschem - Third repository for Schematic Editor

- Netgen - LVS tool

- Ngspice - Spice simulator

## Sky130 Libraries

![image](https://user-images.githubusercontent.com/88816771/129225883-b5825306-a3db-4a1b-a925-3b3c79dbec7b.png)

# Lecture 3: Sky130nm Layers
- Min length of FET is 150nm
- 5 Layers of metals
  - Local interconnect connects to neighboring nodes or to metal 1 and not for routing
  - Poly contacts have nitride poly cut layer (NPC) around them
 - Front-end - Metal layers and vias for Diffusion and Ion implantation
 - Back-end - Deposition, MiM cap layers
   - MiM - Metal insulator Metal (Metal plates between two metal layers)
 - Redistribution Layer: Copper is deposited on the fabricated chip and solder bumps are added on top the Redistribution layer. No wire bonding is required.
 
# Lecture 4: Devices







