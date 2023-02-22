# Convolutional Encoding

```ditaa {cmd args=[-E]}
                       +---->V_1
                       ^
                       | 
            +--->+---->+<----+
            |    |     |     |
        +---+-+--+--+--+--+--+--+
u_f --> | u_f | u_0 | u_1 | u_2 |
        +---+-+--+--+--+--+--+--+
            |    |           |
            +--->+<----------+
                 |
                 v
                 +---->v_2

```

L = constraint length of the code and is equal to k(m-1)

> referenced from http://complextoreal.com/wp-content/uploads/2018/09/convolutional-codes.pdf

HW7 testbench
- 2 instances of encoder
    - one of them gets input with an erroneous bit in it