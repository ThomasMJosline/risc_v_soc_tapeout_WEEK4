# Day2
---
# SPICE simulation for lower nodes and Velocity saturation effect

## SPICE simulation for lower nodes

<div align="center"><img width="1185" height="697" alt="image" src="https://github.com/user-attachments/assets/721eeff0-8cb9-47a1-8fe4-e3416339b316" />
</div>

### Observing Id vs Vds for W = 0.375u and L = 0.25u (W/L = 1.5 as before)
<div align="center"><img width="813" height="648" alt="image" src="https://github.com/user-attachments/assets/0400fedc-3894-40e2-b4ab-2744e2d69404" /></div>

 - Observation 1
   For short channel device the increase in Id with Vgs is linear, but for longer channels the relation is quadratic. This called **Velocity Saturation Effect**

   Plot of variation of Id with Vgs for a fixed value of Vds for long channel and short channel:
   For sweeping Vgs from 0 to 2.5 for fixed value of Vds(here 2.5)
   
   ```.dc Vin 0 2.5 0.1 Vdd 0 2.5 2.5 ```
   
   <div align="center"><img width="1345" height="651" alt="image" src="https://github.com/user-attachments/assets/1b389141-8a03-4513-8ef4-cae6a340f0aa" /></div>

  At lower electric fields, carrier velocity increases linearly with the electric field.
  At higher electric fields, velocity saturates and becomes constant due to velocity saturation.

  <div align="center"><img width="1026" height="376" alt="image" src="https://github.com/user-attachments/assets/f56d55f6-f89e-4226-bf21-3852c6959529" /></div>
   <div align="center"><img width="440" height="139" alt="image" src="https://github.com/user-attachments/assets/24c6545b-162b-4cd4-a6c4-87671a900a17" /></div>

   Re-deriving drain current with considering Velocity saturation:
   <div align="center"><img width="518" height="128" alt="image" src="https://github.com/user-attachments/assets/1220a23b-ac43-4da3-823c-4f75ed6fef90" /></div>
   Simplying this:

   <div align="center"><img width="1011" height="563" alt="image" src="https://github.com/user-attachments/assets/634aa887-6b83-4d5e-b1bd-b6944ce6c542" /></div>

   Different cases for Vmin:
   - Vmin = Vgt
  <div align="center"><img width="376" height="99" alt="image" src="https://github.com/user-attachments/assets/e60ea049-410c-4963-9b3a-176eac12fd62" /></div>

  This is the case when device is in Saturation.

  - Vmin = Vds
  <div align="center"><img width="456" height="99" alt="image" src="https://github.com/user-attachments/assets/0f1eea37-b703-4b16-94ed-dc8d8077a8c2" /></div>

  The device is in linear mode of operation.

  - Vmin = Vdsat (only for short channel devices)
  <div align="center"><img width="500" height="110" alt="image" src="https://github.com/user-attachments/assets/fc8fd37f-ced5-4973-a399-771dce714d2e" /></div> 

  The device is in velocity saturation.

  <div align="center"><img width="1398" height="618" alt="image" src="https://github.com/user-attachments/assets/8f40f6ed-9d26-4682-b9da-25ce1ccb4089" /></div>

### SPICE simulations

Example

```
*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description



XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control

run
display
setplot dc1
.endc

.end
```

 - Simulation for W=0.39 and L=0.15.
 - Vds varied from 0 to 1.8 with a step of 0.1.
 - Vgs varied from 0 to 1.6 with a step of 0.2.

<div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/1c7f6d9a-4e11-4b6f-ba36-1c597774c38c" /></div>

 - For lower values of Vgs the increase in max Id with Vgs is quadratic, but when Vgs is higher the increment in max Id is linear.

 #### To observe Id vs Vgs

```
*Model Description
.param temp=27


*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt


*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15

R1 n1 in 55

Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vin 0 1.8 0.1

.control

run
display
setplot dc1
.endc

.end
```
- Simulation for W=0.39 and L=0.15.
- Vds is kept constant at 1.8V.
- Vgs is varied from 0 to 1.8 with a step of 0.1.

<div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/c73665c0-01c1-44d7-a1bd-3141ac51712f" /></div>

Here as L=0.15u, which is a short channel, the Id vs Vgs will be linear because of velocity saturation.

    

