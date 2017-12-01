---
title: 'Staircase - Solution'
currentMenu: 'articles'
---

## Gather Requirements/Clarify Problem
We are given an integer as input(n).

n represents the height, and width of our desired output.

Our desired output is a staircase drawn with the pound symbol (#). If a pound symbol is not represented it will be replaced by a space (" ").

So this means there will be n number of elements in each print statment, and there will be some combination of spaces and pounds.

Test case:
```nohighlight
input: n = 4
output:    #
          ##
         ###
        ####
```
        
## Break Down Complex Pieces into Smaller Pieces
I know we will have to print n number of new lines. I can do this with a for loop.

Within that for loop I can create a blank string, and concatenate either spaces or pound symbols, depending on where we are in the original loop.

I can then print the new string before the original loop iterates.

## Psuedocode

I will need a for loop, and within that loop I will need to nest 2 additional for loops. My plan is to create a blank string that will have spaces added to it first, then pound signs added second. The loops for spaces and pounds will depend on where we are in the original loop.

```nohighlight
for loop -> goes from 0 to n
    variable print_string = ''
    for loop -> adds the number of spaces to print_string
    for loop -> adds the appropriate number of pounds to print_string
    print(print_string)
```
    

## Code
Following the psuedocode above, this is what I have:

```python
for i in range(0,n):
    print_str = ""
    for j in range((n - i)):
        print_str += " "
    for k in range(i):
        print_str += "#"
    
    print(print_str)
```

## Testing
Let's test it out.

#### Iteration 1:

##### Variables (n = 4; i = 0; print_str = "")

```nohighlight
for j in range((4 - 0)):
    Iteration 1:
    print_str = " "

    Iteration 2:
    print_str = "  "

    Iteration 3:
    print_str = "   "

    Iteration 4:
    print_str = "    "

for k in range(0):
    # nothing happens

print(print_str)
```

##### Output:
```nohighlight
"    "
```

#### Iteration 2:

##### Variables (n = 4; i = 1; print_str = "")
```nohighlight
for j in range ((4 - 1))
    Iteration 1:
    print_str = " "
    
    Iteration 2:
    print_str = "  "

    Iteration 3:
    print_str = "   "

for k in range(1):
    Iteration 1:
    print_str = "   #"

print(print_str)

Output for 2nd Iteration:
"   #"
```

##### Output:
```nohighlight
"    "
"   #"
```

#### Iteration 3:

##### Variables (n = 4; i = 2; print_str = "")
```nohighlight
for j in range ((4 - 2))
    Iteration 1:
    print_str = " "
    
    Iteration 2:
    print_str = "  "

for k in range(2):
    Iteration 1:
    print_str = "  #"

    Iteration 2:
    print_str = "  ##"

print(print_str)

Output for 2nd Iteration:
"  ##"
```

##### Output:
```nohighlight
"    "
"   #"
"  ##"
```

#### Iteration 4:

##### Variables (n = 4; i = 3; print_str = "")
```nohighlight
for j in range ((4 - 3))
    Iteration 1:
    print_str = " "

for k in range(3):
    Iteration 1:
    print_str = " #"

    Iteration 2:
    print_str = " ##"

    Iteration 3:
    print_str = " ###"

print(print_str)

Output for 3rd Iteration:
" ###"
```

##### Output:
```nohighlight
"    "
"   #"
"  ##"
" ###"
```

At this point, the original for loop has completed, and this would be final output.

Our output doesn't quite match up with our expected output, it looks like we are off by one! A common error, let's make small changes and see how these change our output.

## Changes
A blank line is printed first along with the incorrect number of spaces and pounds because the first for loop is going from 0-3. We need it to go from 1-4 and it should be fine. So let's change the first for loop.

```python
for i in range(1,n+1):
    print_str = ""
    for j in range((n - i)):
        print_str += " "
    for k in range(i):
        print_str += "#"
    
    print(print_str)
```

## Testing take two...
Let's try this again!

#### Iteration 1:

##### Variables (n = 4; i = 1; print_str = "")

```nohighlight
for j in range((4 - 1)):
    Iteration 1:
    print_str = " "

    Iteration 2:
    print_str = "  "

    Iteration 3:
    print_str = "   "

for k in range(1):
    Iteration 1:
    print_str = "   #"

print(print_str)
```

##### Output:
```nohighlight
"   #"
```

#### Iteration 2:

##### Variables (n = 4; i = 2; print_str = "")
```nohighlight
for j in range ((4 - 2))
    Iteration 1:
    print_str = " "
    
    Iteration 2:
    print_str = "  "


for k in range(1):
    Iteration 1:
    print_str = "  #"

    Iteration 2:
    print_str = "  ##"

print(print_str)

Output for 2nd Iteration:
"  ##"
```

##### Output:
```nohighlight
"   #"
"  ##"
```

#### Iteration 3:

##### Variables (n = 4; i = 3; print_str = "")
```nohighlight
for j in range ((4 - 3))
    Iteration 1:
    print_str = " "

for k in range(2):
    Iteration 1:
    print_str = " #"

    Iteration 2:
    print_str = " ##"

    Iteration 3:
    print_str = " ###"

print(print_str)

Output for 2nd Iteration:
" ###"
```

##### Output:
```nohighlight
"   #"
"  ##"
" ###"
```

#### Iteration 4:

##### Variables (n = 4; i = 4; print_str = "")
```nohighlight
for j in range ((4 - 4))
    # the conditions are never met so nothing happens!

for k in range(3):
    Iteration 1:
    print_str = "#"

    Iteration 2:
    print_str = "##"

    Iteration 3:
    print_str = "###"

    Iteration 4:
    print_str = "####"

print(print_str)

Output for 3rd Iteration:
"####"
```

##### Output:
```nohighlight
"   #"
"  ##"
" ###"
"####"
```

Alright! We did it. Our actual output matches the desired output.

We successfully followed all of the steps and created a solution that solves the problem!