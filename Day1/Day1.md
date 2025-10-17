# Day1
---

## Introduction to Circuit design & SPICE simulations

### Need for SPICE simulations
The logic gates that are used implement the designs that we have discussed before are realized using combnations of transistors (NMOS and PMOS) in different fashions. SPICE simulations are used to observe and characterize the behaviour of these circuits (for eg. how the output current varies for different values of input current, what is the delay involved etc.).

## Introduction to basic element in circuit design - NMOS transistor

<div align="center"><img width="876" height="397" alt="nmos" src="https://github.com/user-attachments/assets/65ee20d3-8526-4b10-b6e0-c74d591c5270" /></div> 

NMOS, N-Channel Metal Oxide Semiconductor:
- 4 terminal device
- P substrate
- n+ diffusion region
- Gate Oxide
- Poly-Si or metal gate
- Terminals :
    - Gate
    - Source
    - Drain
    - Body
      
### Threshold Voltage

- When the source, drain and the body terminals are grounded and the Gate to source voltage (Vgs) is made positive, the region of P substrate under the gate region gets depleted of positive charge carriers. When Vgs is increased further, the depletion region increases and at a point the surface of the substrate near the gate accumulates -ve charge carriers. This is called **strong inversion** and the value of Vgs at which inversion happens is called **threshold volatge**.
- When increasing Vgs further, there won't be increase in   depletion layer width, but electrons from the heavily doped source region are drawn to the  region under gate. This forms a continuous N channel between source and drain. The conductivity of this channel can be controlled by Vgs.
- When there is voltage between source and body (Vsb a +ve value) it increases the depletion region between source and body and also causes the value of threshold voltage to go up. The equation governing the thresold voltage will be:
<div align="center"><img width="475" height="94" alt="thres_Vsb" src="https://github.com/user-attachments/assets/5d2fe3b2-7d29-4d85-b549-bde31f5ae725" />
</div> 
   
  - Vto : Thresold voltage for Vsb = 0. 
  - $\gamma$ : Body effect coefficient.
  - $\Phi$<sub>f</sub> : Fermi potential
  - Vsb : Voltage between body and source

  Vto, $\gamma$,  $\Phi$<sub>f</sub> are constants particular to the foundry where the ICs are manufactured. These constants are the models that are given to a SPICE simulation.

<div align="center"><img width="653" height="395" alt="thes_Vsb_1" src="https://github.com/user-attachments/assets/4651b72f-4b51-4fd3-99bb-712f7fb96b4c" />
</div>


### Regions of operation for NMOS : Resistive & Saturaion regions

#### Resistive region of operation with small drain-source voltage

<div align="center"><img src="images/resistor_op.png" alt="Alt Text" width="500"/> </div>

#### SPICE conclusion for resistive operation

Impact of Vds and Vgs on drain current
  
  - As Vgs is increased the range of values of Vds for which the device stays in linear region of operation increases:
    <div align="center"><img width="1501" height="672" alt="resistor_op" src="https://github.com/user-attachments/assets/2a788831-1ed2-4705-8d9a-a669abc3642a" /></div>

    ? How do we calculate Id for different values of Vgs and at every value Vgs, sweep Vds for 0 to (Vgs - Vt) using linear equation for Id?
    - Done using SPICE simulations
   
#### Saturation Region

When channel voltage (Vgs - Vds) becomes less than Vt (Threshold voltage). This prevents the happening of inversion and channel disappeares near to the drain terminal.

 <div align="center"><img width="577" height="461" alt="image" src="https://github.com/user-attachments/assets/6cbe30f6-f364-490c-8528-9d2a1de6a7fd" /></div>

 This is called "Pinch-Off" point. 
 Pinch-Off condition : Vgs - Vds <= Vt

 When Vds is increased beyond pinch-off point (i.e. Vds = Vgs - Vt) the drain current Id becomes independant of Vds and only depends on Vgs - Vt.



### Introduction to SPICE
SPICE stands for Simulation Program with Integrated Circuit Emphasis. SPICE is used to simulate the behaviour of electronic circuits in order to analyze various parameters related to the circuit such as voltage, current, power consumption, delay etc.

 Creating the correct SPICE setup.

