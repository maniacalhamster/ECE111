# Shifters

Slides will be uploaded later today. Prof wanted to talk about them a bit.

- Logical 
- Arithmetic

### Funnel Shifter

Shift type | $Z_{N-2:N}$ | $Z_{N-1}$ | $Z_{N-2:0}$ | Offset
--- | --- | --- | --- | ---
Rotate Right | $A_{N-2:0}$ | $A_{N-1}$ | $A_{N-2:0}$ | k
Logical Right| 0 | $A_{N-1}$ | $A_{N-2:0}$ | k
Arithmetic Right| $A_{N-2:0}$ | $A_{N-1}$ | $A_{N-2:0}$ | k
Rotate Left| $A_{N-2:0}$ | $A_{N-1}$ | $A_{N-2:0}$ | k
Logical/Arithmetic Left| $A_{N-2:0}$ | $A_{N-1}$ | $A_{N-2:0}$ | k

> TODO: Update Funnel Shifter chart and fill in Shifter notes

## Array Funnel Shifter

- N N-input MUX
  - Use of 1-of-N hot select signals ==> hshift amount
  - nMOS pass transistor design ($V_t$ drops)

> TODO: copy physical/layout diagram

## Logarithmic Funnel Shifter
- Log N stages of 2-inut MUXes
  - No select decoding needed

> TODO: copy circuit diagram
- Each row of MUXes let you shift by 2^N more (logarithmically more)

### 32-bit Logarithmic Funnel
- wider MUXes reduce delay adn power
- Operands > 32 bits introduce datapath irregularity 

> TODO: copy citcuit diagram
- 4:1 --> 8:1 
- Those wires need to be placed in layers on silicon! Too much ==> weird 3d manuevers ==> physical weak spots ==> potential for break

## Logarithmic Barrel Shifter

> TODO: revisit slides

> Arithmetic: 2's complement + preserve negativity 

```
-2 >> 2         -2 > 2
--------   vs   ---------
-2: 1100        -2: 1100
 6: 0110        -1: 1110
```

> TODO: revisit the explanation on `flr(N/m)` discussed and fractions

### 32-bit Logarithmic Barrel

> TODO: revisit slides

# Back to Main Lecture Notes

## Concurrent Procedural Assignments with Inter-Delay
- [ ] Concurrent **blocking assignents** have unpredictable results due to race condition
    - Order of concurrent evaluation is indeterministic and unpredictable simulation result!

```sv
always@(posedge clk) begin
    #4 a = a + 2;

    // Delay 4 time units due to inter ass delay, then eval RHS and ass to 'a' immed
end

always@(posedge clk) begin
    #4 b = a + 3;

    // Delay 4 time units due to inter ass delay, then eval RHS and ass to 'b' immed
end

// Simulation Output (assuming a=0, b=0 @ 0ns)
// -----------------
// Unpredictable Result !!
// new val of 'b' could be eval before OR after 'a' changes
```

> TODO: copy timing diagram from slides

- [ ] Concurrent **non-blocking assignments** have predictable results
  - Order of concurrent eval is indeterministic, but predictable simulation result!

```sv
always@(posedge clk) begin
    #4 a <= a + 2;

    // Delay 4 time units due to inter ass delay, then eval RHS
    // then ass to 'a' AT END OF TIME STEP (1 clk per + 4ns)
end

always@(posedge clk) begin
    #4 b <= a + 3;

    // Delay 4 time units due to inter ass delay, then eval RHS 
    // then ass to 'b' AT END OF TIME STEP (1 clk per + 4ns)
end

// Simulation Output (assuming a=0, b=0 @ 0ns)
// -----------------
// Predictable Result !!
// After 1st posedge clk + 4ns
// a = 2 and b = 3
```

> TODO: revisit podcast to write notes on the concurrent procedural assignments under various conditions (intra - delays)

## Rules
- [ ] Not reccomended (not alloweed in design) to have both blocking and non-blocking assignments statemnets in the same always block
- [ ] Synthesis compiler will ignore inter and intra delays in both blocking and non-blockig procedural assignment statement
- [ ] Same variable cannot have both blocking and non-blocking assignments to it. Below mention

> TODO: copy over code snippets

## D-FF Model using Non-blocking assignment
```sv
module dff(
    input clk, d,
    output logic q
);

always_ff@(posedge clk) begin
    q <= d;     // when clk rises, copy 'd' to 'q'
end
endmoduel: dff
```
- When clk is not rising, value of q is preserved (memorized)
- Synthesis will produce a positive edge-trigged D-FF

```wavedrom
{ "signal" : [
    { "name": "clk", wave: "lHlHlHl", node: ".1.2.3" },
    { "name": "d", wave: "l1.0..."},
    { "name": "q", wave: "l..h.l."},
]}
```
- value of 'q' is retained until next posedge of clk

### Example of blocking vs non-blocking assignment
```sv
module parallel_registers (
    input clk, d,
    output logic q1, q2
);

always@(posedge clk) // avoid!
begin
    q1 = d;
    q2 = q1;
end
endmodule
```
- q1 is not connected to q2 and both q1 and w2 will get same value of d in same clock cycle

> TODO: copy circuit diagram of parallel regs

```sv
module shift_register (
    input clk, d,
    output logic q1, q2
);

always@(posedge clk) // avoid!
begin
    q1 <= d;
    q2 <= q1;
end
endmodule
```
- q1 is serially connected to q2. New value of d is first propagated to q1 and one clock cycle later it will propagate to q2

> TODO: copy circuit diagram of shift reg

> TODO: revisit slide on separating non-blocking assignments 

## Shift Register using Non-Blocking Assignments
```sv
module shift_register (
    input clk, d,
    output logic q
)
logic[2:0] t
always@(posedge clk)
begin
    t[0] <= d;
    t[1] <= t[0]
    t[2] <= t[1]
    q <= t[2];
end
endmodeule
```