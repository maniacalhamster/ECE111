# Discussion

## Term Project Structure

Top Level (`viterbi_tx_rx_tb.sv`)
- Generator (43:357): Keeps history of expected values
- Monitor (33:37): Keeps history/Observes student soln decoder
    - Note the `33: #410400` offset to wait for generator to finish
- Checker (358:371): scoreboard to compare generated vs student decoder output

### 5 part environment
Generator --> Encoder --> Channel --> Decoder --> Scoreboard

   - Note the `358: #1000000` to wait for student decoder to finish

"Second layer" of testbench (`viterbi_tx_rx.sv`)
- `26:27`: error counter used to generate an error every 16th bit
    - can modify `4'b1111` to manipulate
        - phase: change value (0:14)
        - freq: reduce width to `n'b` to make it more or less frequent
- `32:41`: Hw7 encoder inserted here
    - Q: how do we wire in our encoder into hw8 starter code? Some of the signals seem new

```sv
// HW8 encoder instantiation
// Files
encoder encoder1 (
    .clk,
    .rst,
    .enable_i(enable_encoder_i),
    .d_in(encoder_i),
    .valid_o(valid_encoder_o), 
    .d_out(encoder_o)
);

// Hw7 encoder definition
// Canvas File: Final Project/assignment itself/vitassign.zip
module encoder (
    //...
    always_comb begin
        valid_o = enable_i;
        case(cstate)
        // fill in the guts
        endcase
    end

    //...
)
```
- `enable_i` is separate from `valid_o` because it might not be synced up during runtime. 
- `cstate` and `nstate`: current state and next state used to determine what the next state should be (Moore Machine)
- 8 parallel encoders are run to get states, used to perform:
    - Path Metric Computation generated for each path

`bmc000.sv`: Branch Metric Computation
    - 8 instatiations. 
    - Tracks the metrics for each path

`ACS.sv`: add-compare-subtract
    - 8 instantiations used
    - pretty much identitical constructions

`TBU.sv`: traceback unit
    - used once branch with best branch metric chosen
    - works backward to obtain the "right" data

`encoder_tb.sv`: TestBench to check if encoder works
> Note: don't mess with values. It's "synced" with generator input.

File: Final Project > Lecture/Ppts > Viterbi Explained.pptx
- Examples using the Trellis to determine path metric and most likely path

## HW8

Due AFTER term project. 

supposed to be a fun 5 point assignment afterwards.
- just going to be pushing viterbi to breaking point