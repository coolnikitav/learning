1.8 The Pentium 4 Prescott processor, released in 2004, had a clock rate of
3.6GHz and voltage of 1.25V. Assume that, on average, it consumed 10W of static
power and 90W of dynamic power.
The Core i5 Ivy Bridge, released in 2012, has a clock rate of 3.4GHz and voltage
of 0.9V. Assume that, on average, it consumed 30W of static power and 40W of
dynamic power.

1.8.1 For each processor find the average capacitive loads.
- DP = 1/2 * Capacitive load * voltage^2 * frequency
- C(P4) = 90/(0.5 * 1.25^2 * 3.6 * 10^9) = 90/(2.8125 * 10^9) = 32 * 10^-9 F
- C(i5) = 40/(0.5 * 0.9^2 * 3.4 * 10^9) = 40/(1.377 * 10^9) = 29.05 * 10^-9 F

1.8.2 Find the percentage of the total dissipated power comprised by
static power and the ratio of static power to dynamic power for each technology.
- SP(P4) = 10%, SP/DP = 1/9
- SP(i5) = 30/70, SP/DP = 30/40

1.8.3 If the total dissipated power is to be reduced by 10%, how much
should the voltage be reduced to maintain the same leakage current? Note: power
is defined as the product of voltage and current.
- DP = 0/5 * C * V^2 * F, SP = V*I
- (SPnew + DPnew)/(SPold + DPold) = 0.9
- DPnew = 0.5 * C * Vnew^2 * F = 0.9 * (SPold + DPold) - SPnew
- Vnew = (2 * DPnew / (C * F))^1/2 = (2 * (0.9 * (SPold + DPold) - SPnew) / (C * F))^1/2
- SPnew = Vnew * I
- SPold = Vold * I
- SPnew = Vnew * SPold / Vold
- Vnew = (2 * (0.9 * (SPold + DPold) - Vnew * SPold / Vold) / (C * F))^1/2



1.7 Compilers can have a profound impact on the performance
of an application. Assume that for a program, compiler A results in a dynamic
instruction count of 1.0E9 and has an execution time of 1.1 s, while compiler B
results in a dynamic instruction count of 1.2E9 and an execution time of 1.5 s.
a. Find the average CPI for each program given that the processor has a clock cycle
time of 1ns.
b. Assume the compiled programs run on two different processors. If the execution
times on the two processors are the same, how much faster is the clock of the
processor running compiler A’s code versus the clock of the processor running
compiler B’s code?
c. A new compiler is developed that uses only 6.0E8 instructions and has an
average CPI of 1.1. What is the speedup of using this new compiler versus using
compiler A or B on the original processor?
- a. time/program = inst/program * clock cycles/instr * sec/clock cycle
- CPIA = 1.1/(10^9 * 10^-9) = 1.1 cc/instr
- CPIB = 1.5/(1.2*10^9 * 10^-9) = 1.25 cc/instr
- b. clock cycles/sec = instr/program * clock cycles/instr / time/program
- clock cycles/sec of A = 10^9 * 1.1 / x
- clock cycles/sec of B = 1.2 * 10^9 * 1.25 / x
- f(b)/f(a) = 1.2*1.25/1.1 = 1.37
- c. time/program = 6 * 10^8 * 1.1 * 10^-9 = 0.66s
- speedupA = 1.1/0.66 = 1.67
- speedupB = 1.5/0.66 = 2.27




1.6 Consider two different implementations of the same instruction
set architecture. The instructions can be divided into four classes according to
their CPI (classes A, B, C, and D). P1 with a clock rate of 2.5GHz and CPIs of 1, 2,
3, and 3, and P2 with a clock rate of 3GHz and CPIs of 2, 2, 2, and 2.
Given a program with a dynamic instruction count of 1.0E6 instructions divided
into classes as follows: 10% class A, 20% class B, 50% class C, and 20% class D,
which is faster: P1 or P2?
a. What is the global CPI for each implementation?
b. Find the clock cycles required in both cases.
- a: P1: 0.1 * 1 + 0.2 * 2 + 0.5 * 3 + 0.2 * 3 = 2.6 CPI
- P2: 2 CPI
- b. clock cycles P1 = 1 * 10^6 * 2.6 = 2.6 * 10^6 cc
- clock cycles P2 = 2 * 10^6 cc




