> Note: was ~15min late to lecture. Started taking notes @8:20AM

## Homework 6

> Quick Aside
> : hw5 is simple - convolutional (LSFR) is a barrel shifter with Taps feeding into the [0] 
> (Just a barrel shifter on steriods)

### Gray Code

Gray Code is an ordering of binnary numeral system such that two successive numbers differ in only 1 bit

```
binary: differ by 7 bits!!
0111 1111
1000 0000

gray: only 1 bit changes between each
00
01
11
10
```

bin | grey | grey_dec
--- | ---  | ---
000 | 000  | 0
001 | 001  | 1
010 | 011  | 3
011 | 010  | 2
100 | 110  | 6
101 | 111  | 7
110 | 101  | 5
111 | 100  | 4

> I can follow the logic and figure out the next grey numeral in the sequence but don't really have an algorithm/equation to convert in my head right now.

Hint:
- Gray to Binary: serial XORS
- Binary to Gray: parallel XORS

---

### Carry-Lookahead Adder

```ditaa {cmd}
            +-------------------+
A[N-1:0] -->|                   |
B[N-1:0] -->| carry_lookahead   |---> result[N:0]
CIN      -->| _adder            |
            +-------------------+
```

Carry-Ripple: brute force way learned in Grade school
- Carry is the critical path for the combinational delay --> optimizing this will minimize clk period
- A few extra gates, pay for the hardware --> faster execution of the common case ==> worth overall
```
  PPPG
  111
  0001
  1111
  ----
 10000
```

- G: **Generate** col. Gaurantees to create a carry (1 => cout)
  - AND (11)
- P: **Propagate** col. Will let carry flow through (cin => cout)
  - XOR (01 or 10)
- K: **Kill** col. Will kill the carry (0 => cout)
  - NOR (00)

---

### Clock divide by 3

```wavedrom
{ "signal" : [
    { "name": "clk_in", wave: "lhlhlhl"},
    { "name": "clk_out", wave: "H..L..H"},
]}
```