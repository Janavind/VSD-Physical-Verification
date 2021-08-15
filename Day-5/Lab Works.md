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



https://user-images.githubusercontent.com/88816771/129488882-eb39de04-25ea-4879-a702-22c6338a2052.mp4

![image](https://user-images.githubusercontent.com/88816771/129489076-fa0b5dcb-c759-4299-9546-fc91882323ce.png)