1.5 Consider three different processors P1, P2, and P3 executing
the same instruction set. P1 has a 3GHz clock rate and a CPI of 1.5. P2 has a
2.5GHz clock rate and a CPI of 1.0. P3 has a 4.0GHz clock rate and has a CPI
of 2.2.
a. Which processor has the highest performance expressed in instructions per second?
b. If the processors each execute a program in 10 seconds, find the number of
cycles and the number of instructions.
c. We are trying to reduce the execution time by 30%, but this leads to an increase
of 20% in the CPI. What clock rate should we have to get this time reduction?
- a. instr/sec = instr/clock cycles * clock cycles/sec
- instr/sec(P1) = 1/1.5 * 3 = 2G-instr/sec
- instr/sec(P2) = 1 * 2.5 = 2.5 G-instr/sec
- instr/sec(P3) = 1/2.2 * 4 = 1.82 G-insr/sec
- b. instr(P1) = 2 * 10 = 20G-instr, cycles(P1) = 3 * 10 = 30 G cycles
- instr(P2) = 2.5 * 10 = 25 G-instr, cycles(P2) = 2.5 * 10 = 25 G cycles
- instr(P3) = 1.82 * 10 = 18.2 G-instr, cycles(P3) = 4 * 10 = 40 G cycles
- c. time/program = instructions/program * clock cycles/instruction * sec/clock cycles
- P1: 7 = 20G-instr * 1.2*1.5 * 1/f => f = 20G-instr * 1.2 * 1.5 / 7 = 5.14GHz
- P2: 25G-instr * 1.2 * 1 / 7 = 4.28 GHz
- P3: 18.2 G-instr * 1.2 * 2.2 / 7 = 6.75 GHz




1.4 Assume a color display using 8 bits for each of the primary colors
(red, green, blue) per pixel and a frame size of 1280 × 1024.
a. What is the minimum size in bytes of the frame buffer to store a frame?
b. How long would it take, at a minimum, for the frame to be sent over a 100Mbit/s
network?
- a. Buffer size = 3 * 1280 * 1024 B = 3932160B
- b. Time = 3.932160 MB / 100Mbit/s = 8*3.932160 Mbit / 100Mbit/s = 310 msec




1.3 Describe the steps that transform a program written in a high-level
language such as C into a representation that is directly executed by a computer
processor.
- C program -> assembly via compiler -> machine lanugage via assembler




1.2 The eight great ideas in computer architecture are similar to ideas
from other fields. Match the eight ideas from computer architecture, “Design for
Moore’s Law,” “Use Abstraction to Simplify Design,” “Make the Common Case
Fast,” “Performance via Parallelism,” “Performance via Pipelining,” “Performance
via Prediction,” “Hierarchy of Memories,” and “Dependability via Redundancy” to
the following ideas from other fields:
a. Assembly lines in automobile manufacturing
b. Suspension bridge cables
c. Aircraft and marine navigation systems that incorporate wind information
d. Express elevators in buildings
e. Library reserve desk
f. Increasing the gate area on a CMOS transistor to decrease its switching time
g. Adding electromagnetic aircraft catapults (which are electrically powered
as opposed to current steam-powered models), allowed by the increased power
generation offered by the new reactor technology
h. Building self-driving cars whose control systems partially rely on existing sensor
systems already installed into the base vehicle, such as lane departure systems and
smart cruise control systems

- a: Performance via Pipelining
- b: Dependability via Redundancy
- c: Performance via Prediction
- d: Make the Common Case Fast
- e: Hierarchy of Memories
- f: Performance via Parallelism
- g: Design for Moore’s Law
- h: Use Abstraction to Simplify Design




1.1 Aside from the smart cell phones used by a billion people, list and
describe four other types of computers.
- Personal computer
- Server
- Warehouse scale computer
- Supercomputer
- Embedded computer




