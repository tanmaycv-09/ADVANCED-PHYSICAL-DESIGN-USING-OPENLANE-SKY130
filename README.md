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

<img width="663" alt="Screenshot 2023-01-24 at 6 44 28 PM" src="https://user-images.githubusercontent.com/77117825/214500708-fd98af28-4fe0-4e2f-b02f-f437ac4ee65b.png">

- PDK is the interface between the FAB and the designers.
- PDK stands for "Process Design Kit".
- PDK is a collection of files used to model a fabrication process for the EDA tools used to design an IC.

# Lec 2: Simplified RTL2GDS flow
<img width="912" alt="Screenshot 2023-01-24 at 6 47 38 PM" src="https://user-images.githubusercontent.com/77117825/214501250-7c22b54d-4a95-44df-85f4-088355f3e2f4.png">
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214501627-3c52f564-7640-43cc-a5e5-2ff9fc924b83.jpeg"></p>
<p align="center"><img src="https://user-images.githubusercontent.com/77117825/214501653-87e0da2f-9f91-4697-8d85-1dab0690720d.jpeg"></p>





