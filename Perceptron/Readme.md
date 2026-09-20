# Perceptron - Series

## Train XOR

XOR has these four points. A single perceptron creates only one linear decision boundary, so it cannot classify all four correctly.

Try:
* `0.02`

Which gives you the status:
```
Status
Nice work. You reached 75% accuracy, which is the best target for XOR with a single perceptron.

academy{x0r_unl34rn4bl3_by_p3rc3ptr0ns_d11b363c}
Final accuracy: 75.0% · Target: 75%
```
And the flag:
```
academy{x0r_unl34rn4bl3_by_p3rc3ptr0ns_d11b363c}
```
## Train XNOR

This is the XNOR version of the previous XOR challenge. The strategy is the same, but the labels are reversed. A single perceptron still cannot represent XNOR because the positive points are on opposite corners.

Try:
* `0.02`

Which gives you the status:
```
Status
Nice work. You reached 75% accuracy, which is the best target for XNOR with a single perceptron.

academy{xn0r_unl34rn4bl3_by_p3rc3ptr0ns_a0ea474c}
Final accuracy: 75.0% · Target: 75%
```
And the flag:
```
academy{xn0r_unl34rn4bl3_by_p3rc3ptr0ns_a0ea474c}
```
## Perceptron Train Hole in Middle

This one follows the same pattern, but now there are 9 points: 8 positive points surrounding 1 negative center point.

Dataset concept

Likely arranged approximately like:
```
  +   +   +
  +   -   +
  +   +   +
```
where:

+ = positive
- = negative center

A single perceptron can only draw one straight line, so it cannot create a closed boundary around the center point.

🎯 Target

The challenge tells us the maximum is:

`8 / 9 = 88.9%`

So you need the final model to classify 8 points correctly and only miss the center (or equivalently, miss one point).

Learning-rate sweep

Again, the intended mechanism is the learning-rate slider. Try the rate:

* `0.20`

Which gives you the status:
```
Status
Nice work. You reached 88.9% accuracy, which is the best target for a single-line classifier on this ring-with-center dataset.

academy{h0l3_1n_m1ddl3_unl34rn4bl3_l1n34rly_c85a6043}
Final accuracy: 88.9% · Target: 88.9%
```
And the flag:
```
academy{h0l3_1n_m1ddl3_unl34rn4bl3_l1n34rly_c85a6043}
```

## Perceptron Train Classic 2 Alpha

This challenge is different from the previous three: you don't need to find one exact learning rate. You need to discover 5 different rates that each produce 100% accuracy.

Strategy: sweep + binary search

The allowed range is:

`0.02 → 20.0`

Because the dataset is linearly separable, there should be multiple "sweet spots."

Try the rates:

* `0.60`
* `0.70`
* `1.00`
* `2.00`
* `3.00`

Which gives you the status:
```
Status
Perfect! You found 5 successful learning rates. Flag unlocked.

academy{perceptron_classic_2_alpha_5rates_13e2875c}
Final accuracy: 100.0% · Successful learning rates: 5/5
```
And the flag:
```
academy{perceptron_classic_2_alpha_5rates_13e2875c}
```

## Perceptron Train Classic 0

What counts as success

After each run, wait for all 16 updates to finish.

You want:

Accuracy: 100%

Once you hit 100%, you've solved the challenge and the interface should reveal the flag.

Try:
* `0.02`

Which gives you the status:
```
Status
Perfect! You found a learning rate that reaches 100% accuracy in 16 steps.

academy{perceptron_classic_mode_02a3669e}
Final accuracy: 100.0%
```
And the flag:
```
academy{perceptron_classic_mode_02a3669e}
```

## Perceptron Train 3-Bit Parity

A perceptron can only create one plane in 3D, so it can't perfectly separate these two classes.

The target is therefore:

6 / 8 = 75%
Try these learning rates

Try value:

* `0.10`

