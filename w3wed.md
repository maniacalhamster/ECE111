## 

```sv
always_comb case({reset,set})
  2'b00: Q1 = D;
  2'b01: Q1 = 1;
  2'b10: Q1 = 0;
  2'b11: Q1 = 0; //Q1;
endcase

=============================

always_ff @(posedge clk)
  if(reset)                     // holds prio, handles 01 and 11
	Q1 <= 0;
  else if (set)                 // prio: reset > set
	Q1 <= 1;
  else
	Q1 <= D;

==============================

always_ff @(posedgeclk, negedge reset, posedge reset, posedge set) // `negedge reset` would be needed for !reset
  if (!reset)                   // otherwise !reset FAILS, cannot occur since we're watching on `posedge reset`
	Q1 <= 0;
  else if (set)
	Q1 <= 1;
  else
	Q1 <= D;

==============================

always_latch
  if(sel) Q = D1;
//  else	Q = D0;               // Needed for an always_comb but not for always_latch

always_latch case({reset,set})
  2'b00: Q1 = D;
  2'b01: Q1 = 1;
  2'b10: Q1 = 0;
//  2'b11: Q1 = 0; //Q1;        // Likewise, can be ignored (always_comb requires all conditions to be handled)
endcase
```

## Example: ILLEGAL ASSIGNMENT TO A NET (wire)

```sv
output gate         // Default: wire type
                    // Fix: output logic gate

always_comb
  if(sel) grape = 1;
  else    grape = 0;

// ALT: use ternary op

assign grape = sel ? 1 : 0
```