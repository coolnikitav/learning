## Set 6
10. What are teh different looping constructs in Verilog?
    - for, while, foreach?
9. Implement XOR gate using 2:1 MUX
    ```
    module XOR(
       input a,b,
       output c
    );
       assign c = (a != b);
    endmodule
    ```
8. Difference between mealy and moore FSM.
   - Mealy output depends on the input only. Moore output depends on input and destination.
7. What are some ways a race condition can get created and how can these race conditions be avoided?
   - Race conditions can be created in combinational blocks if some of the inputs and outputs are the same. Example:
     ```
     always @ (a)
         a = ~a;
     ```
     They can be avoided by making sure inputs and outputs are not the same.
6. Give examples of Verilog code that synthesizes into a latch and flipflop
   - FF:
     ```
        always @ (posedge clk)
           out <= in;
     ```
   - Latch:
     ```
        always @ (*)
           out = in;
     ```
5. Give some guidelines on how combinational and sequential logic should be coded in Verilog.
   - Sequential logic should be coded with always @ (posedge or negedge clk) because it depends on the clock.
   - Combinational logic can be coded in a couple of ways:
     - always @ (*) triggers on any input change inside of the always statement
     - assign connects inputs to outputs through combinational logic
4. What does RTL mean?
   - Register transfer level. 
3. What software is used to simulate Verilog code?
   - I used Vivado. I've also used ModelSim. I suppose any Verilog compiler/simulator.
2. What is Verilog used for?
   - Verilog is used to model digital hardware by writing code.
1. What are the main data types in Verilog?
   - Wire, reg

## Set 5
10. Illustrate the side effect of leaving an input port unconnected that influences a logic to an output port.
    - The unconnected ports will have a value of Z. Synthesis tools will optimize it away.
9. Illustrate the side effect of not connecting all the ports during instantiation.
    - The unconnected ports will be dangling ports and take on a value of high-impedance Z. == statements would evaluate to false. It could lead to excessive power consumption and timing issues.
8. What is a parameter in Verilog?
   - It behaves like a constant.
7. What is the purpose of DPI? Give an example.
   - DPI - direct programming interface. It allows hardware to communicate to external software applications.
6. Explain what happens when width of state registers is not increased as more states gets added in a state machine.
   - If truncates the added states to the width of the register, making it overlap with another state.
5. Illustrate the side effect of an implicit 1 bit wire declaration of a multi-bit port during instantiation
   - The other wires of the port are in an unknown state.
4. Same variable used in two loops running simultaneously
   - Race conditions
   - Deadlock
3. Illustrate the side effects of mulitple processes writing to the same variable
   - Race conditions
   - Multiple processes trying to access the same variable
   - A process taking a couple steps to modify the variable and another process trying to access it during that time
   - Deadlock
2 - Illustrate the side effect of specifying delays in assing statements.
    - Delays are not synthesizable. If the functionality depends on them, there will be a mismatch between the simulation and the model.
1 - Illustrate a few important considerations in Verilog simulation regressions.
    - Test coverage
    - Scalability
    - Debugging
    - Simulation accuracy

## Set 4
10. What is PLI?
    - Programming Language Interface. It allows software developers to access and control simulation data within a simulation environment.
9. What are some of the features in VHDL?
   - 9 values of wires.
8. What does timescale 1ns/1ps mean?
   - It means that each step is 1 ns with precision in the picoseconds.
7. Explain stages in the setup of a regression environment for simulations?
   - Not sure what this is.
6. Explain force and release commands in verilog.
   - Force and release can be applied to wires, nets, variable. Force will override any action performed on the variable until it is released.
5. What are good practices of writing FSM code?
   - Define states, know what causes them to transition, draw out the FSM diagram, understand how outputs are determined.
4. What is meant by inertial delay?
   - Delay introduced by gates and wires.
3. What does transport delay mean in verilog?
   - The time it takes for a signal to propagate through a real circuit.
2. Explain $monitor, $display, $strobe.
   - $display prints to the console. Not sure what $monitor and $strobe do.
1. What is a sensitivity list?
   - A sensitivity list is a list of all signal that would trigger a procedural or combinatorial block to execute.

## Set 3
10 - Give major differences between a task and a function.

Task is used to perform a series of actions. Function is used to process an input and get an output.

The main difference is that a task can contain statements that consumer simulation time, like delays.

9 - What are blocking and non-blocking statements?

Blocking statements execute in sequence and use "="

Non-blocking statements execute in parallel and use "<="

8 - What is a reg in Verilog?