Which gives you the status:
```
Status
Nice work. You reached 75% accuracy, the best target for 3-bit parity with a single perceptron.

academy{3b1t_p4r1ty_unl34rn4bl3_l1n34rly_5d1cd0f2}
Final accuracy: 75.0% · Target: 75.0%
```
And the flag:
```
academy{3b1t_p4r1ty_unl34rn4bl3_l1n34rly_5d1cd0f2}
```

## Perceptron Play Naught

This challenge is different from the previous training-slider challenges. Here you directly control the perceptron parameters over a network connection.

The key is to first retrieve the actual points.

1. Connect

Run:
```bash
nc aureolin-pixie.cylabacademy.net 60821
```
You will see something like:
```
Welcome to Perceptron Play Naught!

Tweak the weights of a single-layer perceptron and watch how the decision
boundary moves. This time, the x-values are borrowed from the 1D Charlie
puzzle and lifted into 2D with new y-values so they can be linearly
separated. Your goal is to correctly classify every labeled point.

Commands:
  SHOW                    redraw the ASCII graph and point table
  SET w1 w2 b             set the weights/bias directly (floats are fine)
  ADJUST dw1 dw2 db       add offsets to the current weights
  POINTS                  list the training points with their target labels
  CHECK                   verify every point; prints the flag when perfect
  RESET                   go back to the starting weights (all w1=1.0, w2=-1.0, b=0.0)
  HELP                    show this message again
  EXIT / QUIT             leave the playground

Legend in the graph:
  0  point labeled class 0 and currently classified correctly
  1  point labeled class 1 and currently classified correctly
  x  point that is misclassified right now
  /  approximate decision boundary (where w1·x + w2·y + b ≈ 0)

Start experimenting by typing SET or ADJUST, or just press CHECK once everything
looks good! The starting weights are w1 = 1, w2 = -1, b = 0 so a boundary is
visible immediately.

+4         |       /
+3         |     /  
+2       x x   /   1
+1         | /   1  
+0 - - - - / - - - -
-1 0     / x   x    
-2     /   |        
-3   /     |        
-4 /       |        
   -4-3-2-1+0+1+2+3+4

Current weights -> w1: 1, w2: -1, b: 0

  point    label  perceptron  activation
  ------   -----  ----------  ----------
  (-4,-1)     0        0        -3
  (-1,+2)     1        0        -3
  (+0,-1)     0        1        1
  (+0,+2)     1        0        -2
  (+2,-1)     0        1        3
  (+3,+1)     1        1        2
  (+4,+2)     1        1        2
```

Then enter:
```
POINTS
```
You should get something containing the 2D coordinates and their labels:
```
Labeled points (x, y, label):
  (-4, -1) -> 0
  (-1, +2) -> 1
  (+0, -1) -> 0
  (+0, +2) -> 1
  (+2, -1) -> 0
  (+3, +1) -> 1
  (+4, +2) -> 1

Points graph:
+4         |        
+3         |        
+2       1 1       1
+1         |     1  
+0 - - - - + - - - -
-1 0       0   0    
-2         |        
-3         |        
-4         |        
   -4-3-2-1+0+1+2+3+4
```

Perfect — the POINTS output gives us a very simple separator.

All class 0 points have y = -1:
```
(-4,-1)  0
( 0,-1)  0
( 2,-1)  0
```
All class 1 points have positive y:
```
(-1,+2)  1
( 0,+2)  1
(+3,+1)  1
(+4,+2)  1
```
So we don't even need the x-coordinate. Use:
`$$ w_1=0,\quad w_2=1,\quad b=0 $$`
The perceptron becomes:
`activation = 0*x + 1*y + 0 = y`

Therefore y > 0 → class 1, while y < 0 → class 0.

