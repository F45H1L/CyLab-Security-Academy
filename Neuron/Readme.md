# Neuron

## Neuron Meet 2D-0

this is a black-box 2D perceptron challenge. The key is to recover enough of the decision boundary to deliberately produce the final 8 outputs:

`01110000` → ASCII p.

You can automate the probing rather than manually guessing.

### 1. Connect
```bash
nc aureolin-pixie.cylabacademy.net <LAB-ID>
```
```
Welcome to Neuron Meet 2D-0!
Probe the 2D perceptron to coax out the ASCII for 'p'.
Send two numbers (x, y) to see if the perceptron fires (1) or stays quiet (0).
- Bounds: [-10.0, 10.0] for both x and y
- Output rule: w1*x + w2*y + b >= 0 -> 1, else 0.
- No back-to-back repeats of the same (x, y) pair.
- Goal: make the last 8 outputs read 01110000 (ASCII 'p').
- Command: RESET to clear the firing history.
- Format: x,y or x y (comma or space separated)
Type HELP for a reminder or EXIT to quit.

[1/128] (x,y)>
```

### 2. Sending Pairs

You'll likely be able to send pairs such as `1,1` and receive 0 or 1.

Because repeated inputs are blocked, every (x,y) pair you submit must be unique.

After sending random values I got to know that all positive points like (2,2), (3,3), (4,4) produce 1 where (1,1) is an exception producing 0, while negative-x/positive-y points produce 0.

So try sending the following pairs in order:
* `1,1`
* `2,2`
* `3,3`
* `4,4`
* `-2,2`
* `-3,3`
* `-4,4`
* `-5,5`

The output would look like:
```
[1/128] (x,y)> 1,1
Perceptron stays quiet.
Recent outputs (1/8): 0
[2/128] (x,y)> 2,2
Perceptron fires!
Recent outputs (2/8): 01
[3/128] (x,y)> 3,3
Perceptron fires!
Recent outputs (3/8): 011
[4/128] (x,y)> 4,4
Perceptron fires!
Recent outputs (4/8): 0111
[5/128] (x,y)> -2,2
Perceptron stays quiet.
Recent outputs (5/8): 01110
[6/128] (x,y)> -3,3
Perceptron stays quiet.
Recent outputs (6/8): 011100
[7/128] (x,y)> -4,4
Perceptron stays quiet.
Recent outputs (7/8): 0111000
[8/128] (x,y)> -5,5
Perceptron stays quiet.
Recent outputs (8/8): 01110000
Pattern matched! ASCII 'p' unlocked. Here is your flag:
academy{2d_n3ur0n_m3t_8c453797}
```
And the flag:
```
academy{2d_n3ur0n_m3t_8c453797}
```

## Neuron Meet 0

This challenge is a 1D perceptron threshold-finding problem.

The key equation is:

`$$ output = \begin{cases} 1 & wx+b \ge 0\\ 0 & wx+b < 0 \end{cases} $$`

You don't need to find w and b themselves. You only need to discover which input values produce 0 and which produce 1, then alternate values from the two sides so the final 8 outputs are:

`01110000`

### 1. Connect
```bash
nc aureolin-pixie.cylabacademy.net <LAB-ID>
```
```
Welcome to Neuron Meet 0!
Probe the 1D perceptron to coax out the ASCII for 'p'.
Send a number within the bounds to see if the perceptron fires (1) or stays quiet (0).
- Bounds: [-10.0, 10.0]
- Output rule: w*x + b >= 0 -> 1, else 0.
- No back-to-back repeats of the same number.
- Goal: make the last 8 outputs read 01110000 (ASCII 'p').
- Command: RESET to clear the firing history.
Type HELP for a reminder or EXIT to quit.

[1/128] x> 0
```

### 2. Sending Probes

Start probing values around the range:
```
0
1
-1
2
-2
3
-3
```
Record the response for each input:
```
input    output
-----    ------
  0        0
  1        0
 -1        0
  2        1
 -2        0
 ...
```
Se we’ve found the boundary behavior: `The threshold is between 1 and 2.`

All `Negatives Produces 0` and all `Positives Produces 1` where there is an exception `1 Producing 0`

So try sending the probes in the following order:

* `1`
* `2`
* `3`
* `4`
* `-1`
* `-2`
* `-3`
* `-4`

The output would look like:
```
[1/128] x> 1
Perceptron stays quiet.
Recent outputs (1/8): 0
[2/128] x> 2
Perceptron fires!
Recent outputs (2/8): 01
[3/128] x> 3
Perceptron fires!
Recent outputs (3/8): 011
[4/128] x> 4
Perceptron fires!
Recent outputs (4/8): 0111
[5/128] x> -1
Perceptron stays quiet.
Recent outputs (5/8): 01110
[6/128] x> -2
Perceptron stays quiet.
Recent outputs (6/8): 011100
[7/128] x> -3
Perceptron stays quiet.
Recent outputs (7/8): 0111000
[8/128] x> -4
Perceptron stays quiet.
Recent outputs (8/8): 01110000
Pattern matched! ASCII 'p' unlocked. Here is your flag:
academy{n3ur0n_m3t_227803d1}
```

And the flag:
```
academy{n3ur0n_m3t_227803d1}
```

## Neuron Express 2D-0

### 1. Connect 
```bash
nc aureolin-pixie.cylabacademy.net <LAB-ID>
```
### 2. Retrieving the flag

The simplest exact perceptron is: `3x + 0y - 8 >= 0` 

Therefore:
```
weight_x = 3
weight_y = 0
bias = -8
```
Submit this At the prompt:
```bash
TEST 3 0 -8
```
We will get:
```
Perfect match! Here is your flag:
academy{n3ur0n_expr_2d_0159e40e}
```
Flag:
```
academy{n3ur0n_expr_2d_0159e40e}
```
# Neuron Express 0

### 1. Connect
```bash
nc xebec.cylabacademy.net <LAB-ID>
```
You should get something resembling:
```
Welcome to Neuron Express 0!
Probe the 1D perceptron, then submit its weight and bias.
Send an integer within the bounds to see if the perceptron fires (1) or stays quiet (0).
- Bounds: [-10, 10]
- Output rule: w*x + b >= 0 -> 1, else 0.
- Command: TEST w b to submit a weight and bias guess.
- The guess must match outputs for every integer x in range.
Type HELP for a reminder or EXIT to quit.
```
### 2. Retrieving the flag

The correct perceptron is: `w=1 b=-2`

So its rule is: `x-2>=0`

meaning:

* `x ≤ 1 → quiet (0)`
* `x ≥ 2 → fires (1)`
Submit this At the prompt:
```bash
TEST 1 -2
```
We will get:
```
Perfect match! Here is your flag:
academy{n3ur0n_expr_5fc0ce22}
```
Flag:
```
academy{n3ur0n_expr_5fc0ce22}
```