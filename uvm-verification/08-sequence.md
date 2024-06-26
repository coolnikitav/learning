# Sequence

## Fundamentals
![image](https://github.com/coolnikitav/coding-lessons/assets/30304422/fb36105c-df21-4fc2-b464-e5b3f544bbbe)

## Creating Sequences
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    function new(input string path = "sequence1");
        super.new(path);
    endfunction
    
    virtual task pre_body();
        `uvm_info("SEQ1", "PRE-BODY EXECUTED", UVM_NONE);
    endtask
    
    virtual task body();
        `uvm_info("SEQ1", "BODY EXECUTED", UVM_NONE);
    endtask
        
    virtual task post_body();
        `uvm_info("SEQ1", "POST-BODY EXECUTED", UVM_NONE);
    endtask        
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    
    function new(input string path = "DRV", uvm_component parent = null);
        super.new(path, parent);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("t");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        forever begin
            seq_item_port.get_next_item(t);
            seq_item_port.item_done();  // completed transfer to DUT
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seqr;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("d", this);
        seqr = uvm_sequencer#(transaction)::type_id::create("seqr", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seqr.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction
    
    agent a;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("a", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
    
    sequence1 seq1;
    env e;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("e", this);
        seq1 = sequence1::type_id::create("seq1");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        seq1.start(e.a.seqr);
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(34) @ 0: uvm_test_top.e.a.seqr@@seq1 [SEQ1] PRE-BODY EXECUTED
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(38) @ 0: uvm_test_top.e.a.seqr@@seq1 [SEQ1] BODY EXECUTED
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(42) @ 0: uvm_test_top.e.a.seqr@@seq1 [SEQ1] POST-BODY EXECUTED
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

## Understanding Flow
- Q: Explain the flow between sequence, sequencer, and driver.
  
![image](https://github.com/coolnikitav/coding-lessons/assets/30304422/641a3bf8-f8e6-4cb3-89b1-f2b3ebdca090)

```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        `uvm_info("SEQ1", "Trans obj created", UVM_NONE);
        trans = transaction::type_id::create("trans");
        
        `uvm_info("SEQ1", "Waiting for Grant from Driver", UVM_NONE);
        wait_for_grant();
        
        `uvm_info("SEQ1", "Rcvd Grant. Randomizing Data", UVM_NONE);
        assert(trans.randomize());
        
        `uvm_info("SEQ1", "Randomization Done -> Sent Req to Drv", UVM_NONE);
        send_request(trans);
        
        `uvm_info("SEQ1", "Waiting for Item Done Resp from Driver", UVM_NONE);
        wait_for_item_done();
        
        `uvm_info("SEQ1", "SEQ1 Ended", UVM_NONE);
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    virtual adder_if aif;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("TRANS");
        if(!uvm_config_db#(virtual adder_if)::get(this, "", "aif", aif))
            `uvm_info("DRV", "Unable to access Interface", UVM_NONE);
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        forever begin
            `uvm_info("Drv", "Sending Grant for Sequence", UVM_NONE);
            seq_item_port.get_next_item(t);
            `uvm_info("Drv", "Applying Seq to DUT", UVM_NONE);
            `uvm_info("Drv", "Sending Item Done Resp for Sequence", UVM_NONE);
            seq_item_port.item_done();  // completed transfer to DUT
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seqr;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("d", this);
        seqr = uvm_sequencer#(transaction)::type_id::create("seqr", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seqr.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction
    
    agent a;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("a", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
    
    sequence1 seq1;
    env e;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("e", this);
        seq1 = sequence1::type_id::create("seq1");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        seq1.start(e.a.seqr);
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;
    adder_if aif();
    
    initial begin
        uvm_config_db#(virtual adder_if)::set(null, "*", "aif", aif);
        run_test("test");
    end
endmodule

[Drv] Sending Grant for Sequence
[SEQ1] Trans obj Created
[SEQ1] Waiting for Grant from Driver
[SEQ1] Rcvd Grant. Randomizing Data
[SEQ1] Randomization Done -> Sent Req to Drv
[SEQ1] Waiting for Item Done Resp from Driver
[Drv] Applying Seq to DUT
[Drv] Sending Item Done Resp for Sequence
[Drv] Sending Grant for Sequence
[SEQ1] SEQ1 Ended
```

## Sending Data to Sequencer
- Q: Why do we use the Virtual keyword?
- Q: What does `uvm_do(item) do?
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        repeat(5) begin
            // takes a sequence item object, creates an object, 
            // randomizes it, then sends it to a driver through 
            // associated sequencer
            `uvm_do(trans);  
            trans.print(uvm_default_line_printer);
        end
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    transaction trans;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction

    virtual task run_phase(uvm_phase phase);
        trans = transaction::type_id::create("trans");
        forever begin
            seq_item_port.get_next_item(trans);
            trans.print(uvm_default_line_printer);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seqr;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("d", this);
        seqr = uvm_sequencer#(transaction)::type_id::create("seqr", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seqr.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
    sequence1 s1;
    
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("a", this);
        s1 = sequence1::type_id::create("s1");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        s1.start(a.seqr);
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("e", this);
    endfunction
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

trans: (transaction@563) { a: 'hf  b: 'h3  y: 'h0  begin_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@563) { a: 'hf  b: 'h3  y: 'h0  begin_time: 0  end_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@571) { a: 'h9  b: 'hb  y: 'h0  begin_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@571) { a: 'h9  b: 'hb  y: 'h0  begin_time: 0  end_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@575) { a: 'he  b: 'hb  y: 'h0  begin_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@575) { a: 'he  b: 'hb  y: 'h0  begin_time: 0  end_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@579) { a: 'hf  b: 'hc  y: 'h0  begin_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@579) { a: 'hf  b: 'hc  y: 'h0  begin_time: 0  end_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@585) { a: 'hf  b: 'h7  y: 'h0  begin_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  } 
trans: (transaction@585) { a: 'hf  b: 'h7  y: 'h0  begin_time: 0  end_time: 0  depth: 'd2  parent sequence (name): s1  parent sequence (full name): uvm_test_top.e.a.seqr.s1  sequencer: uvm_test_top.e.a.seqr  }
```
- A: We use virtual keyword whenever we override and define implementation for any inbuilt method

## Sending Data to Driver Method 2
```
1)
`uvm_do(trans)

- Pros: easy to use.
- Cons: user doesn't have any control of the generated data.

2)
create
start_item(trans)
assert(trans.randomize)
finish_item(trans)

- More flexible than method 1. Not as much work as method 3.

3)
create_item
wait_for_grant
assert(trans.randomize)
send_request
wait_for_item_done

- Pros: Can modify data
- Cons: too much work
```

```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        repeat(5) begin
            trans = transaction::type_id::create("trans");
            start_item(trans);
            assert(trans.randomize);
            finish_item(trans);
            `uvm_info("SEQ", $sformatf("a: %0d, b: %0d", trans.a, trans.b), UVM_NONE);
        end
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    transaction trans;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction

    virtual task run_phase(uvm_phase phase);
        trans = transaction::type_id::create("trans");
        forever begin
            seq_item_port.get_next_item(trans);
            `uvm_info("DRV", $sformatf("a: %0d, b: %0d", trans.a, trans.b), UVM_NONE);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seqr;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("d", this);
        seqr = uvm_sequencer#(transaction)::type_id::create("seqr", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seqr.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
    sequence1 s1;
    
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("a", this);
        s1 = sequence1::type_id::create("s1");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        s1.start(a.seqr);
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("e", this);
    endfunction
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(60) @ 0: uvm_test_top.e.a.d [DRV] a: 11, b: 2
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.e.a.seqr@@s1 [SEQ] a: 11, b: 2
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(60) @ 0: uvm_test_top.e.a.d [DRV] a: 11, b: 8
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.e.a.seqr@@s1 [SEQ] a: 11, b: 8
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(60) @ 0: uvm_test_top.e.a.d [DRV] a: 15, b: 2
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.e.a.seqr@@s1 [SEQ] a: 15, b: 2
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(60) @ 0: uvm_test_top.e.a.d [DRV] a: 8, b: 8
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.e.a.seqr@@s1 [SEQ] a: 8, b: 8
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(60) @ 0: uvm_test_top.e.a.d [DRV] a: 10, b: 10
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.e.a.seqr@@s1 [SEQ] a: 10, b: 10
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

## Multiple Sequences in Parallel
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        trans = transaction::type_id::create("trans");
        `uvm_info("SEQ1", "SEQ1 Started", UVM_NONE);
        start_item(trans);
        assert(trans.randomize);
        finish_item(trans);
        `uvm_info("SEQ1", "SEQ1 Ended", UVM_NONE);
    endtask         
endclass

///////////////////////////////////////////////////////

class sequence2 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence2)
    
    transaction trans;
    
    function new(input string inst = "seq2");
        super.new(inst);
    endfunction
    
    virtual task body();
        trans = transaction::type_id::create("trans");
        `uvm_info("SEQ2", "SEQ2 Started", UVM_NONE);
        start_item(trans);
        assert(trans.randomize);
        finish_item(trans);
        `uvm_info("SEQ2", "SEQ2 Ended", UVM_NONE);
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("TRANS");
    endfunction

    virtual task run_phase(uvm_phase phase);
        forever begin
            seq_item_port.get_next_item(t);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seq;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("DRV", this);
        seq = uvm_sequencer#(transaction)::type_id::create("seq", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seq.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
        
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("AGENT", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    sequence1 s1;
    sequence2 s2;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("ENV", this);
        s1 = sequence1::type_id::create("s1");
        s2 = sequence2::type_id::create("s2");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        fork
            s1.start(e.a.seq);
            s2.start(e.a.seq);
        join
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

## Changing Arbitration Mechanism
```
SEQ_ARB_FIFO (DEFAULT): first in first out ... priority won't work -- priority do not effect
SEQ_ARB_WEIGHTED: weight is used for priority
SEQ_ARB_RANDOM: strictly random -- priority do not effect
SEQ_ARB_STRICT_FIFO: support pri
SEQ_ARB_STRICT_RANDOM: support pri
SEQ_ARB_USER
```
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        trans = transaction::type_id::create("trans");
        `uvm_info("SEQ1", "SEQ1 Started", UVM_NONE);
        start_item(trans);
        assert(trans.randomize);
        finish_item(trans);
        `uvm_info("SEQ1", "SEQ1 Ended", UVM_NONE);
    endtask         
endclass

///////////////////////////////////////////////////////

class sequence2 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence2)
    
    transaction trans;
    
    function new(input string inst = "seq2");
        super.new(inst);
    endfunction
    
    virtual task body();
        trans = transaction::type_id::create("trans");
        `uvm_info("SEQ2", "SEQ2 Started", UVM_NONE);
        start_item(trans);
        assert(trans.randomize);
        finish_item(trans);
        `uvm_info("SEQ2", "SEQ2 Ended", UVM_NONE);
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("TRANS");
    endfunction

    virtual task run_phase(uvm_phase phase);
        forever begin
            seq_item_port.get_next_item(t);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seq;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("DRV", this);
        seq = uvm_sequencer#(transaction)::type_id::create("seq", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seq.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
        
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("AGENT", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    sequence1 s1;
    sequence2 s2;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("ENV", this);
        s1 = sequence1::type_id::create("s1");
        s2 = sequence2::type_id::create("s2");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        e.a.seq.set_arbitration(UVM_SEQ_ARB_WEIGHTED);
        fork
            repeat(5) s2.start(e.a.seq, null, 100);  // sequencer, parent sequence, priority, call_pre_post
            repeat(5) s1.start(e.a.seq, null, 200);
        join
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(37) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(41) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(58) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(62) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

## Ways to Hold Access of Sequencer
Holding Sequencer
- priority
- lock method
- grab method

## Holding Access of Sequencer
Priority (sequence with higher priority will complete its transactions first):
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        repeat(3) begin
            trans = transaction::type_id::create("trans");
            `uvm_info("SEQ1", "SEQ1 Started", UVM_NONE);
            start_item(trans);
            assert(trans.randomize);
            finish_item(trans);
            `uvm_info("SEQ1", "SEQ1 Ended", UVM_NONE);
        end
    endtask         
endclass

///////////////////////////////////////////////////////

class sequence2 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence2)
    
    transaction trans;
    
    function new(input string inst = "seq2");
        super.new(inst);
    endfunction
    
    virtual task body();
        repeat(3) begin
            trans = transaction::type_id::create("trans");
            `uvm_info("SEQ2", "SEQ2 Started", UVM_NONE);
            start_item(trans);
            assert(trans.randomize);
            finish_item(trans);
            `uvm_info("SEQ2", "SEQ2 Ended", UVM_NONE);
        end
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("TRANS");
    endfunction

    virtual task run_phase(uvm_phase phase);
        forever begin
            seq_item_port.get_next_item(t);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seq;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("DRV", this);
        seq = uvm_sequencer#(transaction)::type_id::create("seq", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seq.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
        
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("AGENT", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    sequence1 s1;
    sequence2 s2;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("ENV", this);
        s1 = sequence1::type_id::create("s1");
        s2 = sequence2::type_id::create("s2");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        e.a.seq.set_arbitration(UVM_SEQ_ARB_STRICT_FIFO);
        fork
            s1.start(e.a.seq, null, 100);
            s2.start(e.a.seq, null, 200);  // s2 has a higher prio          
        join
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(38) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(61) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(65) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(61) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(65) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(61) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(65) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(42) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(38) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(42) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(38) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(42) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

Lock (once a sequence gets a hold of the lock, it will complete its transactions):
```
///////////////////////////////////////////////////////

`include "uvm_macros.svh"
import uvm_pkg::*;

///////////////////////////////////////////////////////

class transaction extends uvm_sequence_item;
    rand bit [3:0] a;
    rand bit [3:0] b;
         bit [4:0] y;
         
    function new(input string path = "transaction");
        super.new(path);
    endfunction
    
    `uvm_object_utils_begin(transaction)
        `uvm_field_int(a, UVM_DEFAULT)
        `uvm_field_int(b, UVM_DEFAULT)
        `uvm_field_int(y, UVM_DEFAULT)
    `uvm_object_utils_end
endclass

///////////////////////////////////////////////////////

class sequence1 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence1)
    
    transaction trans;
    
    function new(input string inst = "seq1");
        super.new(inst);
    endfunction
    
    virtual task body();
        lock(m_sequencer);
        repeat(3) begin
            trans = transaction::type_id::create("trans");
            `uvm_info("SEQ1", "SEQ1 Started", UVM_NONE);
            start_item(trans);
            assert(trans.randomize);
            finish_item(trans);
            `uvm_info("SEQ1", "SEQ1 Ended", UVM_NONE);
        end
        unlock(m_sequencer);
    endtask         
endclass

///////////////////////////////////////////////////////

class sequence2 extends uvm_sequence#(transaction);
    `uvm_object_utils(sequence2)
    
    transaction trans;
    
    function new(input string inst = "seq2");
        super.new(inst);
    endfunction
    
    virtual task body();
        lock(m_sequencer);
        repeat(3) begin
            trans = transaction::type_id::create("trans");
            `uvm_info("SEQ2", "SEQ2 Started", UVM_NONE);
            start_item(trans);
            assert(trans.randomize);
            finish_item(trans);
            `uvm_info("SEQ2", "SEQ2 Ended", UVM_NONE);
        end
        unlock(m_sequencer);
    endtask         
endclass

///////////////////////////////////////////////////////

class driver extends uvm_driver#(transaction);
    `uvm_component_utils(driver)
    
    transaction t;
    
    function new(input string inst = "DRV", uvm_component c);
        super.new(inst, c);
    endfunction
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        t = transaction::type_id::create("TRANS");
    endfunction

    virtual task run_phase(uvm_phase phase);
        forever begin
            seq_item_port.get_next_item(t);
            seq_item_port.item_done();
        end
    endtask
endclass

///////////////////////////////////////////////////////

class agent extends uvm_agent;
    `uvm_component_utils(agent)
    
    function new(input string path = "agent", uvm_component parent = null);
        super.new(path, parent);
    endfunction    
    
    driver d;
    uvm_sequencer #(transaction) seq;
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        d = driver::type_id::create("DRV", this);
        seq = uvm_sequencer#(transaction)::type_id::create("seq", this);
    endfunction
    
    virtual function void connect_phase(uvm_phase phase);
        super.connect_phase(phase);
        d.seq_item_port.connect(seq.seq_item_export);
    endfunction
endclass

///////////////////////////////////////////////////////

class env extends uvm_env;
    `uvm_component_utils(env)
    
    agent a;
        
    function new(input string path = "env", uvm_component parent = null);
        super.new(path, parent);
    endfunction        
    
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        a = agent::type_id::create("AGENT", this);
    endfunction
endclass

///////////////////////////////////////////////////////

class test extends uvm_test;
    `uvm_component_utils(test)
    
    env e;
    sequence1 s1;
    sequence2 s2;
    
    function new(input string path = "test", uvm_component parent = null);
        super.new(path, parent);
    endfunction
        
    virtual function void build_phase(uvm_phase phase);
        super.build_phase(phase);
        e = env::type_id::create("ENV", this);
        s1 = sequence1::type_id::create("s1");
        s2 = sequence2::type_id::create("s2");
    endfunction
    
    virtual task run_phase(uvm_phase phase);
        phase.raise_objection(this);
        fork
            s2.start(e.a.seq, null, 200);
            s1.start(e.a.seq, null, 100);                      
        join
        phase.drop_objection(this);
    endtask
endclass

///////////////////////////////////////////////////////

module tb;    
    initial begin
        run_test("test");
    end
endmodule

UVM_INFO @ 0: reporter [RNTST] Running test test...
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(20867) @ 0: reporter [UVM/COMP/NAMECHECK] This implementation of the component name checks requires DPI to be enabled
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(64) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(68) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(64) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(68) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(64) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(68) @ 0: uvm_test_top.ENV.AGENT.seq@@s2 [SEQ2] SEQ2 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(39) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(43) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(39) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(43) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(39) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Started
UVM_INFO C:/eng_apps/vivado_projects/08_Sequence/08_Sequence.srcs/sim_1/new/tb.sv(43) @ 0: uvm_test_top.ENV.AGENT.seq@@s1 [SEQ1] SEQ1 Ended
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(19968) @ 0: reporter [TEST_DONE] 'run' phase is ready to proceed to the 'extract' phase
UVM_INFO C:/Xilinx/Vivado/2023.2/data/system_verilog/uvm_1.2/xlnx_uvm_package.sv(13673) @ 0: reporter [UVM/REPORT/SERVER] 
--- UVM Report Summary ---
```

Grab:
- Grab is like a lock, but is put at the front of the arbitration queue(served immediately). Lock is put at the end of the arbitration queue.