Reg is a variable that stores a value.

7 - What does a wire refer to?

Wire refers to a physical wire constantly connecting 2 nodes in a design.

6 - What are verilog parallel case and full case statements?

parallel case - a case statement where not every case it covered. In situations where case isn't covered, default behavior (latch) will be assumed.

full case - every possibility of a case is covered.

5 - Give a few examples of compiler directives.

`define - macros

`include - to include all of the content from another file into this one

`timescale - defines the timescale of the simulation

`ifdef ... endif - conditional statements

4 - What is meant by logic synthesis?

Logic synthesis is the process of transforming an electronic circuit design description into a gate-level netlist.

3 - How is rise, fall and turnoff delays represented in Verilog?

It can be specified as one delay (equal delay for all 3), two delay (rise, fall), or three delay (rise, fall, turnoff).

One delay: #(5) | Two delay: #(5,6) | Three delay: #(6,7,8)

They can also be specified using specify block:
```
specify
  specparam delay = 5;
  delay (a, out) = (specify_values => (delay, 0));
endspecify
```

2 - What is $time in Verilog?

$time variable gives the current time of the simulation. By current time, meaning how long the simulation has been running. It is measured in time units defined by timescale.

1 - What is a defparam used for?

Defparam is used to overwrite parameters set in a module.

## Set 2
10 - What are parallel threads in Verilog?

Multiple blocks within a module that execute concurrently. Parallel threads are also created in fork...join.

9 - What are different types of delay control?

#delay, @posedge, @negedge, wait

I don't really know other delay controls.

8 - What is verilog $random?

$random generates a random 32-bit signed integer in verilog.

7 - What is duty cycle?

Duty cycle is the percentage of time a digital signal stays on during 1 clock cycle.

6 - What do you understand by casex and casez statements in Verilog?

casex and casez are case statements or structures. In regular case the operand needs to match completely. In casex and casez, the digits of the operand that do not need to match completely are replaced by x/z/? and ?/Z, respectively. For example: if case is 4'b01x0, the 2nd MSB doesnt need to match.

5 - How can you generate a sine wave using the verilog coding style?

By using $sin(x) function.

4 - What is the difference between $setup and $hold?

They are constraints in the simulation. Setup time is the time signal has to be stable before it is read and hold time is time that signal has to be stable after it is read.

3 - Can 'define be used for text substitution through variable instead of literal substitution?

No. It can only be used for literal substitution with a value.

2 - What do you understand by continuous assignment?

I imagine a wire connected between 2 signals. The value of the output constantly reflects the value of the input.

1 - What are HDL simulators?

Hardware description language simulators compile our HDL code into a physical RTL layout and allow us to run a simulation to verify it.

## Set 1
1 - Write Verilog code to swap contents of two registers with and without a temporary register

```
// With temp
reg a,b,temp;

always @ (*) begin
  temp = a;
  a = b;
  b = temp;
end

// Without temp
reg a,b;

always @ (*) begin
  a <= b;
  b <= a;
end
```

2 - Elaborate on the file operation support in Verilog
I haven't specifically studied file operation in Verilog. However, I would assume you can read file, write to them.

3 - Difference between inter statement and intra statement delay?
Inter statement delay is the delay between statemenets. Intra statement delay is delay within a statement between its operations.

4 - What is delta simulation time?
It is used to model zero delay. It is the smallest delay that can be specified. It represents one step in the simulation. Usually used for combinatorial logic.

5 - What is meant by inferring latches and how can you avoid it?
A latch is inferred when not all logical posibilies are covered. You can avoid it by adding a default case that covers everything not explicitely stated.

6 - Which will be updated first - variables or signals?
Signals are updated first. They are wires and registers. Then variables are updated based on values of signals.

7 - Why is it necessary to list all inputs in the sensitivity ist of a combinational circuit?
So the output changes wheneve any of the inputs change.

8 - What are the main differences between VHDL and verilog?
VHDL is not case sensitive. It has 2 parts: architecture and entity. Verilog is made of modules. VHDL signals have 9 values, the same signals in Verilog have 4.

9 - Give some examples of commonly used Verilog system tasks and their purposes.
$display - print to the console during simulation

$random - generate a random 32 bit int

$finish - simulation termination

$fopen, $fclose - open and close a file

10 - Write a Verilog code for synchronous and asynchronous reset.

Synch:
```
always @ (posedge clk)
  if (reset)
    do reset;
```

Asynch:
```
always @ (posedge clk or posedge reset)
  if (reset)
    do reset;
```
