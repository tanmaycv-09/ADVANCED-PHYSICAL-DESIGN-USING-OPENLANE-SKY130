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

# Lec 3: De-coupling capacitors

- Representation of an complex circuit 

<p align="center"><img width="950" alt="Screenshot 2023-01-25 at 7 57 00 PM" src="https://user-images.githubusercontent.com/77117825/214592588-ca6ffc4a-9a49-4e8e-9f54-e30e3b41289d.png"></p>

- Noise Margin diagram 

<p align="center"><img width="888" alt="Screenshot 2023-01-25 at 8 00 07 PM" src="https://user-images.githubusercontent.com/77117825/214592652-dbeea47c-ed16-49fb-8000-05c67dda9d66.png"></p>

- Theory of de-coupling capacitors 

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214592805-3049692f-856c-4468-8ab3-19829fb5cd0f.jpeg"></p>

- Adding of de-coupling capacitors in the circuit

<img width="927" alt="Screenshot 2023-01-25 at 8 04 10 PM" src="https://user-images.githubusercontent.com/77117825/214592967-8e535289-bfa9-480c-9236-02c73c26ec7d.png">

# Lec 4: Power planning

- Single Power rail 

<p align="center"><img width="866" alt="Screenshot 2023-01-25 at 8 26 35 PM" src="https://user-images.githubusercontent.com/77117825/214598940-9fc0f2ff-4ffc-4c6c-b8e0-6fe1a37a4d01.png"></p>

- Power Bus 

<p align="center"><img width="835" alt="Screenshot 2023-01-25 at 8 28 19 PM" src="https://user-images.githubusercontent.com/77117825/214599044-788d31a0-29cd-484e-a418-79647a4465a6.png"></p>

- Theory behind power planning

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214599181-c3cb316a-8018-4085-87da-7047706033a7.jpeg"></p>

- Final View after placing vertical & horizontal power rails 

<p align="center"><img width="898" alt="Screenshot 2023-01-25 at 8 29 33 PM" src="https://user-images.githubusercontent.com/77117825/214599365-e221170b-3eb4-4e38-8fe6-6ff141fa45c3.png"></p>

# Lec 5: Pin placement and logical cell placement blockage

- Complete Design of the netlist 

<p align="center"><img width="685" alt="Screenshot 2023-01-25 at 8 55 05 PM" src="https://user-images.githubusercontent.com/77117825/214606609-738b3a08-775a-464d-b18c-4d9371ae8f6b.png"></p>

- Pin Placement along with logical cell placement blockage 

<p align="center"><img width="887" alt="Screenshot 2023-01-25 at 9 06 48 PM" src="https://user-images.githubusercontent.com/77117825/214606785-a5e6a00d-0b1c-4307-ad9c-c42504e383df.png"></p>

- Theory behind pin placement

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214606908-595c1063-8b87-443a-83a3-48569c65096a.jpeg"></p>

# Lec 6: Steps to run floorplan using OpenLANE

- After successfully running the synthesis we need to run the floorplan, for that the command is:

<p align="center"><img width="229" alt="Screenshot 2023-01-26 at 9 31 04 AM" src="https://user-images.githubusercontent.com/77117825/214756483-d2312d67-1ff0-4259-9d80-3539fde6aa97.png"></p>

- To check the floorplanning information, go to the "configuration" folder in openlane using the "cd" command and type the command less README.md in the terminal.

<p align="center"><img width="935" alt="Screenshot 2023-01-26 at 9 22 50 AM" src="https://user-images.githubusercontent.com/77117825/214756598-6620e033-812e-44eb-b0db-b47a0a33c873.png"></p>

- After running floorplan 

<p align="center"><img width="1096" alt="Screenshot 2023-01-26 at 9 33 18 AM" src="https://user-images.githubusercontent.com/77117825/214756665-7a36bce6-c58b-4387-bc55-cb5e3e2ce851.png"></p>

# Lec 7: Review floorplan files and steps to view floorplan

- To review the file created from running the floorplan command, go to the runs directory using the "cd" command and go further into the floorplan directory.
- Use ls -ltr command to check available files.
- After successfully running the floorplan we will check the changes in the run file 

<p align="center"><img width="722" alt="Screenshot 2023-01-26 at 9 49 58 AM" src="https://user-images.githubusercontent.com/77117825/214760067-550d71d1-52b3-4985-a717-3b264b7fbec4.png"></p>

