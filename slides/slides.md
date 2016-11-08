# CMPT231
## Lecture 9: ch16
### Greedy Algorithms

---
## Devotional

---
## Outline for today
+ Greedy algorithms
+ Activity selection
  + Optimal substructure
  + Greedy property
  + Greedy solution
+ List merging
+ Knapsack problem
  + 0-1 knapsack
+ Huffman coding

---
## Greedy algorithms
+ A special case of **dynamic programming**
  + At each **decision** point, choose **immediate** gains
+ Relies on **greedy choice** property:
  + **Locally** optimal choices &rArr; **global** optimum
+ **Not** all problems have greedy choice property

<div class="imgbox"><div><ul>
<li> <strong>Hybrid</strong> strategies use large <em>jumps</em> to get to right "hill"</li>
<li> Then use greedy <strong>hill-climbing</strong> to get to the top <li>
</ul></div><div>
![Saddle point between maxima, [Wikimedia](https://commons.wikimedia.org/wiki/File%3ASaddle_Point_between_maxima.svg)](static/img/Saddle_Point_between_maxima.svg)
</div></div>

---
## Problem-solving outline
+ Describe optimial **substructure** (e.g., via *recurrence*)
+ Convert to naive **recursive** solution
  + Could then convert to **dynamic programming**
+ Use **greedy choice** to simplify recurrence
  + So only **one** subproblem remains
  + Don't have to **iterate** through all subproblems
  + Need to **prove** greedy choice yields *global* optimum
+ Convert to **recursive** greedy solution
+ Convert to **iterative** (bottom-up) greedy solution

---
## Example: activity selection
+ **Activities** \`S = {a\_i}\_1^n\` each need **exclusive** use of shared resource
  + Each activity has **start**/finish times \`[s\_i, f\_i)\` <!-- ] -->
  + Input is **sorted** by finish time
+ Find **largest** subset of S with **non-overlapping** activities

|  i |  1 |  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 |
|----|----|----|----|----|----|----|----|----|----|
|  s |  1 |  2 |  4 |  1 |  5 |  8 |  9 | 11 | 13 |
|  f |  3 |  5 |  7 |  8 |  9 | 10 | 11 | 14 | 16 |

![Activity selection](static/img/activity_selection.png)

---
## ActSel: task substructure
+ Let \`S\_(ij) = {a\_k in S: f\_i <= s\_k < f\_k <= s\_j}\`:
  **start** after \`f\_i\` and **finish** before \`s\_j\`
+ Any activity in \`S\_(ij)\` will be **compatible** with:
  + Any activity that **finishes** no later than \`f\_i\`
  + Any activity that **starts** no earlier than \`s\_j\`
+ Let \`A\_(ij)\` be a **solution** for \`S\_(ij)\`:
  largest mutually-compatible subset of \`S\_(ij)\`
+ Choose an activity \`a\_k in A\_(ij)\` and **partition** \`A\_(ij)\` into
  + \`A\_(ik) = A\_(ij) nn S\_(ik)\`: those that finish **before** \`a\_k\` starts
  + \`A\_(kj) = A\_(ij) nn S\_(kj)\`: those that start **after** \`a\_k\` finishes

---
## ActSel: prove opt substr
+ **Claim**: \`A\_(ik) and A\_(jk)\` are **optimal** solutions for \`S\_(ik), S\_(kj)\`
+ **Proof** (for \`A\_(ik)\`): assume **not**:
  + Let \`A\_(ik)'\` be a **better** solution:
    has non-overlapping elts, and \`|A\_(ik)'| > |A\_(ik)|\`
  + Then \`A\_(ik)' uu {a\_k} uu A\_(kj)\` would be a solution for \`S\_(ij)\`
  + Its size is **larger** than \`A\_(ij) = A\_(ik) uu {a\_k} uu A\_(kj)\`
  + This **contradicts** the assumption that \`A\_(ij)\` was **optimal**
+ Hence our optimal **substructure** is:
  + **Split** on \`a\_k\`,
  + **Recurse** twice on \`S\_(ik)\` and \`S\_(kj)\`
  + **Iterate** over all such choices of \`a\_k\` and choose the **best** split

---
## ActSel: naive recursive
+ Let *c(i, j)* be the **size** of an optimal solution for \`S\_(ij)\`:
  + **Splitting** on \`a\_k\` yields *c(i, j)* = *c(i, k)* + 1 + *c(k, j)*
  + Which **choice** of \`a\_k\` is the best?  Try **all** of them
+ **Recurrence**:
  \` c(i,j) = { (0, if S\_(ij) = O/),
  (max\_(a\_k in S\_(ij)) (c(i,k) + 1 + c(k,j)), else) :}
+ **Dynamic programming** implementation:
  + Fill in 2D **table** for *c(i, j)*, bottom-up
  + Auxiliary table to store **solutions** \`A\_(ij)\`
+ But for this problem, we can do even **better**!

---
## Outline

---
## ActSel: greedy choice
+ Which **choice** of \`a\_k\` leaves as **much** as possible of the
  shared resource available for the other activities?
  + The one which **finishes** the earliest
  + Since input is **sorted** by finish time, just take the **first** activity
+ Let \`S\_i = {a\_k in S: f\_i <= s\_k}\`: those that **start** after \`a\_i\` finishes
+ **Simplified** recurrence: to find optimal subset of \`S\_i\`,
  + Choose the **first** activity in \`S\_i\`: call it \`a\_j\`
  + Recurse on the **remainder**: \`S\_j\`
  + Don't need to **iterate** over all choices of \`a\_k in S\_i\`
+ We need to **prove** this greedy choice is **optimal**

---
## ActSel: prove greedy
+ **Cut-and-paste** style proof:
+ Let \`A\_i\` be any **optimal solution** for \`S\_i\`
+ **Modify** \`A\_i\` to fit *greedy* strategy
  + Let \`a\_j\` have **earliest** finish time in \`S\_i\`
    + This would be the *greedy* choice
  + Let \`a\_k\` have **earliest** finish time in \`A\_i\`
  + **Swap** them: let \`A'\_i = A\_i - {a\_k} uu {a\_j}\`
+ Show the modified solution is just as **optimal**

---
## ActSel: recursive greedy

---
## ActSel: iterative greedy

---
## Greedy vs dynamic programming

---
## Optimising for greedy choice

---
## Outline

---
## List merging
+ Given: **lists** of various lengths, \`l\_1 < l\_2 < ... < l\_n\`
  + Want: **sequence** of lists to merge, minimising total *merge cost*
  + **Merging** two lists \`l\_i, l\_j\` costs \`l\_i+l\_j\` and produces a list of length \`l\_i+l\_j\`
+ e.g., L = `{ 3, 4, 5, 6 }`: merge in sequence `( 3 + 4 ) + ( 5 + 6 )`
  + merge 3+4: *cost* 7, lists = `{ 5, 6, 7 }`
  + merge 5+6: *cost* 11, lists = `{ 7, 11 }`
  + merge 7+11: *cost* 18, lists = `{ 18 }`
  + **Total cost**: 7+11+18 = *36*

---
## List merge: greedy strategy
+ Always select the two **shortest** lists to merge
  + Merging creates a **new** list, that is also **pushed** with other lists
  + **Iterate** on the new set of lists
+ &rArr; what **data structure** to use?
  + Hold a **set** of keys
  + Quickly return the **smallest** key
+ &rArr; **Complexity** of finding optimal schedule for *n* lists?

---
## List merge: notation
+ Represent a merge **schedule** using a binary **tree**:
  + Input lists are at **leaves**
    + Keys are **lengths** of lists
  + Keys for **internal** nodes are **sum** of children's keys
+ Total merge **cost** is sum of lengths of all interior nodes:
  + \`= sum\_(i=1)^n d\_i l\_i\`, where \`d\_i\` is **depth** of leaf *i* in tree
+ Note any leaf of **maximal** depth must have another leaf as its **sibling**
  + Otherwise its sibling would have a **child**, so would not be maximal depth
+ We will **prove** that the greedy strategy gives an optimal solution
  + Using a **cut-and-paste** style proof:
  + Pick an arbitrary **optimal** (not necessarily greedy) solution
  + **Modify** it to fit the greedy strategy, and
  + Demonstrate the modified solution is **no worse** than the original solution

---
## List merge: proof
+ Prove greedy solution optimal by **induction** on number *n* of lists:
+ Let *T* be the tree for **any** optimal solution

---
## Outline

---
## Knapsack problem

---
## 0-1 knapsack

---
## Outline

---
## Encoding

---
## Code trees

---
## Huffman coding

---
## Outline

---
## Optimal caching

---
## Farthest-in-future

---
## Outline

