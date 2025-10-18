# Day3 : CMOS switching threshold and dynamic solutions
---

### Voltage transfer characteristics for CMOS inverter : SPICE Simulations

<div align="center"><img width="580" height="510" alt="image" src="https://github.com/user-attachments/assets/0a3e764e-eb9e-4f3c-abbb-d1318f27f303" /></div>

SPICE deck for the above circuit:
 - Component connectivity
 - Component values
 - Identify the nodes
 - Name the nodes

#### SPICE deck for simulation:
<div align="center"><img width="658" height="499" alt="image" src="https://github.com/user-attachments/assets/0e4865f8-3be7-4b3b-a0cf-5d12340082b7" /></div>

Voltage Transfer charcteristics from SPICE simulations (Vout vs Vin):
<div align="center"><img width="784" height="594" alt="image" src="https://github.com/user-attachments/assets/96609dac-af8e-42b5-8131-fb15b7ce5490" /></div>

### Simulation
SPICE deck:

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op

.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

 - Simulation for typical corner (tt).
 - PFET with Width = 0.84, Length = 0.15.
 - NFET with Width = 0.36 , Length = 0.15.

Running the SPICE simulation:
```
$ngspice day3_inv_vtc_Wp084_Wn036.spice
```
Plotting Vout vs Vin:
```
plot out vs in
```

<div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/af896fd0-6596-45ce-a300-293597f7a909" /></div>

Switching Threshold: point where Vout and Vin are same. 
<div align="center"><img width="376" height="40" alt="image" src="https://github.com/user-attachments/assets/8c08fc28-1c71-40a0-b874-0e0de0501d3d" /></div>
Here x axis is Vin and y axis is Vout.  

#### Transient analysis
SPICE deck:
```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*simulation commands

.tran 1n 10n

.control
run
.endc

.end
```

 - Simulation for typical corner (tt).
 - PFET with Width = 0.84, Length = 0.15.
 - NFET with Width = 0.36 , Length = 0.15.
 - Input is a pulse: ```Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns) ```:
   - Pulse from 0 to 1.8V
   - Shifted by 0
   - Rise time 0.1ns
   - Fall time 0.1ns
   - Pulse width 2ns
   - Total duration 4ns

Running the SPICE simulation:
```
$ngspice day3_inv_vtc_Wp084_Wn036.spice
```
Plotting Vout vs Vin:
```
plot out vs time in
```

<div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/9912c4ee-9b9a-4e86-b50e-24bfa4dffef0" /></div>

 - Red plot : output
 - Blue plot : input

To calculate rise delay: measure the time difference between input getting half the max and output getting half the max, which is 0.9V, when output is rising.
<div align="center"><img width="318" height="59" alt="image" src="https://github.com/user-attachments/assets/2d241778-3fc4-42f2-9101-5169d3b95380" /></div>
 Rise delay = 2.48602e-09 - 2.15269e-09 = 0.334ns

To calculate fall delay: measure the time difference between input getting half the max and output getting half the max, which is 0.9V, when output is falling.
<div align="center"><img width="282" height="65" alt="image" src="https://github.com/user-attachments/assets/e196dcce-8779-4026-b348-b1c5f8bab775" /></div>
 Rise delay = 4.33846e-09 - 4.05641e-09 = 0.282ns
 

### Static behaviour evaluation - CMOS inverter robustness - Switching thresold

<div align="center"><img width="1252" height="657" alt="image" src="https://github.com/user-attachments/assets/b6f784e7-e35c-45b6-b284-293cd8f722d5" /></div>

Switching threshold values are marked in both cases


   <div align="center"><img width="1270" height="658" alt="image" src="https://github.com/user-attachments/assets/ebef2266-ef8d-4fed-bb52-82aaccc7ec69" /></div>

 - Finding the value of switching threshold for different values of width and length
<div align="center"><img width="863" height="239" alt="image" src="https://github.com/user-attachments/assets/9c0dac75-381e-4c11-ab09-b0678c0ef185" /></div>

- Finding the value of width and length for a fixed value of switching voltage
  <div align="center"><img width="949" height="230" alt="image" src="https://github.com/user-attachments/assets/edbfc46a-d793-4d53-92df-e14cd3e78f7e" /></div>

  Fixing Vm and determining W and L by determning Kn and Kp:
  <div align="center"><img width="455" height="98" alt="image" src="https://github.com/user-attachments/assets/4b761937-360f-4a1f-9a01-2886707fff36" /></div>
 <div align="center"> <img width="524" height="127" alt="image" src="https://github.com/user-attachments/assets/6ff2641b-5dd8-4989-b7c8-4de3af7df34a" /></div>
 <div align="center"><img width="530" height="144" alt="image" src="https://github.com/user-attachments/assets/8e4481e5-b702-4158-aeda-b1392a005240" /></div>

 #### Checking Switching Potential for different ratios of W/L of PMOS to that of NMOS:
 <div align="center"><img width="416" height="173" alt="image" src="https://github.com/user-attachments/assets/73f9ad2d-5399-406e-b1d6-92080c3f5642" /></div>
 <div align="center"><img width="416" height="173" alt="image" src="https://github.com/user-attachments/assets/e59ac07c-1448-4852-8a10-74a1f174a01b" /></div>
 <div align="center"><img width="416" height="173" alt="image" src="https://github.com/user-attachments/assets/ab62da10-1a27-4402-82c2-836b75d9b935" /></div>
 <div align="center"><img width="416" height="173" alt="image" src="https://github.com/user-attachments/assets/a6d843a0-b2fe-49c9-9a5c-07444a127b52" /></div>
 <div align="center"><img width="416" height="173" alt="image" src="https://github.com/user-attachments/assets/2fcf64c3-0e50-4a6c-b70b-133c49f28bb9" /></div>

<div align="center"><img width="771" height="287" alt="image" src="https://github.com/user-attachments/assets/23eb92ca-75a8-450d-8127-801e0ba8ecc4" /></div>

Observations:
 - Increasing the ratio between Wp/Lp and Wn/Ln increases the switching threshold.
 - For Wp/Lp = 2 * (Wn/Ln) the rise time is almost equal to fall time. This is the expected characteristic for a inverter to be used in clock cell.


 

 



 




