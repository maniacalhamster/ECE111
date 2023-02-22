Homework 7 tips

How to do programmable taps?
- Use ANDS
- reduction XOR will always take N inputs
  - BUT they are drawn from the ANDs


```
      +-------+-+-+--(+)---
      |       | | |
prgm->D------>D>D>D
      |       | | |
 --> [ ] --> [ | | ]
      |       | | |
prgm->D------>D>D>D
      |       | | |
      +-------+-+-+--(+)---
```

> If interested in error correction: 
> Turbo code, lattice codes, etc.


How to tell, after err correction, if you got ALL errors?
- Append a CRC

Reading the Path Metric Graph (for decoder):
- columns represent incoming bits (from encoder) at each clk cycle
- rows represent the current state
- arrows: represent the state transition based off input bit
    - black arrow: represent an input bit of 0
    - dotted arrow: represents an input bit of 1
- path metric: how many bits were correct along this path
    - for converging paths, have a list of path metrics and choose the most likely
    > using hard decision, not soft decision
    - path metric / num cols = prob of this path 

Notes on path matric graph:
- After N steps, we could end up at 2^N states BUT we run out of states!
    - Meaning: we end up converging after N=log_2(num_states) steps


> Term Project: Given encoded output with some bit corruptions. We need to clean up the errors and decode.
  - Goal: Calculate path metrics 
  - find best path for decode for term proj

*Example Time*

Curr
  - State: 000
  - Incoming Bit: 01

Interpretation
  - First bit got corrupted but we don't know which one
  - If input bit was 0, expect 00
  - If input bit was 1, expect 11
  - It's a 50/50 chance of either, We don't know which one

Refer to `Files > Homework_Project > Final Project` for ppt and pdfs on Viterbi stuff