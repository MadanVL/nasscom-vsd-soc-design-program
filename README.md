# NASSCOM-VSD-SoC-Design-Program  
Exploring the ASIC design flow using OpenLane and the SkyWater 130nm PDK, a collaboration between Google and SkyWater. This program, led by industry expert Kunal Ghosh, provides hands-on experience in standard cell design, physical design, and GDSII generation.  

## 📌 About the Repository  
This repository documents my learnings from a 5-day intensive workshop on VLSI SoC design.  

### 🔹 **Note:**  
*All block diagrams and flowcharts in this repository are sourced from the VLSI System Design (VSD) SoC Design course.*  

[Visit VLSI System Design (VSD)](https://www.vlsisystemdesign.com/)  
## 📖 Table of Contents  
### 🟢 **Day 1: Introduction & Synthesis**
- Familarization with Openlane/sky130.  
- Physical Design (PnR) stages. 
- Design Preparationstage.
- Execution & analysis of Synthesis.
  
### 🟢 **Day 2: Floorplanning & Placement**  
- Introduction to floorplan phase.
- LEF vs DEF 
- Special cells.
- Execution & analysis of Placement.
  
  ### 🟢 **Day 3: Standard Cell Design & Characterization**  
- Std.cell design using sky130 PDK.
- SPICE simulation in ngspice.
- Sid.cell Characterization.

  ### 🟢 **Day 4: Timing Analysis & Clock Tree Synthesis (CTS)**  
- PnR with custom cell.
- Clock Tree Synthesis.
- Static Timing Analysis.
- Timing ECOs.

  ### 🟢 **Day 5: RTL-to-GDS Flow Completion**  
- Routing.
- Algorithm behind TritonRoute.
- SPEF analysis and extraction.

  ## 🏆 Acknowledgment  

---

## DAY-1 : 

### INTRODUCTION

DAY 1 Lecture Summary

  - Learned how VLSI technology integrates millions of tiny transistors into chips, enabling faster, smaller, and more affordable electronics.

  - Explored the Arduino board to see why its central microcontroller unit (MCU) is crucial.

  - The MCU handles everything—from reading sensors to controlling lights—thanks to VLSI advancements.

  - The Arduino board includes key components like the microprocessor (the brain of the system), voltage regulator, I/O pins, and communication interfaces, all working together to perform various tasks.

<li> Chip Packaging and Components </li>
<br>
  - Facilities that manufacture chips, called 'FOUNDRIES', are crucial because they determine the chip’s efficiency and performance.

<br>
  - Inside these chips are pre-designed digital blocks called 'MACROS' that help improve the chip’s functionality and speed.

<br>

Chips are generally made up of three major physical parts:

1) <b>Core</b>: The central zone where the full logic circuit is laid out.

2) <b>Pads</b>: Act as gateways for electrical signals to move in and out of the chip.

3) <b>Die<b>: Represents the total silicon area of the chip that contains both logic and I/O components.

