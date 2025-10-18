# Day4 : CMOS Noise Margin robustness evaluation
---

### Introduction to Noise margin

 - Noise margin is the maximum noise voltage a CMOS circuit can tolerate without logic errors.
 - If noise amplitude is below the noise margin, it is attenuated through the logic gate, ensuring error-free signal transmission.

<div align="center"><img width="773" height="545" alt="image" src="https://github.com/user-attachments/assets/0d480818-96b6-4a05-bf7f-bee573fa8e48" /></div>

Ideal and Practical curve for Vout vs Vin. For practical case:
 - If Vin < Vil : then Vout will be between Vdd and Voh.
 - If Vin > Vih : then Vout will be between Vol and 0.

  <div align="center"> <img width="765" height="451" alt="image" src="https://github.com/user-attachments/assets/3e67e334-6a00-4b76-b52d-6d1703e8aa6b" /></div>

 - NMh (Noise Margin High) : Any voltage level in NMh, will be detected as logic 1.
 - NMl (Noise Margin Low) : Any voltage level in NMl, will be detected as logic 0.

<div align="center"><img width="878" height="525" alt="image" src="https://github.com/user-attachments/assets/3d858ef6-8908-411c-b028-d46779aebcee" /></div>

### Noise margin equation and summary

<div align="center"><img width="771" height="347" alt="image" src="https://github.com/user-attachments/assets/7238e45d-0139-4d53-bdac-764d8a588812" /></div>

- When PMOS size is increased relative to NMOS size, the ability of PMOS to hold logic '1' will become higher compared to ability of NMOS to hold logic '0'. That's why the NMh increase when compared to NMl.

### Simulations for Noise margin

SPICE deck:
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

 -  PFET W/L to NFET W/L = 2.77
 -  Sweeping Vin from 0 to 1.8 with a step of 0.01

   <div align="center"><img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/2ab43743-472c-47b6-a68c-f52683ea3620" /></div>

  Points where slope is 1:
  ```
x0 = 0.784444, y0 = 1.68537

x0 = 0.983333, y0 = 0.097561
 ```

Voh = 1.68537, Vih = 0.983333
So, NMh = Voh - Vih = 0.702037

Vil = 0.784444, Vol = 0.097561
So, NMl = Vil - Vol = 0.686883





