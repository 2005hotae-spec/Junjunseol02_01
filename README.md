

module gates(
    input  wire a, b,
    output wire y_and, y_or, y_xor
);
    assign y_and = a & b;
    assign y_or  = a | b;
    assign y_xor = a ^ b;
endmodule


module gates_tb;
    reg a, b;
    wire y_and, y_or, y_xor;
    gates uut(.a(a), .b(b), .y_and(y_and), .y_or(y_or), .y_xor(y_xor));
    initial begin
        $dumpfile("01_gates_tb.vcd");
        $dumpvars(0, gates_tb);
        a=0; b=0;
        #10 a=0; b=1;
        #10 a=1; b=0;
        #10 a=1; b=1;
    #10 $finish;
    end
endmodule


module half_adder(
    input  wire a, b,
    output wire s, c
);
    assign s = a ^ b;
    assign c = a & b;
endmodule

module full_adder(
    input  wire a, b, cin,
    output wire sum, cout
);
    wire s1, c1, c2;
    half_adder ha1(.a(a),  .b(b),  .s(s1), .c(c1));
    half_adder ha2(.a(s1), .b(cin),.s(sum),.c(c2));
    assign cout = c1 | c2;
endmodule

[Uploamodule full_adder_tb;
    reg a, b, cin;
    wire sum, cout;
    full_adder uut(.a(a), .b(b), .cin(cin), .sum(sum), .cout(cout));
    initial begin
        $dumpfile("02_full_adder_tb.vcd");
        $dumpvars(0, full_adder_tb);
        a=0;b=0;cin=0;
        #10 a=0;b=0;cin=1;
        #10 a=0;b=1;cin=0;
        #10 a=0;b=1;cin=1;
        #10 a=1;b=0;cin=0;
        #10 a=1;b=0;cin=1;
        #10 a=1;b=1;cin=0;
        #10 a=1;b=1;cin=1;
        #10 $finish;
    end
endmodule
ding 02_full_adder_tb.v…]()

[Umodule adder4(
    input  wire [3:0] a, b,
    input  wire cin,
    output wire [3:0] sum,
    output wire cout
);
    assign {cout, sum} = a + b + cin;
endmodule
ploading 03_adder4.v…]()

[Uploadinmodule adder4_tb;
    reg [3:0] a, b;
    reg cin;
    wire [3:0] sum;
    wire cout;
    adder4 uut(.a(a), .b(b), .cin(cin), .sum(sum), .cout(cout));
    initial begin
        $dumpfile("03_adder4_tb.vcd");
        $dumpvars(0, adder4_tb);
        a=4'd5; b=4'd3; cin=0;
        #10 a=4'd15; b=4'd1; cin=0;
        #10 a=4'd10; b=4'd10; cin=1;
        #10 $finish;
    end
endmodule
g 03_adder4_tb.v…]()



module subtractor4(
    input  wire [3:0] a, b,
    output wire [3:0] diff,
    output wire borrow
);
    assign {borrow, diff} = a - b;
endmodule

[Uploadingmodule subtractor4_tb;
    reg [3:0] a, b;
    wire [3:0] diff;
    wire borrow;
    subtractor4 uut(.a(a), .b(b), .diff(diff), .borrow(borrow));
    initial begin
        $dumpfile("04_subtractor4_tb.vcd");
        $dumpvars(0, subtractor4_tb);
        a=4'd9; b=4'd3;
        #10 a=4'd3; b=4'd9;
        #10 a=4'd0; b=4'd0;
        #10 $finish;
    end
endmodule
 04_subtractor4_tb.v…]()

 
module comparator4(
    input  wire [3:0] a, b,
    output wire gt, eq, lt
);
    assign gt = (a > b);
    assign eq = (a == b);
    assign lt = (a < b);
endmodule


module comparator4_tb;
    reg [3:0] a, b;
    wire gt, eq, lt;
    comparator4 uut(.a(a), .b(b), .gt(gt), .eq(eq), .lt(lt));
    initial begin
        $dumpfile("05_comparator4_tb.vcd");
        $dumpvars(0, comparator4_tb);
        a=4'd5; b=4'd3;
        #10 a=4'd3; b=4'd5;
        #10 a=4'd7; b=4'd7;
        #10 $finish;
    end
