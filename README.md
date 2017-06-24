# Forked

**Forked is an unimplemented WIP language. This repository just contains the spec.**

 - **If you would like to contribute to this language, please fork (ba dum crash) and PR.**
 - **Do not implement this language. It is bound to change very frequently.**

Forked is an esoteric stack-based two-dimensional language with multiple IPs, based on [Triangular](//git.io/triangular) and written around the [fork command](#fork).

Forked starts with IP 0 at the first (upper left) command, travelling right. The IP wraps around in the current direction if it hits an edge.

Factoids:

 - There are about six different ways to do everything in Forked.
 - There are actually six different ways to enter loops.
 - There are six directionals and six I/O commands.
 - 666\. ha.
 - A program in the shape of a fork is a thing. \[citation-needed\]

# Commands

General commands:

    | no-op if not next to a fork
    - no-op if not next to a fork
    & exit

Directionals:

    v redirect down
    ^ redirect up
    > redirect right
    < redirect left
    \ bounce IP
        if hit from left, bounce down and vice versa
        if hit from right, bounce up and vice versa
    / bounce IP
        if hit from left, bounce up and vice versa
        if hit from right, bounce down and vice versa

I/O:

    $ read input as integer
    ~ read input as character (-1 if EOF)
    % print ToS as integer
    @ print ToS as character
    ? print ToS as integer and pop
    ! print ToS as character and pop

Stack:

    i increment ToS
    d decrement ToS
    + postfix ADD       pop values used, push result
    ' postfix SUBTRACT  pop values used, push result
    _ postfix DIVIDE    pop values used, push result
    * postfix MULTIPLY  pop values used, push result
    = postfix ISEQUAL   pop values used, push result
    m postfix MODULO    pop values used, push result
    l postfix LESSTHAN  pop values used, push result
    g postfix MORETHAN  pop values used, push result    (heh mlg)
    0 push 0 to stack. 1 pushes 1, 2 pushes 2, etc.
    A push 10 to stack. B pushes 11, ... F pushes 15.
    # duplicate ToS
    p pop ToS
    . pop the stack index stored in the ToS
    , pop the stack index stored in the ToS, then the ToS
    

Loops:

    { if entered (with `v^><|-`) from top or left, create jump point
        otherwise, unconditionally jump back to most recently created jump point
    } if entered (with `v^><|-`) from top or left, unconditionally jump back to the most recently created jump point
        otherwise, create jump point

Memory:

    P pop ToS into memory
    S stash ToS in memory
    U pull memory to stack
    O pop memory

IP:

    [ create new IP here, do not switch to it
    ] create new IP at start of program
    ( create new IP here, switch to it
    ) discard most recently created IP, switch to previous

# Fork

    : fork the program

The fork is the only conditional. It may be entered from any direction, and changes the direction of the IP according to the value on the top of the stack. **Jumps cannot be used to enter forks.**

    -:  enters the fork from the East

     :- enters the fork from the West

     |
     :  enters the fork from the North

     :  enters the fork from the South
     |

If the value on the top of the stack is truthy, the instruction pointer turns right; otherwise it turns left.

|Fork entered from...|New IP direction if truthy|New IP direction if falsy|
|-|-|-|
|West|North|South|
|East|South|North|
|North|East|West|
|South|West|East|

## Fork Examples

This doesn't do anything.

        v       direct down
        $       read input
        |       enter fork from top
    &---:--&    split, go left if truthy, right if falsy

Neither does this.

      v
      $    >-----&
      |    |
      >----:        enter fork from left, fork up if falsy, down if truthy
           |
    &------<

This runs infinitely in a very messy way but illustrates the language quite well.

      V
      |
      |
      |
    /-:-\
    | | |
    | | |
    | | |

(I like that one. The code is read as `v|||:-`, then `/|||/\|||\-:-` infinitely.)

# Examples (golfy)

Truth machine:

        v
        $
        |
    {%}-:-%&

Cat program:

    >-v
    @ ~
    | | 
    \-:-&

Reverse input:

    >-v
    | ~
    | |
    \-:-v
    &{!}<
