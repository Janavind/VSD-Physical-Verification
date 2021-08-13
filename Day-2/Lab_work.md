# Step 1: GDS read
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

# Step 2: Understanding Ports
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

# Step 3: Abstract view
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










