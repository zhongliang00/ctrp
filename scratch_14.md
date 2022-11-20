# City travel refuel problem

## Lemma
The optimal route will not consist of more than 1 turn.

## Proof setup
Suppose the optimal route through 3 arbitrary cities `a`, `b` and `c` turns at `a` and `c`, we will show that this cannot be the most optimal route (i.e. there must be a more optimal route that takes <= 1 turn).

Without loss of generality, we can assume that the route exits at `c` and keeps going to the right. (The `a` case is just the mirror image of this).

## Definition of optimality
For a route to be optimal, it must be valid, and also be the cheapest route among other valid routes (i.e most fuel left at `c`).

## Some obvious primers
Inversion of inequalities
<pre>
a < b   <=>   -a > -b
</pre>
Addition of inequalities
<pre>
a     <  b
    c <=     d
a + c <  b + d  (equality gets consumed)
</pre>


## Case 1: Entering at `a` and leaving at `c`
It is obvious that if one can drive from `a` to `c`, there is no need to double back to `a` or even `b`.

## Case 2: Starting at `b` and leaving at `c`
If the most optimal route is `bcbabc`, then it must be a valid route, from which we have the following constraints:
<pre>
    b     >=        bc  # can reach c from b
    b + c >=  ab + 2bc  # can reach back a after going to c
a + b + c >= 2ab + 3bc  # can complete whole route
</pre>
where `ab` and `bc` are the distances between `a` and `b`, and `b` and `c` respectively.

Let us invert these in advance, we will need them later
<pre>
    -b     <=       -bc  # eq1
    -b  -c <= -ab  -2bc  # eq2
-a  -b  -c <=-2ab  -3bc  # eq3
</pre>

It must also be the cheapest route, which means that the other routes must be more expensive or invalid.

The other non-trivial routes are `abc`, `babc`, `cbabc`. These routes are obviously cheaper, so for `bcbabc` to be the most optimal route, they have to be invalid.

### Route `abc`
Let us consider `abc` first. For `abc` to be invalid, any of the following must be true:
<pre>
a         <   ab        # eq4  # cannot reach b from a
a + b     <   ab +  bc  # eq4b # cannot reach c after reaching b
</pre>
But note that eq4b is redundant, since eq4b + eq1 = eq4 anyway. So the only active constraint is eq4

### Route `babc`
<pre>
    b     <   ab        # eq5  # cannot reach a from b
a + b     <  2ab +  bc  # eq5b # cannot reach c after reaching a
</pre>
Either one of the two constraints must be true for `bcbabc` to be the most optimal route.

### Route `cbabc`
<pre>
        c <         bc  # eq6  # cannot reach c from b
    b + c <   ab +  bc  # eq6b # cannot reach a after reaching b
a + b + c <  2ab + 2bc  # eq6c # cannot reach c after reaching a
</pre>
Adding eq6b+eq2, and eq6c+eq3, we find that
<pre>
        0 <        -bc # eq6b + eq2
        0 <        -bc # eq6c + eq3
</pre>
Since both these cases are impossible, the inequalities eq6b and eq6c are not active. So the only active constraint is eq6.

### Case `eq5` is true
<pre>
    -b     <=       -bc  # eq1
    -b  -c <= -ab  -2bc  # eq2
-a  -b  -c <=-2ab  -3bc  # eq3

 a         <   ab        # eq4
     b     <   ab        # eq5
         c <         bc  # eq6

 a + b + c <  2ab +  bc  # eq7 = eq4 + eq5 + eq6
         0 <       -2bc  # eq8 = eq3 + eq7
</pre>
Clearly `eq8` is impossible, so either
 - at least one of the constraints must not be active (i.e. it corresponds to a valid route) or 
 - the proposed route `bcbabc` is not valid. 

Hence, `bcbabc` cannot be the optimal route if `eq5` is true

### Case `eq5b` is true
<pre>
    -b     <=       -bc  # eq1
    -b  -c <= -ab  -2bc  # eq2
-a  -b  -c <=-2ab  -3bc  # eq3

 a         <   ab        # eq4
 a + b     <  2ab +  bc  # eq5b
         c <         bc  # eq6

         0 <       -2bc # eq9 = eq3 + eq5b + eq6
</pre>
Clearly `eq9` is impossible as well

Hence, `bcbabc` cannot be the optimal route if `eq5b` is true

### Summary
Both ways result in the same conclusion that `bcbabc` cannot be the optimal route. Hence, the optimal route consists of at most 1 turn.