Enter this:
```
SET 0 1 0
```
```
+4         |        
+3         |        
+2       1 1       1
+1         |     1  
+0 / / / / / / / / /
-1 0       0   0    
-2         |        
-3         |        
-4         |        
   -4-3-2-1+0+1+2+3+4

Current weights -> w1: 0, w2: 1, b: 0

  point    label  perceptron  activation
  ------   -----  ----------  ----------
  (-4,-1)     0        0        -1
  (-1,+2)     1        1        2
  (+0,-1)     0        0        -1
  (+0,+2)     1        1        2
  (+2,-1)     0        0        -1
  (+3,+1)     1        1        1
  (+4,+2)     1        1        2
```
Then:
```
CHECK
```
This should classify all 7/7 points correctly and reveal the flag:
```
Perfect! All points are classified correctly.
academy{n4ught_bu7_53p4r4b13_cf9e7c6f}
```
And the flag:
```
academy{n4ught_bu7_53p4r4b13_cf9e7c6f}
```

## Perceptron Play Alpha

For Perceptron Play Alpha, we need the actual coordinates before choosing the weights.

Connect:
```bash
nc aureolin-pixie.cylabacademy.net 55712
```
You will see:
```
Hello, welcome to Perceptron Play!

Tweak the weights of a single-layer perceptron and watch how the decision
boundary moves. Your goal is to correctly classify every labeled point.

Commands:
  SHOW                    redraw the ASCII graph and point table
  SET w1 w2 b             set the weights/bias directly (floats are fine)
  ADJUST dw1 dw2 db       add offsets to the current weights
  POINTS                  list the training points with their target labels
  CHECK                   verify every point; prints the flag when perfect
  RESET                   go back to the starting weights (all w1=1.0, w2=-1.0, b=0.0)
  HELP                    show this message again
  EXIT / QUIT             leave the playground

Legend in the graph:
  0  point labeled class 0 and currently classified correctly
  1  point labeled class 1 and currently classified correctly
  x  point that is misclassified right now
  /  approximate decision boundary (where w1·x + w2·y + b ≈ 0)

Start experimenting by typing SET or ADJUST, or just press CHECK once everything
looks good! The starting weights are w1 = 1, w2 = -1, b = 0 so a boundary is
visible immediately.

+4         |       /
+3         | x   /  
+2         |   1    
+1         | /   1  
+0 - - - - / - - - -
-1       x |        
-2 0 0 /   |        
-3   /     |        
-4 /       |        
   -4-3-2-1+0+1+2+3+4

Current weights -> w1: 1, w2: -1, b: 0

  point    label  perceptron  activation
  ------   -----  ----------  ----------
  (-3,-2)     0        0        -1
  (-1,-1)     0        1        0
  (-4,-2)     0        0        -2
  (+3,+1)     1        1        2
  (+2,+2)     1        1        0
  (+1,+3)     1        0        -2
```

Then run:
```
POINTS
```
You will see:
```
Labeled points (x, y, label):
  (-3, -2) -> 0
  (-1, -1) -> 0
  (-4, -2) -> 0
  (+3, +1) -> 1
  (+2, +2) -> 1
  (+1, +3) -> 1

Points graph:
+4         |        
+3         | 1      
+2         |   1    
+1         |     1  
+0 - - - - + - - - -
-1       0 |        
-2 0 0     |        
-3         |        
-4         |        
   -4-3-2-1+0+1+2+3+4
```
You can separate these very cleanly with the line y = x/2 (equivalently, x - 2y = 0).

A convenient perceptron choice is:
```
SET 1 -2 0
```
Check the activations and you should see:
```
+4         |        
+3         | x      
+2         |   x / /
+1         |   / 1  
+0 - - - / / / - - -
-1     / x |        
-2 x x     |        
-3         |        
-4         |        
   -4-3-2-1+0+1+2+3+4

Current weights -> w1: 1, w2: -2, b: 0

  point    label  perceptron  activation
  ------   -----  ----------  ----------
  (-3,-2)     0        1        1
  (-1,-1)     0        1        1
  (-4,-2)     0        1        0
  (+3,+1)     1        1        1
  (+2,+2)     1        0        -2
  (+1,+3)     1        0        -5
```
That choice doesn't consistently put the classes on opposite sides, so let's use a simpler separator based on the actual geometry.

