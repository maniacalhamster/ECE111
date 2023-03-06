# Convolutional Coding

## Non-recursive, Non-systematic

(HW7)

- constraint length?
    > 2
- rate?
    > 1/3
- why "non-recursive"?
    > No feedback
- why "non-systematic"?
    > No output2 (orig data)

## Recursive, Systematic

- constraint length?
    > 3
- rate?
    > 1/2
- why "recursive"?
    > Output1 feeds back into reg 1
- why "systematic"?
    > Output2 shows orig data

## Which to use?

- Viterbi decoder obv has to know encoders choice of:
  - rate
  - constraint length
  - recursiveness
  - tap choice 

- Recursve favored in turbo codes
  - out of scope for this code
  - WES269 or ECE260C if interested

## Starter Code Overview

TBU - traceback unit
  - tracks the sequence of input bits assumed to get to final state / score

> TODO: rewatch podcast and fill in notes. Working on resolving issues at work and wasn't fully focused on lecture.