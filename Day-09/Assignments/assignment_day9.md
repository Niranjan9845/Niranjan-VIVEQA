1. Draw the state diagram for a FSM, the output toggles whenever it detects ‘110’ LSB first
(overlapping). Consider initial value of output as 0.

RTL solution :


module fsm_eg_110_mealy(input clk,rst,i,output y);
wire d0,d1,q0,q1,q0b,q1b;
assign d0=~i;
assign d1=q0&i;
assign y=q1&q0b&i;

d_flip_flop1 a3(clk,rst,d0,q0,q0b);
d_flip_flop1 a4(clk,rst,d1,q1,q1b);
endmodule 

module fsm_eg_110_mealy_tb();
reg clk=0,rst,i;
wire y;

fsm_eg_110_mealy a1(clk,rst,i,y);

always #5 clk=~clk;

always @(posedge clk) begin
$display("%d\t%d",i,y);
end

initial begin
$display("I\tY");

i=0;
rst=1;#20;

i=1;#10;
i=0;#10;
i=1;#10;
i=1;#10;

rst=0;#20;

i=1;#10;
i=0;#10;
i=1;#10;
i=1;#10;
i=1;#10;
i=0;#10;
i=1;#10;
i=1;#10;
i=1;#10;
i=0;#10;
i=1;#10;
i=1;#10;
i=1;#10;
i=0;#10;
i=1;#10;
i=0;#10;
i=1;#10;
i=0;#10;
i=1;#10;
i=1;#10;
$finish;
end

endmodule




2.Design a FSM over { 0, 1, 2, 3 } that will output the largest input digit read so far. e.g. input : 001032 will produce output: 001133
  Write Verilog Codes for above and Verify it in Vivado.


RTL solution :


module asg2_fsm_0123(input [1:0]in,input clk,rst,output [1:0]y);
parameter s0=2'b00,s1=2'b01,s2=2'b10,s3=2'b11;

reg [1:0]state,next_state;

always @(posedge clk) begin
if (rst==1)
state<=s0;
else
state<=next_state;
end

always @(*) begin
case(state)
s0:begin if(in==2'b00)
         next_state=s0;
         else if(in==2'b01)
         next_state=s1;
         else if(in==2'b10)
         next_state=s2;
         else 
         next_state=s3;
         end
         
s1:begin if(in==2'b00)
                  next_state=s1;
                  else if(in==2'b01)
                  next_state=s1;
                  else if(in==2'b10)
                  next_state=s2;
                  else 
                  next_state=s3;
                  end
                  
                  
s2:begin if(in==2'b00)
                next_state=s2;
                else if(in==2'b01)
                next_state=s2;
                else if(in==2'b10)
                next_state=s2;
                else 
                next_state=s3;
                end
                
s3:begin if(in==2'b00)
                next_state=s3;
                else if(in==2'b01)
                next_state=s3;
                else if(in==2'b10)
                next_state=s3;
                else 
                next_state=s3;
                end
endcase
end
assign y=state;
endmodule





module asg2_fsm_0123_tb();
reg [1:0]in;
reg clk=1'b0,rst;
wire [1:0]y;

asg2_fsm_0123 a1(in,clk,rst,y);


always #5 clk=~clk;

always @ (posedge clk) begin
$display("rst=%d\tin=%d\ty=%d",rst,in,y);
end

initial begin
in=0;
rst=1;#20;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
rst=0;
in=0;#5;
in=2;#5;
in=1;#5;
in=2;#5;
in=3;#5;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
rst=1;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
rst=0;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
in=0;#5;
in=2;#5;
in=1;#5;
in=0;#5;
in=3;#5;
rst=1;#10;
rst=0;
in=0;#5;
in=1;#5;
in=1;#5;
in=0;#5;
in=2;#5;
in=2;#5;
in=0;#5;
in=1;#5;
in=0;#5;
in=2;#5;
in=1;#5;
in=3;#5;
in=3;#5;
in=3;#5;
in=0;#5;
in=1;#5;

$finish;
end
endmodule








