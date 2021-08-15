 # Design rule Checking              
 ## Clone the Exercise files from github
 ![image](https://user-images.githubusercontent.com/88816771/129432234-d28bc179-8204-4beb-bce1-dbc997969d5b.png)
 
 ## Start with Exercise 1:
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

## Exercise 2:

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

## Exercise 3: 
### Minimum area rule
- Select the area to be increased and stretch the box in any direction

https://user-images.githubusercontent.com/88816771/129452564-46e7a441-7802-4f18-82e4-562109331246.mp4

### Minimum hole rule
- Drc(fast) doesn't show the minimum hole error, to do so change the style to drc complete and check drc. 
- Delete a portion of area around the hole to make the hole bigger

https://user-images.githubusercontent.com/88816771/129452710-db6edb87-7f6c-413b-8860-45ac18b71295.mp4

## Exercise 4:
 
## Exercise 5: 
### Derived layers
*cif see layername* can be used to see different layers

https://user-images.githubusercontent.com/88816771/129457887-a86f2142-0fe4-4c0d-ad6e-52926fd3b498.mp4

HVI contacts can be created by adding the mvpcontact and mvpdiffusion layers
![image](https://user-images.githubusercontent.com/88816771/129458235-92e2a894-85da-46ce-941f-e6686faa6e7c.png)

NPC can be seen from the below picture:
![image](https://user-images.githubusercontent.com/88816771/129458331-9922bc64-f2bc-452a-8174-692827ade0b3.png)

## Exercise 6:
### Parameterized devices
- The devices are already designed from foundry but has errors. These can be resolved by painting the metal1 layers as below:

https://user-images.githubusercontent.com/88816771/129459251-f4260318-0a60-4578-a54e-ee9a655def2a.mp4

Some of the edge errors can be removed by erasing the polysilicon layers that form the edges. Diffusion layer is painted in case poly is not erasable

![image](https://user-images.githubusercontent.com/88816771/129460367-5c824d36-cd7b-4adb-9a3a-f2cc36850328.png)

## Exercise 7: 
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

## Exercise 9:

### For latch-up errors, add tap cells manually at the end of the design and adjust in such a way to clear off the errors

![image](https://user-images.githubusercontent.com/88816771/129463037-fa3a6f7d-c34a-433d-b901-e26e1f91205b.png)

### Electrical rules can be checked by antenna checks

![image](https://user-images.githubusercontent.com/88816771/129462890-9ebe2afd-6c61-4100-aa74-119c80101e4b.png)

### Adding metal layers and filler cells
- Metal layers can be seen as of the below image
-   
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
















    









   

