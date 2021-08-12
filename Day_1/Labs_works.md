# Lab1: Checking tool installation
Here the proper functioning of the tools are checked 

  1. Type Magic
  
![image](https://user-images.githubusercontent.com/88816771/129247063-f92382b9-14db-4acc-b247-ca19b513f4de.png)

Two windows pop-up 
  - tkcon console
  - Layout window

  2. Type netgen - tkcon window opens

![image](https://user-images.githubusercontent.com/88816771/129247949-39752797-bbd7-4f2c-ab63-01f55d5d895e.png)

  4. Xschem - schematic window pops-up

![image](https://user-images.githubusercontent.com/88816771/129248019-f0acc516-1e9c-40d4-b804-35be6e71b69e.png)

  6. ngspice - opens in command prompt 

![image](https://user-images.githubusercontent.com/88816771/129248264-b6b7caec-d05b-48a2-a55b-82e89929556e.png)

Running the tools without console will be like:

![image](https://user-images.githubusercontent.com/88816771/129249002-bc6b2770-6a03-4275-b301-efcf74d41056.png)


# Lab2: Creating Device Layout
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

# Lab 3: Creating schematic in xschem

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


# Lab 4: Schematic to symbol

- make schematic to symbol by following the video
- The voltage sources are drawn from devices
- Make sure to add the "code-shown" from the devices
- Add in and out and gnd pins
- Finally save and exit

[sch to symbol.zip](https://github.com/Janavind/VSD-Physical-Verification/files/6978610/sch.to.symbol.zip)