- Now go to the "results/floorplan" directory and use the command ls -ltr to check the contents of this directory.
- Open the file "picorv32a.floorplan.def" using the "less" command.
- When opened the file looks as follow:

<p align="center"><img width="599" alt="Screenshot 2023-01-26 at 9 54 23 AM" src="https://user-images.githubusercontent.com/77117825/214760231-04258911-4eaa-4631-a2d9-df7aa2adcca2.png"></p>

- The DIEAREA ( 0 0 ) ( 660685 671405 ) ; gives us the co-ordinates of the left bottom corner and right top corner of the chip.
- The UNITS DISTANCE MICRONS 1000 ; says that one micron equals thousand database units.
- Exit it by pressing "Q" button on the keyboard.
- Open the "merged.lef" file in magic using the command magic -T 

<p align="center"><img width="906" alt="Screenshot 2023-01-26 at 10 07 32 AM" src="https://user-images.githubusercontent.com/77117825/214760479-0258d62c-6de7-4b19-bc63-680a5210fb5b.png"></P>

- When the magic terminal opens it looks as follows:

<p align="center"><img width="984" alt="Screenshot 2023-01-26 at 10 19 07 AM" src="https://user-images.githubusercontent.com/77117825/214760809-19892a34-0d8a-4d4c-9b02-684f7c090d20.png"></p>

<p align="center"><img width="1076" alt="Screenshot 2023-01-26 at 10 19 25 AM" src="https://user-images.githubusercontent.com/77117825/214760821-593e3572-23c3-4594-9f43-4a6333c2dc11.png"></p>

# Lec 8: Review floorplan layout in Magic

- General commands to follow in magic to move around the layout are :

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214763997-49a041e3-fbcc-467e-8639-66b51ca7524f.jpeg"></p>

- to know any specific cell just place the mouse over it press S and then in the tkcon window type "what"

<p align="center"><img width="575" alt="Screenshot 2023-01-26 at 10 38 51 AM" src="https://user-images.githubusercontent.com/77117825/214764146-0ea26a67-7a32-4932-a986-882c88b789ba.png"></p>

# Part 2: Library Binding and Placement

# Lec 1: Netlist binding and initial place design

- definition of Library 

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214769096-aef1d550-d387-4a7d-a134-a7b733a5a5d2.jpeg".</p>
 
- Placement of the netlist in the floorplan
 
<p align="center"> <img width="971" alt="Screenshot 2023-01-26 at 11 30 17 AM" src="https://user-images.githubusercontent.com/77117825/214769245-d90f6362-d485-4044-b776-ff1d5ae5bf16.png"></p>
 
- For the first two flip-flop lines the placement could be like:

<p align="center"> <img width="968" alt="Screenshot 2023-01-26 at 11 33 45 AM" src="https://user-images.githubusercontent.com/77117825/214769420-cf119ad6-a043-4adb-bd33-fb71f8c4b9ff.png"></p>

# Lec 2: Optimize placement using estimated wire-length and capacitance

- Theory behind optimization :

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214771437-eda4bc02-d3f4-4fee-b17d-49c3e0b533e4.jpeg"></p>

- Optimized Placement of cells look like :

<p align="center"><img width="972" alt="Screenshot 2023-01-26 at 11 55 48 AM" src="https://user-images.githubusercontent.com/77117825/214771489-fe8d2e28-c7a8-4965-99f1-24bb101b9c41.png"></p>

# Lec 3: Final placement optimization

- Theory behind optimization like such :

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214773354-c1791cf3-7d32-49b1-81a4-9668d4c98920.jpeg"></p>

- After optimization the overview looks like : 

<p align="center"><img width="970" alt="Screenshot 2023-01-26 at 12 08 45 PM" src="https://user-images.githubusercontent.com/77117825/214774662-efb42248-9a88-41ff-afaa-da2156d953ba.png"></p>

# Lec 4: Need for libraries and characterization

- Typical IC design flow:

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214774795-ee8cc87b-3404-4a24-b718-6860a1032c07.jpeg"></p>

- Logic Synthesis: Arrangement of gates that will represent the original signal functionality described by the RTL.
- Floor Planning: We import the output of the logic synthesis and decide the size of core and die.
- Placement: We take the particular logic cells present in the netlist and place it on the chip in such a fashion that initial timing is met.
- Clock Tree Synthesis: The clock is being spread across the logic cells at an equal time.
- Clock buffers will take care that the signal has equal rise and fall time.

