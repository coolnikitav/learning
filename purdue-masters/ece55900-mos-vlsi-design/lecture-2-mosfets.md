# MOSFETs

## MOSFETs: Device Structure
- Q: Draw a diagram of an n-MOSFET? When is it on? When is it off? Where is the current flowing from and to?
- Q: Draw a diagram of an p-MOSFET? When is it on? When is it off? Where is the current flowing from and to? 
  
![image](https://github.com/user-attachments/assets/08510c29-ce29-4b74-b6d8-d2191fc8d6ac)

![image](https://github.com/user-attachments/assets/56a74cb2-af09-4b50-a48d-2c6c87df1c33)

- Substrate = body terminal
- n-MOSFET has electrons flowing
- p-MOSFET has holes flowing

## Band Diagrams: N-MOSFET
- Q: What is the equation for energy in this context?
- Q: What is DIBL?
  
![image](https://github.com/user-attachments/assets/999e1753-e71b-4ba3-8b6e-5581545b691e)

![image](https://github.com/user-attachments/assets/08254a91-80a4-4638-a8b3-5559aae26dc2)
- The leakage current increases due to DIBL.

We want the gate voltage to lower the barrier when the device is turned on:

![image](https://github.com/user-attachments/assets/041de298-006c-4520-85ba-37c40117c49a)

- Q1: Where does the fermi level move when you dope a semiconductor with holes to make it p-type? n-type?
- Q2: What are we looking at? What do the lines mean? What does the rectangle/rhombus mean?
![image](https://github.com/user-attachments/assets/2f551778-bb1e-4807-a9f6-d8304586b346)

- A1: Fermi level will move closer to the valence band. n-type is the opposite: towards the conduction band.

- Q: What happens to the previous band diagram when Vgs > 0? Does it become more n-type or p-type? Why?

![image](https://github.com/user-attachments/assets/41f5b444-a74a-412e-b7cf-e573bf9ebb12)

![image](https://github.com/user-attachments/assets/4314428e-c283-42d4-91cf-32db1d1e91a4)

## Threshold Voltage (Vth)
- Q: At Vgs = Vth, what is the relationship between surface and bulk?
- Q: At threshold, what is the relationship between psi S and psi B?
- Q: What is the equation that describes Vgs (ideal and non ideal)?
  
![image](https://github.com/user-attachments/assets/a3f1ae80-d374-4d7e-9e1d-0c5d81b035d8)

![image](https://github.com/user-attachments/assets/9458e601-a88d-4ef7-99ea-94fc92fa6277)

![image](https://github.com/user-attachments/assets/d2a0cbf3-98b5-46c1-aa06-4bc0d1fc006a)

## Body Effect (Non-Zero Source-To-Body Voltage)
- Q1: What should be the voltage of the body of an n-mosfet? What about a p-mosfet? Why?
- Q: How does the body effect impact the energy barrier?

![image](https://github.com/user-attachments/assets/34d5751f-9f11-4ee0-b840-d92dfabf1860)

- A1: Most negative, to make sure the diode in the pn junction is always reverse biased (off). Most positive

![image](https://github.com/user-attachments/assets/357382dd-4f47-46a9-ac9d-97733e7b62bb)

## Threshold Voltage With Body Effect (NMOS)
- Q: Write the equation for the threshold voltage without and with body effect.

![image](https://github.com/user-attachments/assets/b3a1e1a5-1b16-425d-95a2-a9209e09700f)

## Sub-Threshold Characteristics (Vgs < Vth)
- Q: How to express surface potential in terms of oxide capacitance, substrate capacitance, and Vgs?
- Q: What is the body factor? What is the equation for it?
- Q1: What should the voltage be set to in order to completely lower the barrier?

![image](https://github.com/user-attachments/assets/b830c526-b9f7-4c23-89e0-2815d086d542)

- A1: The threshold voltage, which corresponds to Eb.
  
The number of electrons that can be injected over the barrier are exponentially related to the negative of the barrier. If the barrier is high, the number of electrons that can be injected is exponentially reduced. If the barrier is lowered, the number of electrons that can be injected will exponentially increase.

psi S corresponds to Vgs

## Super-Threshold Characteristics: Linear Region (Vgs>Vth, small Vds)
- Q1: What is another name for the linear region?
- Q: What is the equation for inversion charge? How does this equation change when Vds is not 0?
- Q: Why is it called the linear region?

![image](https://github.com/user-attachments/assets/8b76e2d0-6b85-4f28-8a60-10dd47bff82b)

![image](https://github.com/user-attachments/assets/4493e4ef-d4ed-465d-a4b1-49c5a6cbdf76)

v is velocity

- A1: The triode region.

### Super-Threshold Characteristics: Saturation Region (Vgs > Vth, Large Vds)
- Q: What is pinch-off and why does it occur?
- Q: What happens when Vds > Vgs-Vth?
- Q1: Why is it called saturation region?
- Q: What is channel length modulation?

![image](https://github.com/user-attachments/assets/2964d35e-701a-412a-9167-ed290d4a4031)

![image](https://github.com/user-attachments/assets/552c5742-7b37-4746-b2fa-6bf4a994ecdb)

- A1: Current is no longer increasing, and is instead getting saturated.

## Mild Modification To Current In Linear Regime
- Q: How does current for linear region change when we go into saturation region?
  
![image](https://github.com/user-attachments/assets/8bd43ec4-949e-4b8d-bdb5-fc836de4d8dd)

Lambda is a proportionality constant

## Velocity Saturation: Short Channel Effect
![image](https://github.com/user-attachments/assets/a51b0592-095b-41bf-b770-d094223d7741)

## Derivation Revisited
- Q: During velocity saturation, what is the relationship between current and Vgs-Vth?
  
![image](https://github.com/user-attachments/assets/5cce6e48-97a2-40c2-a1c1-768ac9fd0faa)

For the purposes of this course, Vdsat is a constant parameter

## MOSFET Characteristics: Long Channel Regime
- Q: What does a current vs Vgs graph look like? Show where the Vth is.
- Q: What is the difference between transfer characteristics and output characteristics?
- Q1: What would quadratic dependence on Vgs mean?

![image](https://github.com/user-attachments/assets/8d32da0d-81b8-4b9a-b4d3-2a35f1081512)

- A1: It means we are seeing pinch off saturation.

## Transfer Characteristics: Id versus Vgs
- Q: What is the difference in graphs for long channnel vs short channel?
- Q1: Why does short channel have quadratic dependecy at lower values of Vgs?

![image](https://github.com/user-attachments/assets/223b69ca-2e02-4c74-a433-ba9ee3658c42)

- A1: Because pinch off is occuring before velocity saturation.

## MOSFET Characteristics
![image](https://github.com/user-attachments/assets/129c8661-3575-4bed-853d-dc12a3ea37a6)

## Output Characteristics: Id vs Vds
- Q: How would output characteristics differ for short channel vs long channel?

![image](https://github.com/user-attachments/assets/f0ec8d81-df3b-451a-af56-48d8765750f2)

## Transfer Characteristics (Log Scale): Id vs Vgs
- Q: How to calculate DIBL based on the information in the graph?

![image](https://github.com/user-attachments/assets/f5037a09-e7c8-4037-a2a9-5685b7c4e489)

## Key Points: General MOSFET Characteristics (NMOS)
![image](https://github.com/user-attachments/assets/a08a6e7b-38f7-4295-a930-64cfef96e9e9)

## Key Points: General MOSFET Characteristics (PMOS)
![image](https://github.com/user-attachments/assets/2ed912ce-e8c2-4f27-9c23-f37f435ecba6)

![image](https://github.com/user-attachments/assets/87efc0b2-7dad-433c-a944-c9f92b367f2e)

## Other Short Channel Effects
![image](https://github.com/user-attachments/assets/35f18727-9f32-4e9e-a973-994b1796f284)

## Alternate Devices
- Q: What are the 7 alternate devices called?
- Q1: Why did bulk MOSFETs stop being used past 20nm?

![image](https://github.com/user-attachments/assets/4ae03549-ed76-4205-a8d0-0f389a3dec94)

- A1: Drain in source are so close that drain's electric field has a lot of influence on the source through DIBL. Ideally we only want gate to be controlling the source.

## Double Gate MOSFET Structure
- Q: What is the issue with double gate MOSFET?

![image](https://github.com/user-attachments/assets/a745bd0a-3b12-4ebb-90a2-e826f198f776)

## FinFET
![image](https://github.com/user-attachments/assets/5999f8f7-0f45-4568-98bd-a49bec9da4be)

## FinFET Characteristics At A Glance
- Q: What are the voltage conditions under which we look at Ioff?

![image](https://github.com/user-attachments/assets/25a590cc-5bb5-4864-92e5-39705f60bd9f)

S.S - subthreshold swing

## Multi-fin FinFETs
![image](https://github.com/user-attachments/assets/09c720ad-3c22-4dc0-b901-53b3ec6d2587)

# Capacitance Components

## Dynamic Behavior of MOS Transistor
![image](https://github.com/user-attachments/assets/24871f6e-e8f6-4dd4-8108-cb42b7a8d087)

## Gate Capacitance (Cch)
- Q: What is the equation for each of channel, source overlap, drain overlap, and gate capacitances?
  
![image](https://github.com/user-attachments/assets/ef1f2760-088e-446c-af24-7f436cfac369)

ov = overlap

- Q: What conditions define each operation region?
- Q: What is the capacitance of Cgb, Cgs, Cgd at each operation region?

![image](https://github.com/user-attachments/assets/f6fe5571-8b07-4a46-8bae-a65801e81925)

## Diffusion Capacitance (Csb and Cdb)
![image](https://github.com/user-attachments/assets/767fe5d0-afcb-42ec-929d-bbc9aadb9f2b)

## FinFET: Capacitance
- Q: How is FinFETs capacitance compared to planar or bulk ones?
  
![image](https://github.com/user-attachments/assets/1dfa6ffe-17d2-45cc-bb8d-10dfe2bf0ab1)

Having a higher capacitance is not good.

## Source/Drain Resistance
- Q: How do the resistances affect the performance? Why?

![image](https://github.com/user-attachments/assets/18c7bcb0-71cb-457e-8731-0e050c5c0f5c)

- A: Resistance degrade performance by reducing the on current.

## Leakage Components
- Q: What are the 4 leakages discussed?
  
![image](https://github.com/user-attachments/assets/71562f5a-d2e7-4c39-a740-0deb650e96a7)

## CMOS Beyond FinFETs: Nanosheet FETs
![image](https://github.com/user-attachments/assets/6f3ac735-7edf-440f-8260-4b184a485894)

## CMOS Beyond FinFETs: Forksheet FETs
![image](https://github.com/user-attachments/assets/2865a3f0-183c-41be-a078-6c5029aedf88)
