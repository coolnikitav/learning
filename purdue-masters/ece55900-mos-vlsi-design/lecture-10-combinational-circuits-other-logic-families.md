# Combinational Circuits: Other Logic Families

## Ratioed Logic
- Q: What are some downsides of ratioed logic?

![image](https://github.com/user-attachments/assets/711ae929-d052-4eba-93be-75a6b00b4311)

## Dynamic Cascode Voltage Switch Logic (DCVSL)
![image](https://github.com/user-attachments/assets/f27520a6-5b6f-4e96-be82-1af16fe6dd1c)

## DCVSL Example: NAND/AND
![image](https://github.com/user-attachments/assets/ab7caef2-8cf0-49a3-9920-97003977c15a)

## DCVSL Example: XOR/XNOR
![image](https://github.com/user-attachments/assets/a49d2d07-6561-43bd-afd5-0263331a2626)

## DCVSL Example: Improved XOR/XNOR
![image](https://github.com/user-attachments/assets/9dce016a-969e-433d-b582-8e2ddf5a5aee)

## Pass Transistor Logic
![image](https://github.com/user-attachments/assets/cf2c29d0-ccbe-4a5a-9569-834f2195b2eb)

## Pass Transistor Logic (PTL): AND Gate
![image](https://github.com/user-attachments/assets/6d157098-f5a6-42d9-8aec-26607c68ed44)

Note: You get a weak swing at X, but a full swing at F.

## PTL: Static Current In The Inverter
![image](https://github.com/user-attachments/assets/f9641fbf-4c34-4e8d-924a-936438b5fbdd)

## PTL: Level Restorer
![image](https://github.com/user-attachments/assets/d0f07709-7af4-4b56-a8e0-d1533f70696a)

![image](https://github.com/user-attachments/assets/65433065-24c0-4a47-8490-405405fae128)

Note: make sure the level restorer does not beat N2. So make restorer weak or N2 strong

## PTL: Level Restorer Sizing
-Q: What happens if restorer width is too large or too small?

![image](https://github.com/user-attachments/assets/287e8c25-d5cb-45b3-be16-881b7c8651a6)

## PTL: Zero Threshold Voltage Pass Transistors
![image](https://github.com/user-attachments/assets/3057231e-b5b4-4dec-bc17-cabefdd00078)

## Complementary Pass Transistor Logic
![image](https://github.com/user-attachments/assets/ff6b0559-c6b1-4eb7-9b06-f7fadc2df72b)

![image](https://github.com/user-attachments/assets/3d2c0a25-4552-4041-a877-c82b49f400ce)

![image](https://github.com/user-attachments/assets/7224a987-a9ca-40f1-bea2-045e01aaa473)

## Transmission Gate Logic
- Q: How does transission gate logic work?

![image](https://github.com/user-attachments/assets/4878a076-e54d-4618-acf0-164c367da17a)

## Transmission Gates: AND
![image](https://github.com/user-attachments/assets/3727e0cd-5fc6-44bb-aa58-189fd118814f)

## XOR Gate Using Transmission Gates
![image](https://github.com/user-attachments/assets/f1c9ba7a-3a69-49b4-9611-668207653deb)

## Full Adder Transmission Gates
![image](https://github.com/user-attachments/assets/e2d777d3-ddb6-4c98-b9a3-c1ebee489e40)

## Delay In Transmission Gates
![image](https://github.com/user-attachments/assets/19432579-29d0-45c4-b931-abf1c5906222)

![image](https://github.com/user-attachments/assets/05342751-2468-4eef-a09f-849146dc338d)

## Dynamic Logic
![image](https://github.com/user-attachments/assets/2ee1838e-9ce7-4db7-8eb7-da301b39f1d7)

## Static Vs Dynamic Circuits
![image](https://github.com/user-attachments/assets/6d000851-527a-4b2d-8990-dfc8741a8190)

## Dynamic Logic
![image](https://github.com/user-attachments/assets/f96b4aca-5105-484e-9161-86d84599bc88)

## Dynamic Logic: Properties
![image](https://github.com/user-attachments/assets/25e9bd79-eded-40c4-9274-ae6350f47aac)

Note: extremely small short circuit current because clock rise/fall times are very sharp and small

![image](https://github.com/user-attachments/assets/38573eab-9f62-4fa9-b8bd-71116695b557)

Note: dynamic always starts at Vdd, so alpha 2 is higher

## Design Issue 1: Charge Leakage
![image](https://github.com/user-attachments/assets/97bc3105-2ee7-4f2e-930d-a7e484b76842)

Note: leakage is small, so this only has affect when time period is very large

## Solution To Charge Leakage: Keeper
![image](https://github.com/user-attachments/assets/783a3537-22d0-4e8b-9e5b-f9b335c0461f)

## Designing Dynamic Gates With Keepers
![image](https://github.com/user-attachments/assets/28a32fee-f56a-4f8f-a4f4-c7835f3842be)

Note: your PDN needs to be strong or keeper needs to be weak, so Vout is below the trip point of the inverter

## Design Issue 2: Charge Sharing 
![image](https://github.com/user-attachments/assets/fd6b9a36-32bb-4ba9-b89b-a6a80283be64)

![image](https://github.com/user-attachments/assets/acde4edb-a16e-4a26-81b9-47f6b3cdb0cb)

## Solutions To Charge Sharing
![image](https://github.com/user-attachments/assets/eda606ff-8de5-4cc8-9f54-a11c868f6186)

## Design Issue 3: Clock Feedthrough
![image](https://github.com/user-attachments/assets/f7f60ea9-fc06-4599-a76c-446725b9e312)

## Design Issue 4: Backgate Coupling
![image](https://github.com/user-attachments/assets/d4f0388f-65d1-4989-ad4b-ef96966a7ea6)

![image](https://github.com/user-attachments/assets/01c814d3-44f8-4d3b-8af2-ea26528d54a7)

## Design Issue 5: Cascading Dynamic Gates
![image](https://github.com/user-attachments/assets/8181a802-9e5f-4820-a835-02b0ea206434)

Note: since you cannot have 1 to 0 transitions at the second input, you cannot cascade gates like this

## Solution: Domino Logic
![image](https://github.com/user-attachments/assets/3329b817-260d-442e-9626-0a39955cafdf)

## Domino Logic: Properties
![image](https://github.com/user-attachments/assets/dad5160b-f38b-4b53-9190-d749a2519a3a)

Note: you want the PMOS to be strong in the skewed inverter because it needs to pull up

## NP-CMOS
![image](https://github.com/user-attachments/assets/d2c8adf1-0449-446b-8de0-ec7196936bef)

![image](https://github.com/user-attachments/assets/ad556e45-3f37-4c09-b103-3923deb2c887)
