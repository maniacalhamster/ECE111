JTAG: Join Tag Action Group
- the P in FPGA

> zoned out during the explanation :/

# Sync vs Async Counters (Lec 7A)

content orig for ECE260A 

supplement to Ch10 W&H

Synch counter | Async Counter
--- | ---
Low propagation delay | higher propagation delay
...

## Asynch Counter

Diagram of sequential FF using $\neg{Q}$ as input clk of next FF

What does FF0 do?
- it's a 1 bit johnson counter 
- NOTE: you actually can't tell coz it's not initialized
- Q' (drives next FF) runs at half clk rate (updates on posedge clk)

What does FF1 do?
- also a 1bit johnson coutner
- Q'' (drives next FF) runs at quarter clk rate (updates on posedge Q')

Similarly
- Q''' runs at eigth clk rate (updates on posedge Q'')
- Q'''' runs at sixteenth clk rate (updates on posedge Q''')

Overall it's a 4bit counter, but what about the sync of all FFs? What's the delay between when FF0 flips and FF3 flips?
- based off propagation delay 

## Synchronous Counter

JK is just a slightly fancier D FF
- J & K high --> toggles

> zoned out during the explanation of JK FF

Sequence of JK FFs
- Q --> JK of next
- Q & Q' --> JK of next
- Q & Q' & Q'' --> JK of next

## Johnson Counter (sync)

FF
- chain of Q' --> Q of next FF
- all driven by the same clk

## Poly Counter: Max. 2^N - 1 states (all but all 0s)

Diagram example of a Feedback tap 
- XOR (11 ^ 13 ^ 14 ^ 16)

> Phones use 40 bits --> 2^40 - 1 diff combos. Unique tap patterns ensure signals don't interfere

Revisit the Synchronous Counter from earlier
- Issue: cascading ANDs get's `S L O W` for larger bits

## Question: Doesn't CLEAR screw everything up?

Live Demo time

Doesn't the inclusion of negedge clear interfere with the overarching goal of synchronicity on posedge clk
```sv
always_ff @(posedge clk, negedge clr)
```
- Only diff is: allows clears to happen at ANYTIME
> whatever comes first, we'll do that 

```sv
always_ff @(posedge clk)
```
- Otherwise, can only handle clr on posedge clk

```sv
always_ff @(posedge clk, posedge clr)
```
- ASYNC CLEAR TOO (doesn't require BOTH to hit posedge, it's EITHER /R)

```sv
always_ff @(posedge clk, posedge clr) begin
    if (reset):
        Q<= 0;
    else if (set):
        Q<= 1;
    else 
        Q<= D;
end
```
- ASYNC set occurs since RESET has precedence over clk

```sv
always_ff @(posedge clk, posedge clr) begin
    if (set):
        Q<= 1;
    else if (reset):
        Q<= 0;
    else 
        Q<= D;
end
```
- ERRONEOUS. will backfire
- SYNC set occurs since reset does not have precedence

Scenario:
```
             v SET happens, NOT RESET (precedence in code)
R: 0         1
S: 0     1
C: 0   1   0   1   0
```
- Fix: `if (set & !reset):`{.sv} makes the conditional EXPLICIT
- otherwise implicit precedence occurs

Alternatively, you can do a case like this too:
```sv
always_ff @(posedge clk)
    case{(en, reset, set)}
        0:
        1:
        2:
        3:
        4:
        5:
        6:
        7:
```


Regardless of asynch clear, since data bits change on synch clear we consider it synchronous. Async clear is in an otherwise synchronous circuit.

# Main Lecture Sequence

260C will explain everything about clocking blocks

## Swapping of vars using blocking and non-blocking 

```sv
always @(posedge clk)
    p = q;

always @(posedge clk)
    q = p;
```

```sv
always_ff @(posedge clk)
    p <= q;

always_ff @(posedge clk)
    q <= p;
```

> It's idiot-proof! [lecture hall laugh track] "Any idiot knows, but of course you're not just any idiot..."

SV RTL Modeling Events Scheduling Flow
- Active Events Region
    Intermixed and executes below mentioned in any order
    - Execute programming statements and operators
        - All continuous ass
            - Evaluate RHS and update LHS
        - All blocking ass
            - Evaluate RHS and update LHS
        - **All non-blocking ass**
            - **Step 1. Evaluate RHS**
    - Evaluate and print output from \$display, \$write and \$finish
    - Evaluate inputs and update outputs of primitivecs
- Inactive Events Region
    - All #0 Blocking Assignments
- NBA Update Events Region
    - **All non-blocking ass**
        - **Step 2. Update LHS**

> Note: non-blocking will cost you extra computer memory and time since it generates all those temp vars to accurately simulate.