1.10 ![image](https://github.com/user-attachments/assets/15964a5a-5d3f-491c-9c5d-481f817d688c)
- MIPS rating = instr/clock cycles * clock cycles/sec
- MIPSA = 1 * 4GHz = 4G-instr/sec
- MIPSB = 1/1.1 * 4GHz = 3.64G-instr/sec
- TimeA = 0.01G-instr / 4G-instr/sec = 0.0025 sec = 2.5 msec
- TimeB = 0.008G-instr / 3.64G-instr/sec = 2.2 msec
- B is faster




1.7 Suppose we developed a new, simpler processor that has 85% of the capacitive
load of the more complex older processor. Further, assume that it can adjust
voltage so that it can reduce voltage 15% compared to processor B, which
results in a 15% shrink in frequency. What is the impact on dynamic power?
- Power >< Capacitive load * voltage^2 * frequency
- Pnew = Pold * 0.85 * 0.85^2 * 0.85 = Pold * 0.52




1.6 A given application written in Java runs 15 seconds on a desktop processor. A new
Java compiler is released that requires only 0.6 as many instructions as the old
compiler. Unfortunately, it increases the CPI by 1.1. How fast can we expect the
application to run using this new compiler? Pick the right answer from the three
choices below:
- Time/program = instr/program * clock cycles/instr * sec/clock cycle
- New time/program = old time * 0.6 * 1.1 = old time * 0.66 = 15*0.66 = 9.9sec



1.6
![image](https://github.com/user-attachments/assets/bfc3dcc6-e107-4f2b-9b6a-4ad5302738e5)
- Sequence 2 executes the most instructions (6)
- Clock cycles 1 = 2 * 1 + 1 * 2 + 2 * 3 = 10
- Clock cycles 2 = 4 * 1 + 1 * 2 + 1 * 3 = 9
- Sequence 2 will be faster
- CPI1 = 10/5 = 2
- CPI2 = 9/6 = 1.5



1.6 Suppose we have two implementations of the same instruction set architecture.
Computer A has a clock cycle time of 250ps and a CPI of 2.0 for some program,
and computer B has a clock cycle time of 500ps and a CPI of 1.2 for the same
program. Which computer is faster for this program and by how much?
- 500ps/instruction for A, 600ps/instruction for B => A is faster





1.6 Our favorite program runs in 10 seconds on computer A, which has a 2GHz
clock. We are trying to help a computer designer build a computer, B, which will
run this program in 6 seconds. The designer has determined that a substantial
increase in the clock rate is possible, but this increase will affect the rest of the
CPU design, causing computer B to require 1.2 times as many clock cycles as
computer A for this program. What clock rate should we tell the designer to
target?
- Program takes 20G-cycles on A. It will take 24G-cycles on B.
- clock rate = 24G-cycles/6sec = 4GHz




1.6 If computer A runs a program in 10 seconds and computer B runs the same
program in 15 seconds, how much faster is A than B?
- SpeedA = 1/10, SpeedB = 1/15
- SpeeadA/SpeedB = 1.5




1.6 Do the following changes to a computer system increase throughput, decrease
response time, or both?
  1. Replacing the processor in a computer with a faster version
  2. Adding additional processors to a system that uses multiple processors
for separate tasks—for example, searching the web

- 1 would help decrease response time and increase throughput
- 2 would only increase throughput




1.5 A key factor in determining the cost of an integrated circuit is volume. Which of
the following are reasons why a chip made in high volume should cost less?

- With high volumes, the manufacturing process can be tuned to a particular
design, increasing the yield.
- The masks used to make the chip are expensive, so the cost per chip is lower
for higher volumes.
- Engineering development costs are high and largely independent of volume;
thus, the development cost per die is lower with high-volume parts.


1.4: Semiconductor DRAM memory, flash memory, and disk storage differ
significantly. For each technology, list its volatility, approximate relative
access time, and approximate relative cost compared to DRAM.

```
                  DRAM             flash memory      disk
volatility       volatile        non-volatile     non-volatile           
access time      fast                medium        slow
cost              DRAM            cheaper than DRAM   cheaper than flash
```