- The constants in the equations that comes as a part of fabrication technology are to be provided to the SPICE tool as **SPICE model parameters**.
 <div align="center"><img width="881" height="547" alt="image" src="https://github.com/user-attachments/assets/afd35dab-2f77-4722-a59d-3878610b533c" /></div>

- The circuit for which SPICE simulations are to be carried out is are defined in the software as **SPICE netlist**.
<div align="center"><img width="523" height="297" alt="image" src="https://github.com/user-attachments/assets/b22622d2-7890-43d5-a9b4-870371096e83" /></div>

  - Define nodes in the circuit : Nodes are points between two components in the circuit.
    <div align="center"><img width="507" height="343" alt="image" src="https://github.com/user-attachments/assets/7db2f791-d0a9-4dff-910f-b067cff0c2c7" /></div>

  - Defining different components:
    - MOSFET : ```M1 vdd n1 0 0 nmos W=1.8u L=1.2u ```
        - ```M1``` naming for the MOSFET.
        -  Drain connected to node ```vdd```.
        -  Gate connected to node ```n1```.
        -  Source connected to node ```0```.
        -  Body connected to node ```0```.
        -  Name of the device as per technology file.
        -  Width will be ```1.8u```.
        -  Length will be ```1.2u```.
    - Resistance : ```R1 in n1 55```
        - ```in``` is the connection of first terminal.
        - ```n1``` is the connection of second terminal.
        - ```55``` value of the resistor is 55 ohms.
    - Voltage source for Vdd : ```Vdd vdd 0 2.5```
        - ```vdd``` and ```0``` the nodes to which the terminals are connected.
        - ```2.5``` value of the voltage source.
    - Voltage source for Vin : ```Vin in 0 2.5```

      <div align="center"><img width="1175" height="363" alt="image" src="https://github.com/user-attachments/assets/91d0fee9-9705-4b1d-8c72-e3737d646564" /></div>

 - Model file for specifying the technology parameters
 <div align="center"> <img width="575" height="454" alt="image" src="https://github.com/user-attachments/assets/bdcdb36a-8da2-446f-8461-e40769e96f75" /></div>
 <div align="center"> <img width="530" height="316" alt="image" src="https://github.com/user-attachments/assets/df2d9a4c-fea9-40a5-b8d2-8b7bfc90b6a2" /></div>

  Calling the model file : ```.LIB "xxxx_025um_model.mod" CMOS_MODELS```

 - Simulation Commands : These specify the range of voltage values to sweep across for parameters such as Vds or Vgs etc.

### Doing Simulation in NGSPICE

To use the Sky130nm technology model files for simulations:
```$git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git```

3 Important Files in the Repo:

- ```/sky130CircuitDesignWorkshop/design/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.pm3.spice```
    This file contains the SPICE model for the NFET (N-channel MOSFET) in the Sky130 process at typical (tt) conditions.

- ```/sky130CircuitDesignWorkshop/design/sky130_fd_pr/cells/nfet_01v8/sky130_fd_pr__nfet_01v8__tt.corner.spice```
    This file provides the corner model for the NFET, used for simulating different process variations.

- ```/sky130CircuitDesignWorkshop/design/sky130_fd_pr/models/sky130.lib.pm3.spice```
    This library file contains all the SPICE models for components in the Sky130 process node.

#### Example
```spice
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt    *tt - typical corner, if needed slow fast corner put sf


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2              *sweeping Vds from 0 to 1.8 with a step of 0.1 and Vgs from 0 to 1.8 with a step of 0.2

.control

run
display
setplot dc1
.endc

.end
```


 - Running ngSPICE:
  ``` $ngspice day1_nfet_idvds_L2_W5.spice```

 - To plot Vds :
   ```plot -vdd#branch``` as the Vds is controlled by vdd value.
   
   <div align="center"><img width="1920" height="1020" alt="Screenshot 2025-10-17 003354" src="https://github.com/user-attachments/assets/aeafe19f-4ef1-4acf-a7c4-ee17a04f4fe2" /></div>

   This is the graph for Id vs Vds for different Vgs values.
   To get the values at particular points in the plot, click on that point, the corresponding x and y axis values will be shown in the terminal.

<div align="center"><img width="1387" height="711" alt="image" src="https://github.com/user-attachments/assets/a8d0d004-90b9-420d-a002-2fd29aed1de0" /></div>