# Lec 5: Congestion aware placement using RePlAce 

- Global Placement is coarse placement and no legalization is happening.
- Standard cells are placed in standard cell rows and they should be placed exactly in the rows.
- Now, after running the floor plan, execute the command run_placement

- First the global placement will occur.
- Main objective of the global placement is to reduce the wire length.
- In openLANE, the concept of HPWL (Half Parameter Wire Length) is used.
- Now, to observe the placement, go to the placement directory using the "cd" command.
- Use the command ls -ltr to check what files are present in the directory.
- Now type the following command in terminal: magic -T 

<p align="center"><img width="906" alt="Screenshot 2023-01-26 at 12 41 41 PM" src="https://user-images.githubusercontent.com/77117825/214778479-6df6c26a-639d-41f0-acce-8a5273b1f3a2.png"></p>

- This should open the magic terminal that looks like:

<p align="center"><img width="673" alt="Screenshot 2023-01-26 at 12 43 22 PM" src="https://user-images.githubusercontent.com/77117825/214779066-2dff1304-a478-4d2f-abb9-b823c0e2b58c.png"></p>

- Zoom in the layout :

<p align="center"><img width="1419" alt="Screenshot 2023-01-26 at 12 46 46 PM" src="https://user-images.githubusercontent.com/77117825/214778613-13ac2d35-7f4e-4c8d-bfc1-802bf7390649.png"></p>

# Part 3: Cell design and characterization flows

- Euler's path is the path that is traced only once.
- Characterization flow includes:
   - Read the models
   - Read the extracted SPICE netlist
   - Recognize the behaviour of the circuit
   - Read the subcircuits
   - Attach the necessary power source
   - Apply the stimulus
   - Provide necessary output capacitance
   - Provide necessary simulation command
 - Feed in all the above steps as a configuration file to the characterization software called "GUNA".
 - Software will generate timing, noise, power .libs, function
 - Characterization of .libs:
   - Timing characterization
   - Noise characterization
   - Power characterization

<p align="center"><img width="911" alt="Screenshot 2023-01-26 at 4 08 39 PM" src="https://user-images.githubusercontent.com/77117825/214816470-c349a010-2ade-45e4-853f-44a15343d61e.png"></p>

# Part 4: General timing characterization parameters

- Timing Threshold Definitions:
  - slew_low_rise_thr:
    - Defines the point towards the lower set of the rising curve of the output.
    - Typically 20% of Vdd.
  - slew_high_rise_thr:
    - Defines the point towards the higher set of the rising curve of the output.
    - Typically 80% of Vdd.
  - slew_low_fall_thr:
    - Defines the point towards the lower set of the falling curve of the output.
    - Typically 20% of Vdd.
  - slew_high_fall_thr:
    - Defines the point towards the higher set of the falling curve of the output.
    - Typically 80% of Vdd.
  - in_rise_thr:
    - Defines the point towards the centre of the rising curve of the input.
    - Typically 50%
  - in_fall_thr:
    - Defines the point towards the centre of the falling curve of the input.
    - Typically 50%
  - out_rise_thr:
    - Defines the point towards the centre of the rising curve of the output.
    - Typically 50%
  - out_fall_thr:
    - Defines the point towards the centre of the falling curve of the output.
    - Typically 50%
- The formula of propogation delay becomes:

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214826211-fb6ef497-f259-4eb0-ae63-6c31dca70a1c.png"></p>

- If output fall threshold is chosen then input rise threshold must be chosen and if output rise threshold is chosen then input fall threshold must be chosen.

- The delay should always come positive.

- Negative delay is not expected and if negative delay is received then it is due to poor choice of threshold points.

- The formula for Transition time becomes:

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214826316-a0aa64fc-5bd4-404a-bf9c-a3f611b473e2.png"></p>

# Day 3: Design library cell using Magic Layout and ngspice characterization

# Lec 0: IO placer revision

- First, we will look at the input-output placer.
- In OpenLANE follow the steps upto floorplan as discussed earlier.
- If we run the magic file command, then we had got the output file with equidistand input-output pins as follow:

<p align="center"><img width="1440" alt="Screenshot 2023-01-27 at 8 31 57 AM" src="https://user-images.githubusercontent.com/77117825/215003533-c8ce0237-0025-48a0-8216-d3568daf81f7.png"></p>