endmodule

[Uplmodule mux4to1(
    input  wire [3:0] d,
    input  wire [1:0] sel,
    output wire y
);
    assign y = d[sel];
endmodule
oading 06_mux4to1.v…]()


module mux4to1_tb;
    reg [3:0] d;
    reg [1:0] sel;
    wire y;
    mux4to1 uut(.d(d), .sel(sel), .y(y));
    initial begin
        $dumpfile("06_mux4to1_tb.vcd");
        $dumpvars(0, mux4to1_tb);
        d=4'b1010;
        sel=2'b00;
        #10 sel=2'b01;
        #10 sel=2'b10;
        #10 sel=2'b11;
        #10 $finish;
    end
endmodule



module demux1to8(
    input  wire din,
    input  wire [2:0] sel,
    output wire [7:0] y
);
    assign y = din ? (8'b1 << sel) : 8'b0;
endmodule




module demux1to8_tb;
    reg din;
    reg [2:0] sel;
    wire [7:0] y;
    demux1to8 uut(.din(din), .sel(sel), .y(y));
    initial begin
        $dumpfile("07_demux1to8_tb.vcd");
        $dumpvars(0, demux1to8_tb);
        din=1; sel=3'd0;
        #10 sel=3'd3;
        #10 sel=3'd7;
        #10 din=0; sel=3'd3;
        #10 $finish;
    end
endmodule 

module encoder8to3(
    input  wire [7:0] d,
    output reg [2:0] y
);
    always @(*) begin
        casez (d)
            8'b1???????: y = 3'd7;
            8'b01??????: y = 3'd6;
            8'b001?????: y = 3'd5;
            8'b0001????: y = 3'd4;
            8'b00001???: y = 3'd3;
            8'b000001??: y = 3'd2;
            8'b0000001?: y = 3'd1;
            8'b00000001: y = 3'd0;
            default:     y = 3'd0;
        endcase
    end
endmodule


module encoder8to3_tb;
    reg [7:0] d;
    wire [2:0] y;
    encoder8to3 uut(.d(d), .y(y));
    initial begin
        $dumpfile("08_encoder8to3_tb.vcd");
        $dumpvars(0, encoder8to3_tb);
        d=8'b00000001;
        #10 d=8'b00010000;
        #10 d=8'b10000000;
        #10 $finish;
    end
endmodule

module decoder3to8(
    input  wire [2:0] sel,
    output wire [7:0] y
);
    assign y = 8'b1 << sel;
endmodule



module decoder3to8_tb;
    reg [2:0] sel;
    wire [7:0] y;
    decoder3to8 uut(.sel(sel), .y(y));
    initial begin
        $dumpfile("09_decoder3to8_tb.vcd");
        $dumpvars(0, decoder3to8_tb);
        sel=3'd0;
        #10 sel=3'd3;
        #10 sel=3'd7;
        #10 $finish;
    end
endmodule



module seg7decoder(
    input  wire [3:0] bcd,
    output reg [6:0] seg
);
    always @(*) begin
        case (bcd)
            4'd0: seg = 7'b0111111;
            4'd1: seg = 7'b0000110;
            4'd2: seg = 7'b1011011;
            4'd3: seg = 7'b1001111;
            4'd4: seg = 7'b1100110;
            4'd5: seg = 7'b1101101;
            4'd6: seg = 7'b1111101;
            4'd7: seg = 7'b0000111;
            4'd8: seg = 7'b1111111;
            4'd9: seg = 7'b1101111;
            default: seg = 7'b0000000;
        endcase
    end
endmodule


module seg7decoder_tb;
    reg [3:0] bcd;
    wire [6:0] seg;
    seg7decoder uut(.bcd(bcd), .seg(seg));
    initial begin
        $dumpfile("10_seg7decoder_tb.vcd");
        $dumpvars(0, seg7decoder_tb);
        bcd=4'd0;
        #10 bcd=4'd5;
        #10 bcd=4'd8;
        #10 bcd=4'd9;
        #10 $finish;
    end
endmodule





