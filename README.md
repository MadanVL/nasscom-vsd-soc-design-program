
# NASSCOM-VSD-SoC-Design-Program  
Exploring the ASIC design flow using OpenLane and the SkyWater 130nm PDK, a collaboration between Google and SkyWater. This program, led by industry expert Kunal Ghosh, provides hands-on experience in standard cell design, physical design, and GDSII generation.  

## 📌 About the Repository  
This repository documents my learnings from a 5-day intensive workshop on VLSI SoC design.  

### 🔹 **Note:**  
*All block diagrams and flowcharts in this repository are sourced from the VLSI System Design (VSD) SoC Design course.*  

[Visit VLSI System Design (VSD)](https://www.vlsisystemdesign.com/)  

## 📖 Table of Contents  
### 🟢 **Day 1: Introduction & Synthesis**  
- Introduction to ASIC Design Flow  
- Overview of OpenLane and STRIVE Chipsets  
- Commands for OpenLane Flow  
- Tool Invocation & Operation  
- Running Synthesis on a Design  

### 🟢 **Day 2: Floorplanning & Placement**  
- Chip Floorplanning Considerations  
- Steps for Running Floorplan & Reviewing Files  
- Placement Execution and Analysis  

### 🟢 **Day 3: Standard Cell Design & Characterization**  
- Standard Cell Design using Magic & NGSPICE Characterization  
  - Creating a Standard Cell Layout & Extracting Spice Netlist  
  - CMOS Inverter Layout in Magic  
  - Netlist Extraction & Transient Analysis  
- Introduction to Magic and Loading Sky130 Tech Rules  

### 🟢 **Day 4: Timing Analysis & Clock Tree Synthesis (CTS)**  
- Pre-Layout Timing Analysis & CTS  
- Converting Magic Layout to Standard Cell LEF  
- Optimizing Synthesis to Fix Slack  
- Timing Analysis using OpenSTA  
- Post-CTS OpenROAD Timing Analysis  

### 🟢 **Day 5: RTL-to-GDS Flow Completion**  
- Finalizing RTL-to-GDS Flow using TritonRoute & OpenSTA  

## 🏆 Acknowledgment  

---

## DAY-1 : 

### INTRODUCTION

In the DAY 1 lecture series 

   - Learned how VLSI technology packs tons of tiny transistors onto chips, making electronics faster, smaller, and cheaper.

   - Looked at an Arduino board to understand why its main 'chip' is a big deal.
   - Saw how this chip, known as the microcontroller unit (MCU), runs everything on the board, like reading sensors and controlling lights.
   - Noticed how VLSI tech helps make these chips, making gadgets like Arduinos possible.
   - The Arduino Microcontroller Board comprises various elements like the microprocessor, voltage regulator, I/O pins, and communication interfaces. The microprocessor, highlighted , acts as the brain, executing instructions and managing the board's functions. 
 
 <li> Chip Packaging and Components </li>
 <br>
- The places that make chips, called <b>'FOUNDRY'</b>, are really important for how well chips work.
<br>
- <b>'MACROS'</b> are digital parts inside chips that make them work better.

<br>

 There are three main components ,they are:
 
1) <b>Core</b>: Refers to the region where our complete logic will be implemented.

2) <b>Pads</b>:Through pads signals travel from external sources to chip or vice-versa.

3) <b>Die</b>: Refers to the entire area of the chip.

   ![components (1)](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/fa09ff1c-d118-45dc-84b9-68214bddaa6f)

<br>


**<li>Components of open-source digital asic design </li>**
<br>
![WhatsApp Image 2024-05-17 at 5 27 31 PM](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/1bd13b83-370d-4cd0-bb20-1fee9153c474)
<br>

Process Design Kit is PDK data which contains information about the manufacturing process. In 2020, Google collaborated with **SkyWater Technology** to release an **open-source FOSS 130nm** Production PDK.

**<li>RTL2GDS Flow</li>**

![WhatsApp Image 2024-05-17 at 5 27 32 PM](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/4316462c-f36c-44a2-a9d9-406bd5c7a295)

<br>

Above shows simplified RTL to GDSII flow.Steps invloved are explained as follows:

**Step 1 - Synthesis:** -In thi stage RTL is converted to gate level netlist using components from the Standard Cell Library(SCL). These components, known as Standard Cells, have regular layouts with varying widths.