- To change the configurations of the pin change it in floorplan.tcl file

<p align="center"><img width="737" alt="Screenshot 2023-01-27 at 8 38 10 AM" src="https://user-images.githubusercontent.com/77117825/215003596-4f645a18-5c5b-4d23-af9a-0bbda26fbe35.png"></p>

- Now i am copying the env(FP_io_mode)

<p align="center"><img width="1046" alt="Screenshot 2023-01-27 at 8 39 56 AM" src="https://user-images.githubusercontent.com/77117825/215003710-7d37e06a-16b7-4dab-989c-b671bb97e8ce.png"></p>

- Setting the mode to 2:

<p align="center"><img width="743" alt="Screenshot 2023-01-27 at 8 42 01 AM" src="https://user-images.githubusercontent.com/77117825/215003748-8ca018db-277a-43d3-a4ad-4aaf4e84d80a.png"></p>

- Resultant changes can be seen :
- The pins are more closer now :

<p align="center"><img width="1440" alt="Screenshot 2023-01-27 at 8 48 25 AM" src="https://user-images.githubusercontent.com/77117825/215003777-c6ae2afc-d6f0-4028-8d97-d2e66ffba45e.png"></p>

<p align="center"><img width="1432" alt="Screenshot 2023-01-27 at 9 00 11 AM" src="https://user-images.githubusercontent.com/77117825/215003983-cea00f94-d007-49dd-993a-5ab20fc26767.png"></p>

# Lec 1: SPICE deck creation for CMOS inverter

- To create a SPICE deck for a CMOS inverter we must complete the following steps:
   - Define component connectivity
   - Declare component values
   - Identify nodes
   - Name nodes
- Nodes are those two points in between which there is a component.
- We create the following netlist:

<p align="center"><img width="970" alt="Screenshot 2023-01-27 at 9 11 45 AM" src="https://user-images.githubusercontent.com/77117825/215005947-b4965275-f12d-4155-b9be-f630893d92e4.png"></p>

# Lec 2: SPICE simulation lab for CMOS inverter

- The netlist has the following code:
   
        - ***MODEL Descriptions***
        - ***NETLIST Description***
        - M1 out in vdd vdd pmos w=0.375u l=0.25u
        - M2 out in 0 0 nmos w=0.375u l=0.25u
        - cload out 0 10f
        - Vdd vdd 0 2.5
        - Vin in 0 2.5
        - ***SIMULATION Commands***
        - .op
        - .dc Vin 0 2.5 2.5
        - ***.include tsmc_025um_model.mod***
        - .LIB "tsmc_025um_model.mod" CMOS_MODELS
        - .end

- Increasing the PMOS width shift the dc transfer characteristics plot to the right.

# Lec 3-4: Switching Threshold Vm

- Switching threshold is the point at which the device switches.
- It is the point where Vin = Vout
- Graphical method to find Vm is to draw a line across the graph of output voltage to input voltage of a CMOS inverter starting at the origin and ending at the opposite diagonal of the plot (basically a line with a 45 degree inclination with the x-axis). Now, the x-coordinate of the point of intersection of this line and the curve is the switching threshold.
- At Vm, both PMOS and NMOS are turned 'ON' because Vgs has almost crossed the threshold region for both of them.
- IdsP = - IdsN which means that IdsP + IdsN = 0

# Lec 5 : Lab steps to git clone vsdstdcelldesign

- Git clone vsdstdcelldesign repo 

<p align="center"><img width="735" alt="Screenshot 2023-01-27 at 10 29 14 AM" src="https://user-images.githubusercontent.com/77117825/215014768-fb6eb51d-5183-43b6-9b7a-ed41b97e74c3.png"></p>

- ls -ltr vsdstdcelldesign

<p align="center"><img width="726" alt="Screenshot 2023-01-27 at 10 31 05 AM" src="https://user-images.githubusercontent.com/77117825/215014861-38e82bd8-1c22-4fe3-9947-9d3d734badf6.png"></p>

- Copy sky130.tech file in vsdstdcelldesign

<p align="center"><img width="1370" alt="Screenshot 2023-01-27 at 10 41 41 AM" src="https://user-images.githubusercontent.com/77117825/215014976-ccafd645-f254-431c-964f-77377937c3dc.png"></p>

- After copying run the magic command