The class-0 points are around the lower-left, while class-1 points are upper-right. Try:
```
SET 1 1 0
```
This gives x + y:
```
+4 /       |        
+3   /     | 1      
+2     /   |   1    
+1       / |     1  
+0 - - - - / - - - -
-1       0 | /      
-2 0 0     |   /    
-3         |     /  
-4         |       /
   -4-3-2-1+0+1+2+3+4

Current weights -> w1: 1, w2: 1, b: 0

  point    label  perceptron  activation
  ------   -----  ----------  ----------
  (-3,-2)     0        0        -5
  (-1,-1)     0        0        -2
  (-4,-2)     0        0        -6
  (+3,+1)     1        1        4
  (+2,+2)     1        1        4
  (+1,+3)     1        1        4
```
Then run:
```
CHECK
```
You should get the flag:
```
Perfect! All points are classified correctly.
academy{11n34r1y_53p4r4813_9938a80d}
```
And the flag:
```
academy{11n34r1y_53p4r4813_9938a80d}
```

## Perceptron Play 1D Alpha

Connect first:
```bash
nc aureolin-pixie.cylabacademy.net 53454
```
You will see:
```
Welcome to Perceptron Play 1D!

Tweak the weight and bias of a single-layer perceptron on a 1-dimensional
number line. Your goal is to correctly classify every labeled point.

Commands:
  SHOW                redraw the number line and point table
  SET w b             set the weight/bias directly (floats are fine)
  ADJUST dw db        add offsets to the current weight and bias
  POINTS              list the training points with their target labels
  CHECK               verify every point; prints the flag when perfect
  RESET               go back to the starting params (w=1.0, b=0.0)
  HELP                show this message again
  EXIT / QUIT         leave the playground

Legend on the number line:
  0  point labeled class 0 and currently classified correctly
  1  point labeled class 1 and currently classified correctly
  x  point that is misclassified right now
  ^  decision boundary marker below the line (where w·x + b ≈ 0)
  |  origin marker when no point or boundary is present

Start experimenting by typing SET or ADJUST, or just press CHECK once
everything looks good! The starting parameters are w = 1, b = 0 so a
boundary is visible immediately.

Number line (predictions):
    -4-3-2-1+0+1+2+3+4
     0 . 0 . x . 1 1 1
             ^        

Current parameters -> w: 1, b: 0

  x    label  perceptron  activation
  --   -----  ----------  ----------
  -4      0        0        -4
  -2      0        0        -2
  +0      0        1        0
  +2      1        1        2
  +3      1        1        3
  +4      1        1        4
```
Then run:
```
POINTS
```
You will see:
```
Labeled points (x, label):
  (-4) -> 0
  (-2) -> 0
  (+0) -> 0
  (+2) -> 1
  (+3) -> 1
  (+4) -> 1

Points line:
    -4-3-2-1+0+1+2+3+4
     0 . 0 . 0 . 1 1 1
```
The points are perfectly separated at a threshold between 0 and 2.

Use:
```
SET 1 -1
```
You should see:
```
Number line (predictions):
    -4-3-2-1+0+1+2+3+4
     0 . 0 . 0 . 1 1 1
               ^      

Current parameters -> w: 1, b: -1

  x    label  perceptron  activation
  --   -----  ----------  ----------
  -4      0        0        -5
  -2      0        0        -3
  +0      0        0        -1
  +2      1        1        1
  +3      1        1        2
  +4      1        1        3
```
This gives the activation: `x - 1`

So:

* Class `0`: `-4, -2, 0` → negative
* Class `1`: `2, 3, 4` → positive

Then run:
```
CHECK
```
That should classify all 6 points correctly and reveal the flag:
```
Perfect! All points are classified correctly.
academy{0n3_d_thr35h0ld_f458f97b}
```
And the flag:
```
academy{0n3_d_thr35h0ld_f458f97b}
```