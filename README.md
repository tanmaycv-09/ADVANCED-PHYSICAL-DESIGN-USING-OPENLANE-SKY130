# ADVANCED-PHYSICAL-DESIGN-USING-OPENLANE-SKY130

<p align="center"><a href="https://www.vlsisystemdesign.com/advanced-physical-design-using-openlane-sky130/"><img width="1074" alt="Screenshot 2023-01-25 at 11 28 06 AM" src="https://user-images.githubusercontent.com/77117825/214491585-61d01d0a-5acc-442b-9d41-4e58329937cd.png">
</a></p>

# Brief Description of the Workshop

Physical Design or PnR (Place and Route) is the core of any IC design cycle. From a RTL netlist to final tape-out, each phase of PnR brings it’s own challenges and surprises. Design and characterize your own standard cell. Have a hands-on in the Physical Design domain. Generate a full GDSII from a RTL netlist.

 # Table of Contents 
 
 
 
  # Day1 Inception of open-source EDA, OpenLANE and Sky130 PDK
  
  On the first day of the workshop, we learnt about the method in which an integrated circuit talks to a computer. We also looked into the steps that are required for a computer to talk to an IC. We also saw the RISC-V architecture and the flow from a high-level language program to a binary level program. Later on, we also saw the RTL2GDS flow. Finally, the day ended with us performing a synthesis run on an example design of "picorv32a".
  
  # Part 1: How to talk to computers
# Lec 1 : Introduction to QFN-48 Package, chip, pads, core, die and IPs
- Block diagram of an arduino board

<img width="1165" alt="Screenshot 2023-01-24 at 5 28 12 PM" src="https://user-images.githubusercontent.com/77117825/214494751-b0e602dc-c6a2-424d-9039-a88c89eeae3e.png">

 . Chip Overview 

<img width="1122" alt="Screenshot 2023-01-24 at 5 43 05 PM" src="https://user-images.githubusercontent.com/77117825/214495242-699b9c62-cf90-48b0-b374-5d489c344dd5.png">

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214495830-43848f13-6644-4096-aa9f-02a930dab062.jpeg"></p>

# Lec 2 : Introduction to RISC-V
This is the way in which we are going to talk to the computers. This is basically the flow of information from high level language program to the flip-flop level language program (binary language), any high level language is first compiled in its assembly language program this assembly language program is then converted into the machine language program. The bits (0s and 1s) of the machine level program get executed in the layout and we get the required output. The interface that is required between RISC-V architecture and the layout is the "Hardware Description Language".
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214499190-933d4ea4-1b42-4892-b6d6-757e1a06bbb0.jpeg"></p>

# Part 2: SoC design and OpenLANE
# Lec 1: Introduction to all components of open-source digital asic design
- "ASIC" stands for Application specific integrated circuits.
- The digital ASIC design elements are:
  -  RTL IPs (Hardware Description Language and Register Transfer Level models)
  - EDA Tools
  - PDK Data

<p align="center"><img width="663" alt="Screenshot 2023-01-24 at 6 44 28 PM" src="https://user-images.githubusercontent.com/77117825/214500708-fd98af28-4fe0-4e2f-b02f-f437ac4ee65b.png"></p>

- PDK is the interface between the FAB and the designers.
- PDK stands for "Process Design Kit".
- PDK is a collection of files used to model a fabrication process for the EDA tools used to design an IC.

# Lec 2: Simplified RTL2GDS flow
<p align="center"><img width="912" alt="Screenshot 2023-01-24 at 6 47 38 PM" src="https://user-images.githubusercontent.com/77117825/214501250-7c22b54d-4a95-44df-85f4-088355f3e2f4.png"></p>
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214501627-3c52f564-7640-43cc-a5e5-2ff9fc924b83.jpeg"></p>
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214501653-87e0da2f-9f91-4697-8d85-1dab0690720d.jpeg"></p>

#Lec 3: Introduction to OpenLANE and Strive chipsets
- OpenLANE's Main Goal: Produce a clean GDSII with no human intervention (no-human-in-the-loop).
- Clean means:
  - No LVS violations
  - No DRC violations
  - No Timing Violations

- It has two modes of operation:
  - Autonomous mode
  - Interactive mode

# Lec 4: Introduction to OpenLANE detailed ASIC design flow
<p align="center"><img width="896" alt="Screenshot 2023-01-24 at 8 33 45 PM" src="https://user-images.githubusercontent.com/77117825/214511335-4fe028eb-4574-4996-aa4c-487862793f81.png"></p>


- The flow starts with RTL synthesis so the RTL is fed to "Yosys" with the design constraints.
- Yosys translates the RTL into a logical circuit using generic components.
- This circuit can be optimized and then mapped in to cells using "abc".
- ABC has to be guided during the optimization and this guidance comes in the form of abc synthesis strategies.
- Synthesis exploration utility is used to generate a report that shows how the design delay and area is affected by the synthesis strategy.
- Based on this exploration we can choose the best strategy to continue with.
- Design exploration utility can be used to sweep the design configurations.
- It generates a report which shows different design matrix and number of violations generated after generating the final layout.
- OpenLANE Regression Testing:
  - The design exploration utility is also used for Regression Testing.
  - We run openLANE on ~70 designs and compare the results to the best known ones.
- Design for Test (DFT) includes:
  - Scan Insertion
  - Automatic Test Pattern Generation (ATPG)
  - Test Patterns Compaction
  - Fault Coverage
  - Fault Simulation