<p align="center"><img width="728" alt="Screenshot 2023-01-27 at 10 45 02 AM" src="https://user-images.githubusercontent.com/77117825/215015025-97e1e1f8-b879-41b3-aed8-efd458f9f316.png"></p>

- magic opens up and the inverter layout is printed

<p align="center"><img width="1341" alt="Screenshot 2023-01-27 at 10 45 28 AM" src="https://user-images.githubusercontent.com/77117825/215015083-3322467a-11f2-4b0e-a9c4-8b733a96705d.png"></p>

# Part 2: Inception of Layout and CMOS fabrication process

- There is a 16 mask CMOS process which includes:
   - Selecting a substrate
   - Creating active region for transistors
   - N-well and P-well formation
   - Formation of Gate
   - Lightly doped drain (LDD) formation
   - Source and drain formation
   - Steps to form contacts and local interconnects
   - Higher level metal formation
- In a magic layout, to check if there are connections between two parts of the circuit, press "S" button on keyboard 3 times.
- LEF is library exchanged format.
- Functionality of LEF is protecting the IP.

- Using S to identify parts of the layout:

<p align="center"><img width="1436" alt="Screenshot 2023-01-27 at 12 24 38 PM" src="https://user-images.githubusercontent.com/77117825/215028792-ab6674c9-7b2b-4e65-9f1b-4473ed0da5fa.png"></p>

<p align="center"><img width="1419" alt="Screenshot 2023-01-27 at 12 25 25 PM" src="https://user-images.githubusercontent.com/77117825/215028793-daaf62db-c05f-49a0-935c-fdc980f739ee.png"></p>

- extracting into spice 

<p align="center"><img width="723" alt="Screenshot 2023-01-27 at 12 53 19 PM" src="https://user-images.githubusercontent.com/77117825/215032163-dc798d46-ecd1-46f7-b109-a08d725cd9fa.png"></p>

- Changes can be seen in vsdstdcelldesign file 

<p align="center"><img width="730" alt="Screenshot 2023-01-27 at 12 57 26 PM" src="https://user-images.githubusercontent.com/77117825/215032369-3f751833-8ecc-447d-904a-af0fd0ed974f.png"></p>

<p align="center"><img width="673" alt="Screenshot 2023-01-27 at 12 57 45 PM" src="https://user-images.githubusercontent.com/77117825/215032449-f51e9f91-1536-4f65-b044-ce07c1993ecd.png"></p>

<p align="center"><img width="726" alt="Screenshot 2023-01-27 at 12 59 09 PM" src="https://user-images.githubusercontent.com/77117825/215032461-76e36145-6865-438f-902e-f6ce520305dd.png"></p>

- looking in the spice netlist 

<p align="center"><img width="734" alt="Screenshot 2023-01-27 at 12 59 56 PM" src="https://user-images.githubusercontent.com/77117825/215032467-e706c6f8-cbbb-472d-aede-5ed53d948e8f.png"></p>

# Part 3: Sky130 Tech File Labs

# Lec 1 :Lab steps to create final SPICE deck using Sky130 tech

- Look into the sky130_inv.spice using vim command;

<p align="center"><img width="726" alt="Screenshot 2023-01-27 at 12 59 09 PM" src="https://user-images.githubusercontent.com/77117825/215103452-a87d6584-8ba3-433d-9efd-ab606af434f9.png"></p>

<p align="center"><img width="734" alt="Screenshot 2023-01-27 at 12 59 56 PM" src="https://user-images.githubusercontent.com/77117825/215103518-5f74a22e-30d2-46ea-9ece-807d365a80d4.png"></p>

- Update the file 

<p align="center"><img width="851" alt="Screenshot 2023-01-27 at 7 27 50 PM" src="https://user-images.githubusercontent.com/77117825/215104384-c87d18ff-52be-4bbd-8d2d-a585aad6f540.png"></p>

- Use ngspice command to look into spice simulations 

<p align="center"><img width="745" alt="Screenshot 2023-01-27 at 6 42 11 PM" src="https://user-images.githubusercontent.com/77117825/215104517-82ae23bb-6125-48cb-8cd3-96f8f3226855.png"></p>


<p align="center"><img width="347" alt="Screenshot 2023-01-27 at 6 43 12 PM" src="https://user-images.githubusercontent.com/77117825/215104530-e13e33f2-9c50-43a3-9a1f-22449046eaa3.png"></p>



