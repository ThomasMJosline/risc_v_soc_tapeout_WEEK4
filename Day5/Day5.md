# Day5 : CMOS power supply and device variation robustness evaluation
---

<div align="center"><img width="1101" height="308" alt="image" src="https://github.com/user-attachments/assets/165e3c65-8f07-45c7-943e-d4c06036ee11" /></div>
 
 - Simulate the CMOS inverter across different supply voltage values (VDD).
 - Analyze how the voltage transfer characteristics (VTC) and critical parameters (like VM and noise margins) shift with varying power supply.

Plot of Vout vs Vin for different values of Vdd:
<div align="center"><img width="765" height="555" alt="image" src="https://github.com/user-attachments/assets/c273548c-e4e2-46c6-812e-bd6c9fcdb315" /></div>

 - CMOS inverter is also able to operate at Vdd = 0.5V.
 - Gain factor for increases as Vdd is decreased

   Advantages of using 0.5V supply:
    - Increase in gain(close to 50% improvement)
    - Significant reduction in energy(close to 90% improvement)

   Disadvantage of using 0.5V supply:
    - The  rise time and fall time will be higher which causes the output not to settle on one of the logic level.


 ###  SPICE simulations:
SPICE Deck:
```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15


Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

.control

let powersupply = 1.8
alter Vdd = powersupply
        let voltagesupplyvariation = 0
        dowhile voltagesupplyvariation < 6
        dc Vin 0 1.8 0.01
        let powersupply = powersupply - 0.2
        alter Vdd = powersupply
        let voltagesupplyvariation = voltagesupplyvariation + 1
      end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "input voltage(V)" ylabel "output voltage(V)" title "Inveter dc characteristics as a function of supply voltage"

.endc

.end
```

 - Starting simulations with a Vdd of 1.8V.
 - Doing 6 iterations of decreasing Vdd by 0.2V.


  <div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/1b1d9569-29f6-4af8-a135-9fa63fe0a6a1" /></div>

---
  ### Variation due to Etching process

  - This process defines the structure of the circuit.

   <div align="center"> <img width="1154" height="516" alt="image" src="https://github.com/user-attachments/assets/f8e802b6-0c24-4c56-b1bb-f0db8c2e74e9" /></div>

   Inverter Chain:
   <div align="center"><img width="1352" height="683" alt="image" src="https://github.com/user-attachments/assets/c8f19440-3508-4f03-a2e1-8af432c466eb" /></div>

  Imperfections during fabrication:
   <div align="center"><img width="679" height="303" alt="image" src="https://github.com/user-attachments/assets/971284f4-7689-4be1-aec1-06e1a3eb0be3" /></div>

   - This can cause variation in drain current as Id depends on W/L.

### Variation due to Oxide thickness

 <div align="center"><img width="1199" height="458" alt="image" src="https://github.com/user-attachments/assets/476a6ef2-2f29-47de-a623-d1671f0e39ee" /></div>

 Oxide length along the channel may not be uniform due to fabrication:

 <div align="center"><img width="883" height="241" alt="image" src="https://github.com/user-attachments/assets/f4a744e4-0701-45b9-ad00-a9e9ee05111f" /></div>

  - This can also affect the drain current.

### Observing variation in Vout vs Vin due to process variations:

<div align="center"><img width="1180" height="303" alt="image" src="https://github.com/user-attachments/assets/b9df144a-467c-4f2e-ac98-7d75e7a10bec" /></div>

- Sweep the value of Wp and Wn along a range and observe the Vout vs Vin for each case.

Variation in switching threshold for each case:
<div align="center"><img width="764" height="561" alt="image" src="https://github.com/user-attachments/assets/d9d6a450-0d26-4759-ae0a-488347a1d20d" /></div>

Variation in noise margin:
<div align="center"><img width="952" height="566" alt="image" src="https://github.com/user-attachments/assets/ef06b26c-2c86-4076-a6d4-a3b7eb040c53" /></div>


 - CMOS inverter operation is intact even device variation occurs.

### SPICE simulations

SPICE deck:
```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description


XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15


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

 - PFET width very high compared to that of NFET.
 - Sweeping Vin from 0 to 1.8V.

<div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/48600edd-bd5d-496c-9db6-20349b288f39" /></div>

 - Here as the PFET have higher driving strength compared to NFET, which result in Vout vs Vin curve being high for more range of Vin.

 <div align="center"><img width="272" height="31" alt="image" src="https://github.com/user-attachments/assets/1cb6fda8-f618-4f48-b9d1-a8443bdc96a5" /></div>

 Here the switching threshold is near 980mV which is not a high variation. 
 This shows that the CMOS inverter is robust device.





