# LVS Basics
## Compare the netlist to check for LVS
- netA.spice and netB.spice are both compared to check for LVS the circuit matches uniquely
- comp.out has the results
- When the netlist topology is modified with one of the pins from any of the netlists, the lvs mismatch is seen

# LVS with subcircuits
- *reinitialize* command can be used to resets the previous netlists from memeory. This command should be used between different runs
- During the LVS check from exercise_2, the result shows "spice doesn't contain devices"
- Invoking "netA.spice test" "netB.spice test" shows the pins matching

## Changing port orders
The result is the same showing pins match as this doesn't consider the ordering of ports

## changing pin orders
The result is the same but shows top level cell failed pin matching

## Batch mode:
*netgen -batch lvs "netA.spice test" "netB.spice test" |tee lvs.log*
This generates two files:
- comp.out showing the outputs
- lvs.log showing the errors

## generating script:
cat > run_lvs.sh
#!/bin/sh
netgen -batch lvs "netA.spice test" "netB.spice test" /usr/share/pdk/sky130A/libs.tech/netgen/sky130A_setup.tcl exercise_2_comp.out -json|tee lvs.log
echo ""../count_lvs.py exercise_2_comp.json|tee -a lvs.log
chmod a+x run_lvs.sh
./run_lvs.sh

# LVS with Blackboxes Subcircuits

- The netlist has empty subcircuits and a test subcircuit. When changing the order of the pins shows mismatched nets and devices
- when a new pin is replaced in one of the netlists, the result from comp.out shows "no matching pins"
- When a cell name is replaced with a new name, the LVS result shows pins matching but it flattens the unmatched subcells. To resolve this, include -blackbox next to -json in the above command in  run_lvs.sh. Now, the errors are seen

# LVS with SPICE low-level components
- netA.spice has some of the components like resisitors, capacitors and diodes included. It results in error. 
- sky130A_setup.tcl file is copied into the current directory. Now, the errors are seen.
- Permute commands can be included in the setup file to make LVS understand that the pins can be swapped. 
  - *permute "-circuit1 cell1 A C"*
  - *permute "-circuit2 cell2 A C"*

# LVS with Small Analog Block 

- Use xschem to open the caravel wrapper project and select LVS netlist: Top level is a .subcircuit
 ![image](https://user-images.githubusercontent.com/88816771/129491765-5604e02f-8cd6-4956-800c-dcc4e95d98c8.png)
 
 - Open Caravel project in Magic and extract to spice as shown below:  

https://user-images.githubusercontent.com/88816771/129488882-eb39de04-25ea-4879-a702-22c6338a2052.mp4

- Check for LVS, and results in errors.
- The testbench circuit can be seen in xschem and LVS check on the same results in more errors

# LVS with Analog devices-Poer on reset
- Check the power vdd3v3 from the caravel and find out the connections relating to that.
- Get the node that connects vdd3v3 

![image](https://user-images.githubusercontent.com/88816771/129492580-eb2a8bd9-e068-4a4c-8d29-e834674d7474.png)

- Use the below commands to paint the layer that separates the power connections

![image](https://user-images.githubusercontent.com/88816771/129492653-196546f5-4224-481a-96e5-480a726417a9.png)

- Add resitor to the schematic for reduced LVS errors

![image](https://user-images.githubusercontent.com/88816771/129492721-c419ac43-44c8-4c84-b05f-162e278ce37f.png)

- The remaining errors would be from vssd nodes

![image](https://user-images.githubusercontent.com/88816771/129492795-b7bcb25b-6ff0-4e5f-af5a-cd55e2ab2f32.png)

# LVS with Verilog netlists

- The circuit is extracted to spice from magic and it is checked for LVS with the verilog netlist. It is found that the filler_0_11 is missing.

![image](https://user-images.githubusercontent.com/88816771/129492987-d073dd08-cf41-440f-95fc-cae9a46491ff.png)

- Add "export MAGIC_EXT_USE_GDS=1" in the run_lvs.sh and this resolves all the errors

# LVS with macros

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

# LVS with digital_pll

- Extract the below circuit to spice

![image](https://user-images.githubusercontent.com/88816771/129489076-fa0b5dcb-c759-4299-9546-fc91882323ce.png)

- Add "export MAGIC_EXT_USE_GDS=1" to run_lvs.sh and rerun LVS check
- There are missing diodes and, this can be fixed by adding diodes into the spice file. By doing so, the diode errors are fixed
- Errors related to power can be fixed by connecting them to the power supplies

![image](https://user-images.githubusercontent.com/88816771/129493525-ea26f50b-1c68-48e7-a41c-9c737468db3d.png)


# LVS with property Errors

- Extracting the spice from magic and running LVS shows 6 errors relating to the resistior and capacitor values.
- This can be fixed by lowering the resistor and capacitor values as shown below

![image](https://user-images.githubusercontent.com/88816771/129493627-4be8be3f-00fa-4529-b0e9-3972c767412d.png)

- Doing so, brings down the LVS errors to 1 as shown below, which can be solved by doing clean DRC


![image](https://user-images.githubusercontent.com/88816771/129491673-bdbebe6c-28bf-45c1-938a-16f9854be19c.png)



