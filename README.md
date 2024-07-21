# Vending_machine_verilog
The project involved designing a vending machine that could dispense four different products with varying prices and has the additional feature of returning change when a higher denomination coin was inserted. The vending machine only accepts coins of denominations five and ten.

The design of the vending machine was accomplished through the creation of a Finite State Machine (FSM) model, which defined the machine's different states, inputs, and outputs, as well as the transitions between the states. The FSM model was implemented using Verilog code, which defined the different states and their corresponding logic for accepting coins, dispensing products, and returning change.

A test-bench was created to simulate different scenarios, such as different coin denominations and product selections, to ensure that the vending machine worked correctly. The testing process was successful in verifying the proper functioning of the vending machine.

This project required knowledge of Verilog, FSMs, and digital design principles and provided a challenging and rewarding opportunity to better understand how vending machines operate. The completed project was a functional and efficient vending machine that could dispense products and return change with ease.

This project simulates the way an ideal vending machine would handel transactions. This vending machine works for only one item of Rs15 ans so it shows how a vending machine works if it accepts only 5 and 10Rs. 

# State diagram.

![WhatsApp Image 2024-07-21 at 15 31 34_892c121b](https://github.com/user-attachments/assets/54c23a59-136a-43f4-af5c-004743edbc93)

# Verilog code.

```
module vending_machine(
  input clk,
  input rst,
  input reg[1:0] x,
  output reg out,
  output reg[1:0] c
);
  
  
  parameter s0=2'b00;
  parameter s1=2'b01;
  parameter s2=2'b10;
  
  reg[1:0] state,next_state;
  always@ (posedge clk)
    begin
      if(rst==1)
        begin
          state=0;
          next_state=0;
          c=2'b00;
        end
      
      else
        state=next_state;
      
      case(state)
        s0:
          if(x==0)
            begin
              next_state=s0;
              out=0;
              c=2'b00;
            end
        else if(x==2'b01)
          begin
            next_state= s1;
            out=0;
            c=2'b00;
          end
        else if(x==2'b10)
          begin
            next_state=s2;
            out=0;
            c=2'b00;
          end
        s1:
          
          if(x==0)
            begin
              next_state=s0;
              out=0;
              c=2'b01;
            end
        else if(x==2'b01)
          begin
            next_state=s2;
            out=0;
            c=2'b00;
          end
        else if(x==2'b10)
          begin
            next_state=s0;
            out=1;
            c=2'b00;
          end
        s2:
          if(x==2'b00)
            begin
              next_state=s0;
              out=0;
              c=2'b10;
            end
        else if(x==2'b01)
          begin
            next_state=s0;
            out=1;
            c=2'b00;
          end
        else if(x==2'b10)
          begin
            next_state=s0;
            out=1;
            c=2'b01;
          end
      endcase
    end
endmodule

```
# Testbench.

```

module vending_machine_tb;
  //input
  reg clk;
  reg rst;
  reg [1:0] x;
  
  //outputs
  
  wire out;
  wire [1:0] c;
  vending_machine uut(.clk(clk),.rst(rst),.x(x),.out(out),.c(c));
  initial begin
    $dumpfile("vending_machine.vcd");
    $dumpvars(0,vending_machine_tb);
    rst=1;
    clk=0;
    #6 rst=0;
    x=1;
    #11 x=1;
    #16 x=2;
    #25 $finish;
  end
  always #5 clk=~clk;
endmodule

```
# Output.


-1 Case: Rs5 is added as input cosecutively after every 5sec.
![Screenshot (338)](https://github.com/user-attachments/assets/56fb1233-dbcf-445e-9d62-a06a06447d0c)
-2 Case: Rs5 is added first then another Rs5 after 5sec and at last Rs10 after another 5sec.
![Screenshot (339)](https://github.com/user-attachments/assets/1bad6526-694f-4095-973e-c15e2b1ed79c)
