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
The flipflop percentage is obtained by formula i.e **Flop Ratio = ((no of D_flipflops) / (Total no of cells))100**
so we get Flop ratio =(1613/18036)*100 = **8.94 %.**