<p align="center"><img width="1405" alt="Screenshot 2023-01-27 at 6 46 14 PM" src="https://user-images.githubusercontent.com/77117825/215104547-c2e18af1-ce35-482c-b6f5-a2789c397dfe.png"></p>
- After adjusting capacitor values spikes in the simulation can be made less and now the waveforms after adjustment can be seen as:

<p align="center"><img width="1357" alt="Screenshot 2023-01-27 at 7 40 45 PM" src="https://user-images.githubusercontent.com/77117825/215107280-0ea5ac1a-395c-4e80-b143-86debea8c177.png"></p>

# Lec 2:Lab steps to characterize inverter using sky130 model files

- Theory behind characterization of a cell 

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/215109059-3703acf0-6a2f-42c2-a718-f839b57972cf.jpeg"</p>
 
 - Rise time Calculations :

<p align="center"><img width="797" alt="Screenshot 2023-01-27 at 7 56 07 PM" src="https://user-images.githubusercontent.com/77117825/215111811-873ac4a3-a00f-4b6f-a636-37b9f0be8486.png"></p>

<p align="center"><img width="1326" alt="Screenshot 2023-01-27 at 8 04 21 PM" src="https://user-images.githubusercontent.com/77117825/215111920-3aec44e6-d6aa-4a59-9028-c1190e64f114.png"></p>

- Time values at 0.66V and 2.64V (i.e. 20% & 80% of 3.3V)

<p align="center"><img width="293" alt="Screenshot 2023-01-27 at 7 58 59 PM" src="https://user-images.githubusercontent.com/77117825/215112222-fe26fb7c-4c6a-4f8d-b158-37d77ab7cd14.png"></p>

<p align="center"><img width="267" alt="Screenshot 2023-01-27 at 8 01 35 PM" src="https://user-images.githubusercontent.com/77117825/215112235-e5a879d8-c366-42a2-8cf7-eb1c129cd403.png"></p>

         2.2462 - 2.1824 = 0.0638ns

- Fall Time Calculations 

<p align="center"><img width="1270" alt="Screenshot 2023-01-27 at 8 13 48 PM" src="https://user-images.githubusercontent.com/77117825/215114817-91272ebc-fb6d-4858-a0a3-b5e64dde14a3.png"></p>


<p align="center"><img width="399" alt="Screenshot 2023-01-27 at 8 15 20 PM" src="https://user-images.githubusercontent.com/77117825/215114823-fe946d19-4bae-451c-b5df-854f08b1e4ca.png"></p>

<p align="center"><img width="296" alt="Screenshot 2023-01-27 at 8 15 46 PM" src="https://user-images.githubusercontent.com/77117825/215114824-832b47e0-fb8b-494b-b877-ee86e8311aa1.png"></p>
      
         4.0953 - 4.05291 = 0.04239ns

- Rise Cell Delay ( Propogation Rise Delay)


<p align="center"><img width="390" alt="Screenshot 2023-01-27 at 8 20 37 PM" src="https://user-images.githubusercontent.com/77117825/215116650-b65e0bb6-cc99-4d0f-b79b-427dae6b1ff0.png"></p>


<p align="center"><img width="295" alt="Screenshot 2023-01-27 at 8 21 17 PM" src="https://user-images.githubusercontent.com/77117825/215116651-e086de99-c9c2-487a-afd2-5e26d0102ebd.png"></p>

        2.21 - 2.15 = 0.06ns
        
- Fall Cell Delay ( Propogation Fall Delay)


<p align="center"><img width="396" alt="Screenshot 2023-01-27 at 8 24 39 PM" src="https://user-images.githubusercontent.com/77117825/215116959-5d87a5cd-b46e-4a18-a90a-22694dd493a2.png"></p>

<p align="center"><img width="313" alt="Screenshot 2023-01-27 at 8 24 26 PM" src="https://user-images.githubusercontent.com/77117825/215116974-35d65b40-970c-419b-8511-d86fe240f16d.png"></p>

            4.07 - 4.05 = 0.02ns
 
 
 # Lec 3-9 : Eplanation and a walk around the magic tool:
 
            http://opencircuitdesign.com/magic/
            
- Everything related to magic can be learnt from this site, It also has DRC rules which are very helpful to get a clear hands on the reasons behind those DRC errors.

- Some basic commands, more can be explored on the site 