![Screenshot 2025-04-10 175151](https://github.com/user-attachments/assets/71656b65-7f85-402f-adfe-eaecc95f9be4)
![Screenshot 2025-04-10 175839](https://github.com/user-attachments/assets/13c62d15-5733-4815-a589-29e8a047c770)

**<li>Components of open-source digital asic design </li>**
<br>
![Screenshot 2025-04-10 185239](https://github.com/user-attachments/assets/5178f569-824a-4bef-ba24-3564f0c72965)
<br>

Process Design Kit is PDK data which contains information about the manufacturing process. In 2020, Google collaborated with **SkyWater Technology** to release an **open-source FOSS 130nm** Production PDK.

**<li>RTL2GDS Flow</li>**

![Screenshot 2025-04-10 230449](https://github.com/user-attachments/assets/e0c449b7-3c0e-4c17-81ac-3372ecd65dad)

<br>

Above shows simplified RTL to GDSII flow.Steps invloved are explained as follows:

**Step 1 – Synthesis:**
This step converts the RTL design into a gate-level netlist using pre-designed blocks from the Standard Cell Library (SCL). These standard cells come in regular shapes but different sizes to perform basic logic functions.

**Step 2 – Floorplanning & Power Planning:**
Here, we decide the layout of major blocks on the chip—where inputs, outputs, and various components should go to save space.
Power planning involves setting up the chip's power supply network (VDD and GND) using dedicated routing structures.

**Step 3 – Placement:**
Standard cells are placed in their planned locations. This happens in two stages:

 Global Placement – Rough positioning of cells.

 Detailed Placement – Fine-tuning of exact cell positions.

**Step 4 – Clock Tree Synthesis (CTS):**
The clock signal is distributed throughout the chip using special tree-like structures to ensure it reaches all parts at the same time, preventing timing delays.

**Step 5 – Routing:**
Once placement and clock planning are done, we connect all components using metal layers:

 Global Routing – Planning the paths.

 Detailed Routing – Drawing the exact wire routes.

**Step 6 – Sign-off Checks:**
In the final step, the design is checked for correctness:

 DRC (Design Rule Check) – Ensures layout follows manufacturing rules.

 LVS (Layout vs. Schematic) – Confirms that layout matches the circuit design.

 STA (Static Timing Analysis) – Verifies the design meets timing constraints.

 
## OPENLANE AND STRIVE CHIPSETS

OpenLane began as an open-source project aiming to facilitate a genuine open-source tape-out experiment. It originated from e-fabless and serves as a platform that incorporates various tools like **Yosys, OpenRoad, Magic, KLlayout**, and other open-source tools. OpenLane streamlines the different stages of silicon implementation, making them easier to understand and work with. At e-fabless, they have a series of SOC (System on Chip) designs called Strive. Strive SOCs are entirely open, featuring open PDK (Process Design Kit), open RTL (Register Transfer Level), and open EDA (Electronic Design Automation) tools.

The Main aim of Openlane is to **produce clean GDSII without human intervention.**

<br>

**<li>OpenLANE ASIC design flow</li>**

<br>

![Screenshot 2025-04-10 233610](https://github.com/user-attachments/assets/e404c718-71c2-4b4b-ac74-d65719094bb7)

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
![Screenshot from 2025-04-11 15-13-23](https://github.com/user-attachments/assets/f9c3a482-0bbe-409f-a6e2-5c316071f21a)
Design preperation completed
![Screenshot from 2025-04-11 15-21-53](https://github.com/user-attachments/assets/679f4c75-336a-447a-bf24-b4a85f068f2d)

### GETTING STARTED - SYNTHESIZING THE DESIGN :

Next step is we need to perform the Synthesis process on the design. command used is

```
run_synthesis
```
It'll take a while (1-2 min) to perform synthesis but once it's done,we will see a message saying **'Synthesis was successful'**.
![Screenshot from 2025-04-11 15-37-43](https://github.com/user-attachments/assets/3eab5523-48e8-4060-af43-b9cc6c39c077)
![Screenshot from 2025-04-11 15-38-47](https://github.com/user-attachments/assets/2aacf3e9-a24f-434e-b0dc-c138b3e36445)
Total number of counter D flip-flops is **1613** and the total number of cells is **18036**.
The flipflop percentage is obtained by formula i.e **Flop Ratio = ((no of D_flipflops) / (Total no of cells))100**
so we get Flop ratio =(1613/18036)*100 = **8.94 %.**


## DAY-2

Configuration variables and their default values for *floorplanning* , *placement* , *CTS* , *Routing* , *magic* and *LVS*.
![Screenshot from 2025-04-11 23-28-41](https://github.com/user-attachments/assets/59584575-da7f-4b1b-94d4-4a30f50900f8)
![Screenshot from 2025-04-11 23-28-55](https://github.com/user-attachments/assets/9a122c4e-be18-4ee0-8b4d-02dd0f3142d6)
![Screenshot from 2025-04-11 23-29-08](https://github.com/user-attachments/assets/c683808c-02c3-4b22-b0d3-473b73367f68)
![Screenshot from 2025-04-11 23-29-19](https://github.com/user-attachments/assets/165a3739-6db4-4abd-b042-07012f377b5c)

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
![Screenshot 2025-04-11 174127](https://github.com/user-attachments/assets/4c9a36b3-5bed-4bc1-ba48-df3f7f0d5887)

### STEPS TO RUN FLOORPLAN & REVIEW FLOORPLAN FILES 

Once synthesis is sucessful,next step is floorplan. we use command 
```
run_floorplan
```
This will create a folder inside **runs** folder of **picorv32a** directory.
it will take a while to execute.once done we will get **PDN GENERATION IS SUCESSFUL** as shown in below image.
![Screenshot from 2025-04-11 23-26-36](https://github.com/user-attachments/assets/5e39b09f-abfe-4440-9b33-4a15f6915618)
![Screenshot from 2025-04-11 23-35-43](https://github.com/user-attachments/assets/d5da6b11-42b1-4402-9aa3-b29d0e852110)
Units is measured in microns,
**DIEAREA** is given in the below image:
![Screenshot from 2025-04-11 23-45-25](https://github.com/user-attachments/assets/78ad6dc4-ca2b-4af4-8b39-11f9bacd2714)

To see the actual layout after the floorplan ,go the folders as shown below:
```
openlane/designs/picorv32a/runs/08-02_06-43/results/floorplan
```

now we need to open the **magic** file by the  following command
<br>

```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```
![Screenshot from 2025-04-11 23-50-19](https://github.com/user-attachments/assets/25460a84-cbe8-4c98-b0f6-bddbb91db711)
<br>
To zoom in press left button of mouse then right button and press z 
<br>
![Screenshot from 2025-04-11 23-51-06](https://github.com/user-attachments/assets/ba73e104-3a00-4a1b-9a7e-39749c219d25)
<br>

In order to know the details of any cell in the layout , just move the cursor to that cell and press **S** to select the cell and then in the window of **tkcon** enter the command **"what"**. then it will displey the details of the selected cell.
lets see the detail of horizontal and vertical pins , in tkcon window it shows that the pin is in the metal 3 for horizontal pins ,similarly for the vertical pins, we see that thepin is at metal 2. image is shown below.
<br>
![Screenshot from 2025-04-11 23-57-49](https://github.com/user-attachments/assets/18687fb1-f773-4cd2-a638-4405aec81a72)
![Screenshot from 2025-04-12 00-00-19](https://github.com/user-attachments/assets/1f5e1688-8e95-4f6e-b0b5-5d492adbb008)

<br>

![Screenshot from 2025-04-12 00-02-45](https://github.com/user-attachments/assets/7ba4b55a-b72b-4ee2-8347-865003d7e151)
![Screenshot from 2025-04-12 00-02-14](https://github.com/user-attachments/assets/8dd29a53-6499-45eb-b009-73433be85541)

<br>

### Library Binding and Placement

Netlist binding and initial place design.
![Screenshot 2025-04-12 124252](https://github.com/user-attachments/assets/4fb55a31-c928-4b6e-8da0-a4794a056e8d)
![Screenshot 2025-04-12 122723](https://github.com/user-attachments/assets/87f9f41f-d378-40ef-883c-f053affdca1e)

Optimize placement using estimated wire-length and capacitance.

When sender and receiver are far diastance use buffer and repeaters.Hence there will no single loss.
![Screenshot 2025-04-12 130144](https://github.com/user-attachments/assets/0fe18d7b-b2b1-40f4-b3dd-7148d81390f7)

### STEPS TO RUN PLACEMENT

Once the Floorplanning is done sucessfully , the next step in the process is Placement. 
To run the placement use the command
```
run_placement
```
![Screenshot from 2025-04-12 13-25-36](https://github.com/user-attachments/assets/85305ce2-b750-457f-ae41-3fa95d6e6634)

Once Placement process is done , next step is to check whether the cells are placed correctly or not.by using **magic** tool.

use the below command to view the standerd cells placement. image is shown below
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

![Screenshot from 2025-04-12 13-30-06](https://github.com/user-attachments/assets/7423f9a5-3895-4ed6-892e-b1f51abfdfd3)
![Screenshot from 2025-04-12 13-31-46](https://github.com/user-attachments/assets/9df78398-bd70-4ac2-8774-508f49e750f2)

### Cell Design Flow

![Screenshot 2025-04-12 144428](https://github.com/user-attachments/assets/54e33ac6-6361-4127-a468-f14d0dfa8401)


## DAY-3

### **SPICE deck creation for CMOS inverter**

-Component connectivity
-Component values
-Identify 'nodes'
-Name 'nodes'
![Screenshot 2025-04-12 183145](https://github.com/user-attachments/assets/20b81dd1-121f-4b45-b21d-ee6e07b422af)
![Screenshot 2025-04-12 183225](https://github.com/user-attachments/assets/48c5d8b2-d56b-4501-8cc3-8f7c6b043fbd)