**Step 2 - Floor Planning & Power Planning: :** In floor planning, we decide where to put components on the chip to make it as small as possible. We also plan where to put inputs, outputs, dimensions and power connections.

In power planning, the power supply network (VDD & GND) using special components of chip will be laid out.  

**step 3 - Placement:**

During placement, we put components and standard cells in their designated spots on the chip. We do this in two steps: first Global Placement, then Detailed Placement.

**step 4 - CTS (Clock Tree Synthesis):**

Before connecting signals, we organize the clock signals to avoid timing issues. We use special structures to make sure the clock arrives at different parts of the chip at the same time.

**step 5 - Routing:**

Once the clock is set up, we connect the rest of the signals. We use the remaining metal layers to make these connections. First, we plan out the routes (Global Routing), then we actually make the connections (Detailed Routing).

**step 6 - Sign-off:**

Finally, we check if everything is correct. We make sure the design follows the rules (Design Rule Check - DRC), matches the plan (Layout Vs. Schematic - LVS), and meets timing requirements (Static Timing Analysis - STA).


## OPENLANE AND STRIVE CHIPSETS

OpenLane began as an open-source project aiming to facilitate a genuine open-source tape-out experiment. It originated from e-fabless and serves as a platform that incorporates various tools like **Yosys, OpenRoad, Magic, KLlayout**, and other open-source tools. OpenLane streamlines the different stages of silicon implementation, making them easier to understand and work with. At e-fabless, they have a series of SOC (System on Chip) designs called Strive. Strive SOCs are entirely open, featuring open PDK (Process Design Kit), open RTL (Register Transfer Level), and open EDA (Electronic Design Automation) tools.

The Main aim of Openlane is to **produce clean GDSII without human intervention.**

<br>

**<li>OpenLANE ASIC design flow</li>**

<br>

![WhatsApp Image 2024-05-17 at 8 31 48 PM](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/f94a13ab-6cbe-4735-b360-619977aa5fd0)

</ul>
<br>

### COMMANDS USED IN OpenLANE FLOW:

```
1. prep -design <design> -tag <tag> -config <config> -init_design_config -overwrite
2. run_synthesis
3. run_floorplan
4. run_placement
5. run_cts
6. run_routing
7. run_magic
8. run_magic
9. run_magic_spice_export
10. run_magic_drc
11. run_netgen
12. run_magic_antenna_check

for fully automated run we can use command : ./flow.tcl -deisgn picorv32a
```


### TOOL INVOCATION & OPERATION:

- We're using the Sky130_fd_sc_hd PDK variant.
- "Sky130" refers to the process or node name.
- "fd" indicates the foundry name, which is SkyWater foundry.
- "sc" denotes standard cell library files.
- "hd" represents high density, a specific variant.
- The Sky130_fd_sc_hd variant includes various technology files such as Verilog, Spice, Techlef, Meglef, Mag, GDS, CDL, LIB, LEF, etc.
- The techlef file provides layer information essential for the design process.

  <br>

<ul


**<li>Directory order to invoke the tool OPENLANE </li>**
```
Desktop/work/tools/openlane_working_dir/openlane
```

In order to enter into BASH in terminal ,we must use a command 
```
docker
```

Now enter the follwing commands to invoke the openlane in terminal i.e using bash programming:

