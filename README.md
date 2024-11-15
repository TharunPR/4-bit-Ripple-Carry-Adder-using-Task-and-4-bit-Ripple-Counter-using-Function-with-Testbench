# 4-bit Ripple Carry Adder using Task and 4-bit Ripple Counter using Function with Testbench

**Aim:** <br>
<br>
&emsp;&emsp;To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.<br>
&emsp;&emsp;To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.<br>
<br>
**Apparatus Required:** <br>
<br>
&emsp;&emsp;Computer with Vivado or any Verilog simulation software.<br>
&emsp;&emsp;Verilog HDL compiler.<br>
<br>
**Verilog Code for Ripple Carry Adder:** <br>
```

module ripple_carry_adder_4bit (
    input [3:0] A,      // 4-bit input A
    input [3:0] B,      // 4-bit input B
    input Cin,          // Carry input
    output [3:0] Sum,   // 4-bit Sum output
    output Cout         // Carry output
);
    reg [3:0] sum_temp;
    reg cout_temp;
    task full_adder;
        input a, b, cin;
        output sum, cout;
        begin
            sum = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (cin & a);
        end
    endtask
    always @(*) begin
        full_adder(A[0], B[0], Cin, sum_temp[0], cout_temp);
        full_adder(A[1], B[1], cout_temp, sum_temp[1], cout_temp);
        full_adder(A[2], B[2], cout_temp, sum_temp[2], cout_temp);
        full_adder(A[3], B[3], cout_temp, sum_temp[3], Cout);
    end
    assign Sum = sum_temp;
endmodule

```

**Test bench for Ripple Carry Adder:** <br>
```

module ripple_carry_adder_4bit_tb;
    reg [3:0] A, B;
    reg Cin;
    wire [3:0] Sum;
    wire Cout;
    ripple_carry_adder_4bit uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );
    initial begin
        A = 4'b0001; B = 4'b0010; Cin = 0;
        #10;
        A = 4'b0110; B = 4'b0101; Cin = 0;
        #10;
        A = 4'b1111; B = 4'b0001; Cin = 0;
        #10;
        A = 4'b1010; B = 4'b1101; Cin = 1;
        #10; 
        A = 4'b1111; B = 4'b1111; Cin = 1;
        #10;
        $stop;
    end
    initial begin
        $monitor("Time = %0t | A = %b | B = %b | Cin = %b | Sum = %b | Cout = %b", $time, A, B, Cin, Sum, Cout);
    end
endmodule

```

**Verilog Code for Ripple Carry Counter:** <br>
```

module ripple_counter_4bit (
    input clk,           // Clock signal
    input reset,         // Reset signal
    output reg [3:0] Q   // 4-bit output for the counter value
);
    function [3:0] next_state;
        input [3:0] curr_state;
        begin
            next_state = curr_state + 1;
        end
    endfunction
    always @(posedge clk or posedge reset) begin
        if (reset)
            Q <= 4'b0000;       // Reset the counter to 0
        else
            Q <= next_state(Q); // Increment the counter
    end
endmodule

```

**TestBench for Ripple Carry Counter:** <br>
```

module ripple_counter_4bit_tb;
    reg clk;
    reg reset;
    wire [3:0] Q;
    ripple_counter_4bit uut (
        .clk(clk),
        .reset(reset),
        .Q(Q)
    );
    always #5 clk = ~clk;
    initial begin
        clk = 0;
        reset = 1;
        #20 reset = 0;
        #200 $stop;
    end
    initial begin
        $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
    end
endmodule

```
**Sample Output for Ripple Carry Adder:**
```

Time = 0 | A = 0001 | B = 0010 | Cin = 0 | Sum = 0011 | Cout = 0
Time = 10000 | A = 0110 | B = 0101 | Cin = 0 | Sum = 1011 | Cout = 0
Time = 20000 | A = 1111 | B = 0001 | Cin = 0 | Sum = 0000 | Cout = 1
Time = 30000 | A = 1010 | B = 1101 | Cin = 1 | Sum = 1000 | Cout = 1
Time = 40000 | A = 1111 | B = 1111 | Cin = 1 | Sum = 1111 | Cout = 1

```

**Sample Output for Ripple Carry Counter:**
```

Time = 0 | Reset = 1 | Q = 0000
Time = 20000 | Reset = 0 | Q = 0000
Time = 25000 | Reset = 0 | Q = 0001
Time = 35000 | Reset = 0 | Q = 0010
Time = 45000 | Reset = 0 | Q = 0011
Time = 55000 | Reset = 0 | Q = 0100
Time = 65000 | Reset = 0 | Q = 0101
Time = 75000 | Reset = 0 | Q = 0110
Time = 85000 | Reset = 0 | Q = 0111
Time = 95000 | Reset = 0 | Q = 1000
Time = 105000 | Reset = 0 | Q = 1001
Time = 115000 | Reset = 0 | Q = 1010
Time = 125000 | Reset = 0 | Q = 1011
Time = 135000 | Reset = 0 | Q = 1100
Time = 145000 | Reset = 0 | Q = 1101
Time = 155000 | Reset = 0 | Q = 1110
Time = 165000 | Reset = 0 | Q = 1111
Time = 175000 | Reset = 0 | Q = 0000
Time = 185000 | Reset = 0 | Q = 0001
Time = 195000 | Reset = 0 | Q = 0010
Time = 205000 | Reset = 0 | Q = 0011
Time = 215000 | Reset = 0 | Q = 0100

```
<br>

**Output Waveform for Ripple Carry Adder:** 

<br>

![Screenshot 2024-11-14 141455](https://github.com/user-attachments/assets/6ab8bee1-b8b8-4b6a-a907-edfa55d3f224)

<br>

**Output Waveform for Ripple Carry Counter:**

<br>

![Screenshot 2024-11-14 141906](https://github.com/user-attachments/assets/836583f0-0891-4116-9259-805356da711c)

<br>

**Conclusion:** <br>

<br>

&emsp;&emsp;The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations.The simulation results matched the expected outputs.<br>
&emsp;&emsp;The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.<br>