<p align="center"><img width="1165" alt="Screenshot 2023-01-28 at 8 25 01 AM" src="https://user-images.githubusercontent.com/77117825/215238589-c89fa628-58fa-402d-a4ec-5573774c5f12.png"></p>

<p align="center"><img width="1060" alt="Screenshot 2023-01-28 at 8 26 06 AM" src="https://user-images.githubusercontent.com/77117825/215238591-2d0199b5-cf92-4885-9da0-5cec0c8b49bf.png"></p>

# Day 4 : Pre-layout timing analysis and importance of good clock tree

# Lec 1 :  Lab steps to convert grid info to track info

- Opening the .mag file 
<p align="center"><img width="875" alt="Screenshot 2023-01-28 at 8 46 40 AM" src="https://user-images.githubusercontent.com/77117825/215239557-4fbf9c7a-e5af-4da1-bc48-800727e457e7.png"></p>
<p align="center"><img width="1091" alt="Screenshot 2023-01-28 at 8 51 24 AM" src="https://user-images.githubusercontent.com/77117825/215239567-519de6e8-6500-4924-b908-8eade0eb985f.png"></p>

- Now our objective is to extract a lef file out of this .mag file and then see if that .lef file can be plugged into the picorv32a flow 
          - Guidelines 
          - i/p & o/p port must lie on the intersection of vertical & horizontal lines 
          - width of the std cell must be in the odd multiples of the track pitch & height should be in the odd multiple of vertical track pitch 
          
- opening the track file 
<p align="center"><img width="902" alt="Screenshot 2023-01-28 at 9 00 11 AM" src="https://user-images.githubusercontent.com/77117825/215239947-df89b3f7-bc5d-41a1-9dc4-ff08ad17a849.png"></p>
<p align="center"><img width="217" alt="Screenshot 2023-01-28 at 9 00 33 AM" src="https://user-images.githubusercontent.com/77117825/215239952-81dea828-f40f-4cd1-9d0e-c4968c1e75f5.png"></p>

              PnR is an automated flow, tracks is to specify where all you want your routes to go 

- Now the step is to check if the tracks are placed correctly by using this command & then checking the layout window

<p align="center"><img width="614" alt="Screenshot 2023-01-28 at 9 08 33 AM" src="https://user-images.githubusercontent.com/77117825/215240630-ed9b225b-5eb1-410c-b884-8edddd69fd01.png"></p>

<p align="center"><img width="361" alt="Screenshot 2023-01-28 at 9 12 05 AM" src="https://user-images.githubusercontent.com/77117825/215240634-c05cc72d-73fa-4be3-8e37-5dd2ccb59c0d.png"></p>


<p align="center"><img width="478" alt="Screenshot 2023-01-28 at 9 13 05 AM" src="https://user-images.githubusercontent.com/77117825/215240639-9a996a2c-b11e-4444-84e9-84cccb384526.png"></p>

# Lec 2: Lab steps to convert magic layout to std cell LEF
- width of std cell must be in the odd multiples of x-pitch 

- Creating .lef file 

<img width="871" alt="Screenshot 2023-01-28 at 9 29 24 AM" src="https://user-images.githubusercontent.com/77117825/215241432-33a01447-6039-4e06-a459-ea9e1c01acb3.png">

<p align="center"><img width="858" alt="Screenshot 2023-01-28 at 9 30 44 AM" src="https://user-images.githubusercontent.com/77117825/215241100-85dd1794-aa18-4486-b138-0ecaae27b576.png"></p>

<p align="center"><img width="528" alt="Screenshot 2023-01-28 at 9 32 27 AM" src="https://user-images.githubusercontent.com/77117825/215241400-ff52e967-828d-4b19-a696-554920f84665.png"></p>

<p align="center"><img width="748" alt="Screenshot 2023-01-28 at 9 34 35 AM" src="https://user-images.githubusercontent.com/77117825/215241144-b2231870-bc3b-40dd-a760-1c8ef1ec3197.png"></p>

                So our .lef file is ready now next step is to plug this .lef file into picorv32a flow
                

# Lec 3 : Introduction to timing libs and steps to include new cell in synthesis

- Copying the .lef file into openlane/designs/picorv32a/src

<p align="center"><img width="859" alt="Screenshot 2023-01-28 at 9 54 26 AM" src="https://user-images.githubusercontent.com/77117825/215241923-b965f625-d967-42b5-88c1-5ecb128f0cf3.png"></p>