```
-bash-4.2$ pwd
/OpenLANE_flow 
-ls -ltr ( it includes several files like flow.tcl,scripts,conf.py files,README files  nearly 136 files etc) as shown in below image

```
![Screenshot from 2025-02-08 11-59-28](https://github.com/user-attachments/assets/a92c5537-b921-49df-bf20-9552d5c4fd3d)
Design preperation completed
![Screenshot from 2025-02-08 12-05-29](https://github.com/user-attachments/assets/126504c5-2b40-47e3-97b9-bd6f1ea687a4)

### GETTING STARTED - SYNTHESIZING THE DESIGN :

Next step is we need to perform the Synthesis process on the design. command used is

```
run_synthesis
```
It'll take a while (1-2 min) to perform synthesis but once it's done,we will see a message saying **'Synthesis was successful'**.
![Screenshot from 2025-02-08 12-15-36](https://github.com/user-attachments/assets/cd5d598f-6e4c-420a-9270-6fc1f7dd1bbc)
Total number of counter D flip-flops is **1613** and the total number of cells is **18036**.
The flipflop percentage is obtained by formula i.e **Flop Ratio = ((no of D_flipflops) / (Total no of cells))100**
so we get Flop ratio =(1613/18036)*100 = **8.94 %.**



## DAY-2


Configuration variables and their default values for *floorplanning* , *placement* , *CTS* , *Routing* , *magic* and *LVS*.
![Screenshot from 2025-02-08 10-26-30](https://github.com/user-attachments/assets/2171bf24-4f30-49c6-86e6-56cd53cd665f)
![Screenshot from 2025-02-08 10-26-40](https://github.com/user-attachments/assets/e865cf9f-180f-4554-ab5f-4bf27413e18e)
![Screenshot from 2025-02-08 10-26-44](https://github.com/user-attachments/assets/4dc28bbb-fc5b-490f-aa53-e8d4331a0ecb)
![Screenshot from 2025-02-08 10-26-51](https://github.com/user-attachments/assets/428ce80a-b853-41ed-bdb4-a3ff55780e5f)

### CHIP FLOOR PLANNING CONSIDERATION 

<ul>
    
**<li> UTILIZATION FACTOR AND ASPECT RATIO </li>**

In order to calculate the Utilization Factor and Aspect Ratio, we must know the height and width of core and die areas.
formula is given by:

**Utilization Factor = area occupied by the netlist / Total area occupied by the core**

--> If the utilization factor is 1 then it denotes 100 % utilization.
--> We never go for 100 % utilization, we go for 50 % or 60 % factor.

**Aspect Ratio = height of the core / width of the core**

--> When aspect ratio is 1 , then its a square chip.



### STEPS TO RUN FLOORPLAN & REVIEW FLOORPLAN FILES 

Once synthesis is sucessful,next step is floorplan. we use command 
```
run_floorplan
```
This will create a folder inside **runs** folder of **picorv32a** directory.
it will take a while to execute.once done we will get **PDN GENERATION IS SUCESSFUL** as shown in below image.
![Screenshot from 2025-02-08 12-16-47](https://github.com/user-attachments/assets/e4273110-7375-4fd9-b618-68d5fb65a8e5)
![Screenshot from 2025-02-08 12-26-25](https://github.com/user-attachments/assets/abd1dd0e-436c-4c3c-931c-58195a28c19b)
Units is measured in microns,
**DIEAREA** is given in the below image:
![Screenshot from 2025-02-08 12-31-52](https://github.com/user-attachments/assets/2d61aa11-5b4e-43ea-af14-a8cdf80a8814)

To see the actual layout after the floorplan ,go the folders as shown below:
```
openlane/designs/picorv32a/runs/08-02_06-43/results/floorplan
```

now we need to open the **magic** file by the  following command
<br>

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

![Screenshot from 2025-02-08 12-58-22](https://github.com/user-attachments/assets/201ae215-b1f2-45a0-b9af-7b64272cad95)
<br>
To zoom in press left button of mouse then right button and press z 
<br>
![Screenshot from 2024-05-16 22-38-02](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/5e7db0d4-169e-4faf-ba66-ddf3a5f50b6e)
<br>

In order to know the details of any cell in the layout , just move the cursor to that cell and press **S** to select the cell and then in the window of **tkcon** enter the command **"what"**. then it will displey the details of the selected cell.
lets see the detail of horizontal and vertical pins , in tkcon window it shows that the pin is in the metal 3 for horizontal pins ,similarly for the vertical pins, we see that thepin is at metal 2. image is shown below.
<br>
![Screenshot from 2024-05-16 22-40-09](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/e492e6b9-223c-4ae8-94f0-c2c774ff56f4)
![Screenshot from 2024-05-16 22-40-20](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/f4cfea60-a78e-479a-ad9b-da645c1b3311)


<br>

![Screenshot from 2024-05-16 22-42-19](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/cc2a9a61-fe67-44a8-bed4-f7f7467676d5)
![Screenshot from 2024-05-16 22-42-35](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/da1fd5a6-6a93-4f68-af1f-280602bc6f0d)

<br>
</ul>


### STEPS TO RUN PLACEMENT

Once the Floorplanning is done sucessfully , the next step in the process is Placement. 
To run the placement use the command
```
run_placement
```
![Screenshot from 2025-02-20 22-53-27](https://github.com/user-attachments/assets/82915ef4-a8a5-4f93-94f6-01e630ed9511)



Once Placement process is done , next step is to check whether the cells are placed correctly or not.by using **magic** tool.

use the below command to view the standerd cells placement. image is shown below
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

![Screenshot from 2025-02-10 20-04-41](https://github.com/user-attachments/assets/bd9bb9ca-db4b-4f18-8649-b52aabc1adf2)


## DAY-3

  1.Git clone vsdstdcelldesign    
  Commands to open inverter layout in magic  
  
```bash
  #Change directory to openlane
  cd Desktop/work/tools/openlane_working_dir/openlane

  #Clone the repository with custom inverter design
  git clone https://github.com/nickson-jose/vsdstdcelldesign

  #Change into repository directory
  cd vsdstdcelldesign

  #Copy magic tech file to the repo directory for easy access
  cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

  #Check contents whether everything is present
  ls

  #Command to open custom inverter layout in magic
  magic -T sky130A.tech sky130_inv.mag &
```
![Screenshot from 2025-02-11 17-59-47](https://github.com/user-attachments/assets/e215a120-f5d8-40b3-96bf-5780845bb61e)
![Screenshot from 2025-02-11 18-14-13](https://github.com/user-attachments/assets/4fcf3e00-d5f1-4046-aef6-646a6cd6fb11)
CMOS inverter layout
![Screenshot from 2025-02-11 18-13-33](https://github.com/user-attachments/assets/99fb518b-5e1f-4079-9f0b-9996aa039e41)
NMOS
![Screenshot from 2025-02-11 20-11-49](https://github.com/user-attachments/assets/c58490c4-4907-424f-824c-99662d47a3d0)
PMOS
![Screenshot from 2025-02-11 20-12-38](https://github.com/user-attachments/assets/f8e0e6b4-df90-47dc-bad2-85a687003cc7)
Y(output) connecting the drain of nmos and pmos
![Screenshot from 2025-02-11 20-14-46](https://github.com/user-attachments/assets/c9a8e9d7-2582-451f-b2d5-a0f82da440b5)
Poly
![Screenshot from 2025-02-11 21-05-33](https://github.com/user-attachments/assets/e52c3d81-e176-4eb3-a7fa-5c3a51f8ab5b)
Spice file
![Screenshot from 2025-02-11 21-27-09](https://github.com/user-attachments/assets/ade380ef-7256-4d25-b451-040556db0666)

2.Editing the spice model file for analysis through simulation. Edited spice file
![Screenshot from 2025-02-13 20-25-03](https://github.com/user-attachments/assets/644240de-8de2-4eb2-ac8e-7be5dc1d92b7)

  3.Spice extraction of inverter in magic.  

```bash
  #Check current directory
  pwd

  #Extraction command to extract to .ext format
  extract all

  #Before converting ext to spice this command enable the parasitic extraction also
  ext2spice cthresh 0 rthresh 0

  #Converting to ext to spice
  ext2spice
```

![Screenshot from 2025-02-12 18-49-11](https://github.com/user-attachments/assets/34ba8037-ebc6-4c14-9135-dc2b271c4645)
  4.Ngspice simulation
  Commands for ngspice simulation
  
```bash
  #Command to directly load spice file for simulation to ngspice
  ngspice sky130_inv.spice

  #Now that we have entered ngspice with the simulation spice file loaded we just have to  load the plot
  plot y vs time a
```
![Screenshot from 2025-02-13 20-27-34](https://github.com/user-attachments/assets/d853404a-871c-410a-8a16-a7684042cf03)

Transient Analysis of the CMOS inverter is given below:
When C=0.07fF
![Screenshot from 2025-02-13 20-24-05](https://github.com/user-attachments/assets/f2a4c53c-a8a8-4c3d-bd80-87c688decc69)
when C=2fF
![Screenshot from 2025-02-13 20-26-28](https://github.com/user-attachments/assets/bdc46569-22cb-44bf-a5d5-2a7c0ce60313)

next step is to calculate the rise time , fall time , cell rise delay and cell fall delay:
click on the waveforms to get values i.e 

--> 20 % of VDD (3.3 V) is 0.66V
--> 80 % of VDD (3.3 V) is 2.64V
--> 50 % of VDD (3.3 V) is 1.65V
<br>
1) **Rise time**- Time taken to the output waveform to 20% value to 80% value.

<br>

![WhatsApp Image 2024-05-21 at 1 56 45 AM(1)](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/e6be18ed-736a-4df4-ae12-a09afac58937)

<br>
therfore rise time= (2.24 - 2.18)e-09 = 0.64e-09s
  
2) **Fall time**- Time taken to the output waveform to 80% value to 20% value.
<br>


<br>
therfore fall time = (2.17999 - 2.12)e-09 = 0.5999e-09s


5) **cell rise delay**-Time difference between the 50% value of input and 50% value of the output.

<br>

![WhatsApp Image 2024-05-21 at 1 56 46 AM](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/f03945b9-f78b-46b8-bc39-af73de135a37)



<br>
therfore cell rise delay or propogation delay = (2.2107 - 2.1501)e-09 = 0.061e-09s

7) **cell fall delay**-Time difference between output falling to 50% and input is rising to 50% value.

<br>

![WhatsApp Image 2024-05-21 at 1 56 45 AM](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/04d2adf7-f47f-4d9f-830e-1f5f3ad379d0)


<br>
thefore cell fall delay = (4.077 -4.05)e-09 = 0.027e-09 s

</ul>

### INTRODUCTION TO MAGIC TOOL AND STEPS TO LOAD SKY130 TECH - RULES 
<ul>
First step is to download the lab files required for DRC error fixing by below coammand
    
```
   wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
```
to open magic use 
```
  magic -d XR

```
![Screenshot from 2025-02-13 22-34-56](https://github.com/user-attachments/assets/b81337b6-da34-47b6-9e52-cb191b2a7e0f)
![Screenshot from 2025-02-13 22-38-27](https://github.com/user-attachments/assets/0701f863-be5a-49d2-abfa-6e6c4b2caf84)
.magicrc file
![Screenshot from 2025-02-13 22-39-07](https://github.com/user-attachments/assets/b0bac58e-69b1-4dfd-962e-7798ab04d4cb)

Open met met3.mag file from file -> Open in magic window

![Screenshot from 2024-05-21 02-28-23](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/981cce52-bfa1-4a4b-b97e-dcb17c62bd17)

![Screenshot from 2024-05-21 02-29-44](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/d9b57b99-8bd0-41d5-87fd-3b9830e832ab)
<br>


![Screenshot from 2024-05-21 09-27-39](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/51b0d993-0c41-4d77-b664-6be93f8cea59)

![Screenshot from 2024-05-21 09-27-47](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/8de8ec48-cbb8-4b9c-954d-ed008e24c4f8)
![Screenshot from 2024-05-21 09-29-44](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/d684b929-e8b3-416d-919f-973400895a18)
<br>

There are some changes to be made in **sky130A.tech file**. Search for the **poly.9** in the **sky130A.tech** file. It appears in both the **POLY** and **uhrpoly** sections,changes given below:

open sky130A.tech file as shown below

<br>

![Screenshot from 2024-05-21 09-30-10](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/ed5c72ea-2148-40ad-b146-a8a769baa0e9)


<br>

![Screenshot from 2024-05-21 09-41-52](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/d29c9f03-29e7-4b1f-bb74-1611914c04e0)


<br>

![Screenshot from 2024-05-21 09-45-04](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/749cf951-eda5-46e4-937f-6ad00744ea70)

<br>

next step is to load the sky130A.tech file in tckon window as as shown below:
<br>
![Screenshot from 2024-05-21 09-45-35](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/3f281da7-04d7-456c-bbd7-8947901587ef)

<br>

DRC is checked as shown in below image:
<br>

![Screenshot from 2024-05-21 09-46-51](https://github.com/akshaynayak212/NASSCOM-VSD-SoC-Design-Program/assets/169296665/3a3afb3c-1bd6-428f-8fca-6af9c35358b5)

<br>
    
</ul>


## DAY-4


### PRELAYOUT TIMING ANALYSIS AND CLOCK TREE SYNTHESIS <ul>
The next step is to get the **.lef** file from the inverter design.

Open the tracks.info file 
directory is : 
```
openlane_working_dir/pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info 
```
![Screenshot from 2025-02-16 11-17-50 (1)](https://github.com/user-attachments/assets/feec19de-d301-4c0c-80be-293e93c39969)

Change grid into tracks in console window as shown in below image:
![Screenshot from 2025-02-16 11-49-48](https://github.com/user-attachments/assets/cf4797e8-be36-45c4-8b3c-8567d64f7a50)
![Screenshot from 2025-02-16 11-57-01](https://github.com/user-attachments/assets/3dcc28d8-276e-4cee-b191-e14e6bfca109)
![Screenshot from 2025-02-16 11-57-21](https://github.com/user-attachments/assets/5769a86c-af2d-40dc-958d-48ddde636a1c)

Intersection of grids at the input(A) and output(Y) of the inverter.
![Screenshot from 2025-02-14 18-10-10](https://github.com/user-attachments/assets/52e5b982-3d2a-4c3a-85e5-bb8090cf8960)
![Screenshot from 2025-02-14 18-19-18](https://github.com/user-attachments/assets/3e3dcb42-2e8f-4195-864c-03991e8ed3e5)

*<li>Steps to convert magic layout to std cell LEF</li>*

for the extraction of lef file ,enter the given below command in the tckon window.

```
lef write
```
![Screenshot from 2025-02-14 18-25-43](https://github.com/user-attachments/assets/5ee9fbfd-38fb-4fbb-8c32-c1cc2949a796)
![Screenshot from 2025-02-14 18-26-20](https://github.com/user-attachments/assets/d2db057a-c944-4894-b920-b2b8b1475c60)

now the *sky130_inv.lef* file is created in *vsdstdcelldesign* folder in *openlane* directory.

![Screenshot from 2025-02-16 12-08-06](https://github.com/user-attachments/assets/973b2714-de98-44ed-a8dd-f1e672557434)

The next step is to copy the newly created *.lef* file to src folder of *picorv32a* directory along with some important libraries.

libraries are present in following directory
![Screenshot from 2025-02-19 22-43-33](https://github.com/user-attachments/assets/adc301d6-c452-4740-8c96-90cb40d68b33)
![Screenshot from 2025-02-19 22-44-51](https://github.com/user-attachments/assets/ada15ecb-4b4e-452a-b53e-362aaaa9c762)

Now the next step is to edit the config.tcl of picorv32a design.

config.tcl file is present in picorv32a folder

NOW go to the openlane directory run **docker** command to go to bash terminal

next commands are given below:

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis
```

![Screenshot from 2025-02-19 21-30-38](https://github.com/user-attachments/assets/7de84003-2f36-4310-a85b-2c1d1e21b284)
**<li> Steps to configure synthesis settings to fix slack </li>**

Once the synthesis is sucessfull, check the slack

![Screenshot from 2025-02-19 21-36-12](https://github.com/user-attachments/assets/36d722aa-a598-4b98-876f-e821641c197a)

we get wns= -23.89  and tns== -711.5.

next command is to run floorplan

```
run_floorplan

```
if error occurs in floorplan stage use below commands one by one :
```
init_floorplan

place_io

tap_decap_or

```

![Screenshot from 2025-02-19 21-42-11](https://github.com/user-attachments/assets/8197b336-0a38-4e8a-b5fd-456dac23bb41)
![Screenshot from 2025-02-19 21-42-28](https://github.com/user-attachments/assets/3fc0f32e-56ee-4976-af08-07e1e7e530f6)
![Screenshot from 2025-02-19 21-42-58](https://github.com/user-attachments/assets/04fb8e6b-ac96-4d0f-98e8-20b17ab55dc3)
next command is to run placement
```
run_placement

```

![Screenshot from 2025-02-19 21-43-22](https://github.com/user-attachments/assets/87b86adc-0815-4f3c-9be7-df62fa51b075)
The standard cell inverter can be seen in the below image using magic tool
![Screenshot from 2025-02-16 13-09-57](https://github.com/user-attachments/assets/8f5171f6-533a-4255-94a4-9ab223c38b96)
use the below command in tckon window to check whether inverter placed properly or not
```
expand
```
![Screenshot from 2025-02-16 13-14-07](https://github.com/user-attachments/assets/28059584-1605-4972-883e-e7fae4cfb3d3)

![Screenshot from 2025-02-16 13-14-15](https://github.com/user-attachments/assets/fd37a774-9c2a-413c-a3e3-285a2d395ebf)
![Screenshot from 2025-02-17 18-09-17](https://github.com/user-attachments/assets/39fcf3f3-d2ae-4d4c-897a-a6eac3598dea)
next create a my_base.sdc file in openlane/designs/picorv32a/src directory. write the code as given in below image in my_base.sdc file.
![Screenshot from 2025-02-19 23-18-48](https://github.com/user-attachments/assets/e93e6114-5fc2-4be6-baf2-67ce7e424983)
![Screenshot from 2025-02-16 15-12-05](https://github.com/user-attachments/assets/931dabde-1ab4-4631-8ec4-4af44e7a2cda)

![Screenshot from 2025-02-16 15-12-09](https://github.com/user-attachments/assets/c466bd9f-f718-43fa-82d4-9b6b5c26465d)

![Screenshot from 2025-02-16 17-08-21](https://github.com/user-attachments/assets/0b02a770-96c7-4db8-8c8c-528d0fff9012)
To Run clocktree synthesis use below command :

```
run_cts
```


![Screenshot from 2025-02-16 17-12-36](https://github.com/user-attachments/assets/6bd987b2-9e48-4c9c-89c2-d544ede67c08)

![Screenshot from 2025-02-16 18-48-07](https://github.com/user-attachments/assets/2af07a93-bcf0-4f11-8ae9-7a4bcbfab694)
next commands :

```
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
```


```
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/16-02_07-04/results/placement/picorv32a.placement.def
run_cts

```


<li> Enter the openROAD flow and check timing by using the following commands </li>

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/16-02_07-04/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/16-02_07-04/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts1.db
read_verilog /openLANE_flow/designs/picorv32a/runs/16-02_07-043/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

exit

echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)


```
![Screenshot from 2025-02-16 18-51-19](https://github.com/user-attachments/assets/8f184dc9-05eb-4f13-9afb-aacde9807e96)


## DAY-5 


###  Final step for RTL2GDS using tritinRoute and openSTA

first step is to build  the power distribution network by follwing command :
```
gen_pdn

```
![Screenshot from 2025-02-16 18-57-08](https://github.com/user-attachments/assets/319210c7-e6c8-4ab0-8b19-2da4ea31682a)
![Screenshot from 2025-02-16 18-57-37](https://github.com/user-attachments/assets/d22dfa7f-6417-4364-b771-9e6425a0bd4a)


To check PDN, open Magic tool go to /tmp/floorplan/ indside the run folder in openlane directory by using below commands :

```

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```  Commands to load PDN def in magic in another terminal

```bash
  #Change directory to path containing generated PDN def
  cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/16-02_07-04/tmp/floorplan/

  #Command to load the PDN def in magic tool
  magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 14-pdn.def &
```

![3](https://github.com/user-attachments/assets/fe10cc3a-e698-4285-94ac-a2f22c2ad5bc)
![4](https://github.com/user-attachments/assets/9fd43f7a-7363-4929-bb47-2f5bd4de3534)
  Final step is routing to run routing use below command :

```
run_routing
```
![Screenshot from 2025-02-16 21-37-40](https://github.com/user-attachments/assets/cb45aa76-93e5-41ae-ab8c-01f3098aa726)
Routing is done.
![Screenshot from 2025-02-16 22-43-30](https://github.com/user-attachments/assets/0339cc25-b809-4d44-9ea9-9f8a563eb4cc)

To view the final design  with routing in Magic tool , go to the results/routing/ in  the runs folder of openlane directory by using below command :

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```

![8](https://github.com/user-attachments/assets/b5370bc4-1ea9-46bd-9e17-9c39bd73ffdf)
![9](https://github.com/user-attachments/assets/750bc2e9-7f4d-4a1d-8895-ece57687fbf2)
![10](https://github.com/user-attachments/assets/983e02f7-37d5-4449-ad0e-b8255166eea6)

  Fast route guide present in openlane/designs/picorv32a/runs/16-02_07-04/tmp/routing directory
  ![Screenshot from 2025-02-19 18-42-21](https://github.com/user-attachments/assets/6bb161d6-d6b9-45ad-8705-20e13b3962f3)

## Acknowledgement


I want to give a big thank you to Mr. Kunal Ghosh, one of the founders of VLSI System Design (VSD) Corp. Pvt. Ltd., and Mr. Nickson Jose for their amazing help during the DIGITAL-VLSI-SOC-DESIGN-AND-PLANNING workshop. They really know their stuff when it comes to chip design using OpenLANE software and other cool techniques. Their guidance has been super valuable in teaching me all about it.
