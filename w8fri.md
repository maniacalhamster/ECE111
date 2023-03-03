## Term Project

Set to be released later tonight, prof will be providing onboarding to starter code today.

HW8 is a checkpoint for Term Project.


```ditaa {cmd, args=[-E]}
                                 Channel
                                  +---+
                                  | 0 |                     
                                  | 1 |                     
               +-----------+(RxT) | . | (Rx) +-----------+  
  (Tx)         |           | TxD  | . | RxD  |           |       (Dx)
Orig Data -/-> | Enc (HW7) | -/-> | . | -/-> | Term Proj | -/-> Decoded Data
           1   |           |  2   | 0 |  2   | Vit. Dec. |  1
               +-----------+      +---+      +-----------+
```

--- 

### Aside

Q: What does larger constraint length (AKA history AKA N)?
- enlarges the hamming distance 
- expands the number of possible paths
- those able to deduce prob to a finer grain
  - 2/5 vs 47/100 (which would've been 2.35/5)
  - more precise 

Consider hamming with p = ceil(log_2(d+1)) 
  - get's more efficient as you get longer
  - but also higher chance of more bit corruption

> Fred Harris did a study on this to compare efficiency vs corruption rate for best hamming rate performance.

  - The viterbi enc sys handles this with convolution. Which is more resistant to MULTIPLE bit errors since it works with a SLIDING WINDOW

---

### Term Proj Starter Code

Viterbi encoder runs way faster than decoder can process outputs
- don't worry though, testbench passes Rxt through channel and store Rx in a buffer for decoder

IRL there will be synchronization challenge
- you don't transmit clk. irl you would have to inject signal bits to section off input
- In Term Proj, we'll assume synchronized to same clk (thankfully)

Testbench will have both Orig Data AND the Decoded Data
- will come with a checker that will compare both and give you a score (DD/OD correct)

IRL there will be certain patterns of bit corruption
- In Term Proj, just randomly picks bits to corrupt
- industry testbenches will generate bit corruption patterns (they have names, I didn't catch them)