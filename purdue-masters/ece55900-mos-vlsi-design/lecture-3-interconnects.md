# Interconnects

## Interconnects in VLSI Circuits
![image](https://github.com/user-attachments/assets/fdbbda6d-2ee7-485e-8bdf-ca0dec52a907)

## Backend Interconnects in a Chip
![image](https://github.com/user-attachments/assets/661e8bc3-3731-49b3-b6fc-18bc181f74aa)

BEOL - back end of layers

## Modern Interconnect
![image](https://github.com/user-attachments/assets/bc120ab3-ecfe-4d33-924d-e3d413027c60)

## Modern Wire Structure
![image](https://github.com/user-attachments/assets/7521312f-23a9-42d1-b7ef-ebfdacf1a7fc)

## Backend Layers
- Q: Why is a low-k dialectric used?

![image](https://github.com/user-attachments/assets/ee3c0a97-6d70-4abf-8c45-0b9a62612487)

- A: Because of low capacitance

## Interconnect Pitch
- Q: Why does the pitch increase when we go to higher layers?

![image](https://github.com/user-attachments/assets/b9edfe4a-e68d-4d10-89e4-67018ac5f595)

- A: The higher layers are reserved for more global signals like power. They are longer (more resistance), so to lower resistance they are made wider

## Interconnect Parasitics
- Q: What are classes of parasitics?
- Q: What are some of the effects due to them?
- Q: What are the wire models?
- Q1: What is the lumped C model good for?

![image](https://github.com/user-attachments/assets/bc454987-c9de-43a7-94e2-3f25942b087f)

- A1: It is good to model a wire that is short because it ignores capacitance

## Interconnect Capacitance
![image](https://github.com/user-attachments/assets/333a2d2c-9867-4509-874a-e60d19dc00a9)

## Capactitance: The Inter-Layer Parallel Plate Model
Big C is total capacitance. Small c is capacitance per unit length.

## Capacitance: Fringing Capacitance Model
- Q1: If you increase the aspect ratio, will the parallel plat or the fringing fields become more dominant?
  
![image](https://github.com/user-attachments/assets/2c8cb819-cdea-4d3d-8be2-ef7b834ce70b)

- A1: Fringing fields
  
## Fringing Versus Parallel Plate
![image](https://github.com/user-attachments/assets/fe196d3a-a3d4-4b14-97ec-a87eb48b3be3)

## The Side Parallel Plate Capacitance
![image](https://github.com/user-attachments/assets/afc5efd5-88b9-447e-91cd-496ea3e0c50b)

## Impact of Inter-wire Capacitance
![image](https://github.com/user-attachments/assets/e66fd208-a3a3-4375-868d-90a8f78179c7)

## Interconnect Resistance

## Wire Resistance
![image](https://github.com/user-attachments/assets/5fb0c45e-d049-453e-b190-ede6ec89796e)

## Interconnect Resistivity
![image](https://github.com/user-attachments/assets/cc298d3f-f4a5-47ca-8e1b-d0389e06d428)

## Interconnect Circuit Models

## The lumped capacitive model
- Draw this circuit as a lumped capacitive model:
  
![image](https://github.com/user-attachments/assets/ae07abc1-4bec-49fa-9e84-fefc3511a59e)

![image](https://github.com/user-attachments/assets/b7859669-aae5-46af-90d0-3a5f6888c28f)

## The Distriburted RC Models
- Draw the pi-1, pi-2, pi-3 model, t-1, t-2, t-3 model, and distributed RC chain for this wire:
  
![image](https://github.com/user-attachments/assets/f4dd991b-09ad-47ff-84f3-733f0a60dd70)

![image](https://github.com/user-attachments/assets/95644bdd-5f46-425d-b844-cb2e2de11559)

### The Distributed RC Model: pi-1 Model
![image](https://github.com/user-attachments/assets/fc227073-e12a-4533-8067-ef576b9bbe24)

### Elmore Delay Model
- Q: What is the time constant of the waveform appearing at node 3?

![image](https://github.com/user-attachments/assets/72703af7-fc4c-459c-a4dc-884fccdcc446)

![image](https://github.com/user-attachments/assets/81f7c60f-1759-4795-99f5-e612c248a187)

![image](https://github.com/user-attachments/assets/405e1512-9a1c-4c33-b831-fc45f80bf791)

![image](https://github.com/user-attachments/assets/a7957e5d-3212-4d2c-b79e-e3e5253fb577)

- This model is typically used for end nodes, like 3,4,5. We cannot really use it to approximate the time constant on nodes 1 and 2.

### The Elmore Delay RC Chain
- Q: What is the time constant of the rc chain?

![image](https://github.com/user-attachments/assets/399263f9-f702-40f2-8d91-4f3e82685d58)

![image](https://github.com/user-attachments/assets/79731df3-5a2a-4948-9593-6762648bc3c7)

- Q1: Where does the 2 in the denominator come from?
- A1: The RC components are distributed.

- Time constant is quadratically related to the length of the wire. If you double the length, it will quadriple.

### Step-Response of RC Wire
![image](https://github.com/user-attachments/assets/1b8743cf-d95d-4781-90b0-82291a9a4e43)

![image](https://github.com/user-attachments/assets/d095623a-719a-47ad-9821-3cd549903101)

### RC Models: Lumped vs Distributed
![image](https://github.com/user-attachments/assets/ccde7d8a-692b-4635-b5d0-80dd54544f14)

### Driving an RC-Line
- Q1: Why is the c_w * L divided by 2?

![image](https://github.com/user-attachments/assets/3e97d3fa-beaa-466e-a9f4-fce47e970196)

- A1: Because of the distributed effect.