<p align="center"><img width="910" alt="Screenshot 2023-01-28 at 9 55 35 AM" src="https://user-images.githubusercontent.com/77117825/215241924-847d2b8f-6184-4e4a-99ee-6f56e3e946d2.png"></p>

<p align="center"><img width="898" alt="Screenshot 2023-01-28 at 10 13 44 AM" src="https://user-images.githubusercontent.com/77117825/215251474-f91a1fa1-ed14-4b84-9058-1f0170a206fb.png"></p>

- Updating the config.tcl file 

<p align="center"><img width="874" alt="Screenshot 2023-01-28 at 12 19 31 PM" src="https://user-images.githubusercontent.com/77117825/215251604-019be74a-b64c-49ad-9baf-76f3aca14b2c.png"></p>

- Now in new terminal again run docker through openlane and run these steps 
               
               docker 
               ./flow.tcl -interactive
               package require openlane 0.9 
               prep -design picorv32a -tag "current file" -overwrite 
               run_synthesis
               
               set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
  
               add_lefs -src $lefs
               
               
               
<p align="center"><img width="722" alt="Screenshot 2023-01-28 at 12 05 52 PM" src="https://user-images.githubusercontent.com/77117825/215251484-22b62e9a-840c-4993-ac2a-9b2d9bb1da3d.png"></p>

<p align="center"><img width="721" alt="Screenshot 2023-01-28 at 12 16 09 PM" src="https://user-images.githubusercontent.com/77117825/215251485-325931ca-1835-4c18-b561-bfe635423ac3.png"></p>

<p align="center"><img width="736" alt="Screenshot 2023-01-28 at 12 11 07 PM" src="https://user-images.githubusercontent.com/77117825/215251488-2be6b5f0-38b2-4762-8faf-e3917c93807d.png"></p>

# Lec 4: Introduction to delay tables

- Theory :

<p align="center"><img src="https://user-images.githubusercontent.com/77117825/215253457-77805649-3deb-429f-ace2-7447afd38947.jpeg"></p>

<p align="center"><img width="961" alt="Screenshot 2023-01-28 at 1 01 46 PM" src="https://user-images.githubusercontent.com/77117825/215253458-e559bf8b-3387-4c69-b1ac-3ba074a25038.png"></p>

# Lab :  Lab steps to configure synthesis settings to fix slack and include vsdinv

- if you run_floorplan you will face this error

<p align="center"><img width="731" alt="Screenshot 2023-01-28 at 7 50 35 PM" src="https://user-images.githubusercontent.com/77117825/215279884-3e5aad70-5a35-438e-9ceb-6854872d39cb.png"></p>

<p align="center"><img width="730" alt="Screenshot 2023-01-28 at 7 51 22 PM" src="https://user-images.githubusercontent.com/77117825/215279966-911b02b9-6a98-4ec5-8e4a-c52baa38f770.png"></p>

- Follow these steps 

         init_floorplan
         place_io
         global_placement_or
         detailed_placement
         tap_decap_or
         detailed_placement
         gen_pdn
         
<p align="center"><img width="723" alt="Screenshot 2023-01-28 at 10 24 32 PM" src="https://user-images.githubusercontent.com/77117825/215280121-69fdfdd4-9df6-47d2-8158-dcd9178302c5.png"></p>

- Magic opens up 

<p align="center"><img width="1435" alt="Screenshot 2023-01-28 at 9 26 28 PM" src="https://user-images.githubusercontent.com/77117825/215280200-576bd8c4-7c35-4128-9f81-e49102b12a94.png"></p>

<p align="center"><img width="1282" alt="Screenshot 2023-01-28 at 9 32 33 PM" src="https://user-images.githubusercontent.com/77117825/215281771-5e72ff50-a4f2-446a-b7b6-ca2e86cf7b8f.png"></p>

<p align="center"><img width="1419" alt="Screenshot 2023-01-28 at 10 12 49 PM" src="https://user-images.githubusercontent.com/77117825/215282150-705cc597-2392-4dd3-b63c-107306ccde61.png"></p>

<p align="center"><img width="685" alt="Screenshot 2023-01-28 at 10 15 13 PM" src="https://user-images.githubusercontent.com/77117825/215281935-437260b3-c59f-4eb5-b71b-23b47888c576.png"></p>



# part 2 : Timing analysis with ideal clocks using openSTA

# Lec 1 :  Setup timing analysis and introduction to flip-flop setup time







