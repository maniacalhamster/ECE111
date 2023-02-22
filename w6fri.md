> Note: ~15 minutes late, started taking notes at 8:15. 
> While walking in, heard something to about "corrolary", "feedforward loops", and "half clk"

XOR: difference
XNOR: equivalence

hw7 don't worry about bad bits, that's the encryption side

term proj starts w/ all good bits

Build viterbi decoder w/ a perfect score (path n/m) assuming NO BAD BITS

Then term proj will start corrupting bits

---

## System Verilog Procedural Block

initial
    - will not be synthesizable into hardware logic
    - testbench material

always
    - synthesizable into actual hardware logic
    - develop RTL code --> specifies design behavior

## Latch vs Flip-Flop

Latch
- controlled by level triggered enable signal (either pos or neg lvl trig)
- piece of memory
- asynchronous (output state changes as soon as input changes --> when active lvl is maintained @ Enable inp)

Flip-Flop
- controlled by edge triggered ctrl sig (e.g. clk) (either posedge or negedge trig)
- synchronous (output repsonds to changes in input only @ posedge/negedge of ctrl sig)

> D-FF built out of complementary Latches

## Always block: always@

Always procedural can be used to model:
- combinational logic
- Clocked sequential logic (e.g. FF)
- Level sensitive logic (e.g. Latch)

Sensitivity list can have any number of sigs and can be specified in mult diff ways:


Sensitivity list must include all inp sigs used by procedural statement within always block to properly model combinational logic:
- Omission of any input signal which impacts behavior of logic, can lead to siulation and 

```sv
always@(posedge clk, negedge reset) begin
    // if (reset)   <-- (negedge reset) & (reset) => 0
    //                  otherwise might trigger on (posedge clk) & reset

    if (!reset) 
        Q<=0
    else
        Q<=D
end
```

> The following are really more like linting tools to avoid logic errors. Turning potential logic errors into syntax errors to avoid common pitfalls.

## always_comb

## always_ff

> TODO: revisit slides

## Dissimilar D-FF (Good SV coding)

> TODO: revisit slides

## Multiple assignment statment on same variable

```sv
always_ff@(posedge clk) begin
    if (add1) result <= result +1;              // This would inf loop if it was in a comb block

    // if (add2) result <= result +2;           // if add1 == add2 = 1, result is assigned to both simultaneously 
                                                // due to non-blocking assignment (ambigious)
    else if (add2) result <= result + 2;        // code WILL synthesize but circuit will not function correctly

    // else result <= result                    // Assumed in FF, but REQUIRED for always_comb (lint)
end
```