- Physical Implementation (also called automated PnR (Place and Route)) includes the following:
  - Floor planning and Power planning
  - End Decoupling capacitors and Tap Cells insertion
  - Placement: Global and Detailed
  - Post placement optimization
  - Clock Tree Synthesis (CTS)
  - Routing: Global and Detailed
- Logical Equivalence Check (LEC):
  - Everytime the netlist is modified, verification must be performed:
    - CTS modifies the netlist
    - Post placement optimization modifies the netlist
  - LEC is used to formally confirm that the function did not change after modifying the netlist.
- Dealing with Antenna Rules Violations:
  - When a metal wire segment is fabricated, it can act as an antenna.
    - Reactive ion etching causes charge to accumulate on the wire.
    - Transistor gates can be damaged during fabrications.
  - Two solutions are possible:
    - Bridging attaches a higher layer intermediately (requires router awareness).
    - Add antenna diode cell to leak away charges (antenna diodes are provided by the SCL)
  - We take a preventive approach:
    - Add a fake antenna diode next to every cell input after placement.
    - Run the antenna checker (magic) on the routed layout.
    - If the checker reports a violation on the cell input pin, replace the fake diode cell by a real one.
- Static Timing Analysis:
  - The tool used for STA is OpenSTA.
  - Physical Verification DRC and LVS:
- Magic is used for Design Rule Checking and spice extraction from layout.
  - Magic and Netgen are used for LVS (extracted spice by magic versus verilog netlist)

# Part 3: Get familiar to open-source EDA tools

- Run docker using the command docker.
- This should run the docker application in the terminal.
- Now, we have to run the flow.tcl file in an interactive manner so type the command ./flow.tcl -interactive.
- The terminal should look as follows:
<p align="center"><img width="943" alt="Screenshot 2023-01-25 at 9 15 00 AM" src="https://user-images.githubusercontent.com/77117825/214511076-ed193a3d-733e-4b7a-9e2f-a35b3731a91e.png"></p>

- Type the command 
  - package require openlane 0.9 
  - prep -design picorv32a
  - run_synthesis

<img width="1094" alt="Screenshot 2023-01-25 at 9 30 28 AM" src="https://user-images.githubusercontent.com/77117825/214511875-9be4c32b-c807-4710-9d6c-a069d5914b69.png">
<img width="987" alt="Screenshot 2023-01-25 at 9 58 45 AM" src="https://user-images.githubusercontent.com/77117825/214511919-e63dd616-6b1a-4882-a8b7-3f427bc934c3.png">

- Exploring the openlane directories
<img width="1199" alt="Screenshot 2023-01-25 at 10 16 10 AM" src="https://user-images.githubusercontent.com/77117825/214512337-272ab523-c2f6-4237-abe2-39b29b1f0e88.png">

- Calculating Flip flop ratio

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214519023-1467de2e-0aaa-4257-b7d0-46751eca83e8.jpeg"></p>

<p align="center"><img width="599" alt="Screenshot 2023-01-25 at 10 05 28 AM" src="https://user-images.githubusercontent.com/77117825/214519070-20321543-8d9b-41aa-b043-40105fbd06f5.png"></p>

<p align="center"><img width="491" alt="Screenshot 2023-01-25 at 10 05 49 AM" src="https://user-images.githubusercontent.com/77117825/214519092-1621f703-a905-4375-bc45-3ddf1a0b02de.png"></p>


- Calculating buffer ratio 

<p align="center"><img width="384" alt="Screenshot 2023-01-25 at 3 36 21 PM" src="https://user-images.githubusercontent.com/77117825/214535566-9a227006-3c65-4604-9026-67b606774f60.png"></p>
- Buffer ratio = Total no of buffers/Total no of cells
  
  Buffer ratio = 0.1118
  
# Day 2: Good floorplan vs bad floorplan and introduction to library cells
On the second day of the workshop, we started the discussion with the chip floor planning considerations which consisted of the utilization factor, aspect ratio, concept of pre-placed cells, de-coupling capacitors, power planning and pin placement. We also discussed about netlist binding and optimization of placement of the components. Finally, we also had a look on the cell design and characterization flows.

# Part 1: Chip Floor planning considerations

# Lec 1 : Utilization factor and aspect ratio

<img width="1440" alt="Screenshot 2023-01-25 at 6 17 45 PM" src="https://user-images.githubusercontent.com/77117825/214571125-bee8d13f-5335-4ffd-b1a6-c97be654b7cb.png">

<img width="1440" alt="Screenshot 2023-01-25 at 6 19 59 PM" src="https://user-images.githubusercontent.com/77117825/214571205-90a3c6c0-d3a0-49cc-abfe-e12140afb214.png">

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214571682-efefe826-9cf2-44ca-9660-f5f26e13a06d.jpeg"></p>

# Lec 2: Concept of pre-placed cells
<p align="center"><img width="945" alt="Screenshot 2023-01-25 at 6 51 29 PM" src="https://user-images.githubusercontent.com/77117825/214579535-a1775812-091d-484e-82a3-06614c2df03f.png"></p>

<p align="center"><img width="817" alt="Screenshot 2023-01-25 at 7 15 14 PM" src="https://user-images.githubusercontent.com/77117825/214579569-1ee14d13-7f29-46d4-92e1-25af9abcbbc4.png"></p>

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214579644-88be07fc-0b49-4280-a8d4-3d3e1efa367e.jpeg"></p>





