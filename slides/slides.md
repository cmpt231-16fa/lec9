<!-- .slide: data-background-image="https://sermons.seanho.com/img/bg/unsplash-8MbdD0pHXGY-italy_mtn.jpg" -->
# CMPT231
## Lecture 9: ch16
### Greedy Algorithms

---
<!-- .slide: data-background-image="https://sermons.seanho.com/img/bg/unsplash-8MbdD0pHXGY-italy_mtn.jpg" -->
## 2 Peter 3:10-12 <span class="ref">(NASB)</span>
But the **day of the Lord** will come **like a thief**, in which <br/>
[...] the **earth** and its **works** will be burned up.

Since all these things are to be **destroyed** in this way, <br/>
**what sort** of people ought you to be <br/>
in **holy conduct** and **godliness**, <br/>
**looking** for and **hastening** the coming of the day of God

>>>
+ **Local** optimum vs **global** optimum:
  + Only **God** sees the big picture of time
  + We see a **local** patch
+ Bible tells us what is **optimal**:
  + **holy** conduct (*ἀναστροφαῖς*, turning again)
  + **godliness** (*εὐσεβείαις*, piety, reverence)
  + **looking** (welcome, hope+fear)
  + **hastening** (speed, via proclaiming gospel)

---
<!-- .slide: data-background-image="https://sermons.seanho.com/img/bg/unsplash-8MbdD0pHXGY-italy_mtn.jpg" -->
## Outline for today
+ Greedy algorithms
+ Activity selection problem
  + Optimal substructure
  + Proving greedy property
  + Greedy solution
+ List merging problem + proof
+ Knapsack problem: fractional and 0-1
+ Huffman coding
+ Optimal offline caching

---
## Greedy algorithms
+ A special case of **dynamic programming**
  + At each *decision* point, choose **immediate** gains
+ Relies on **greedy choice** property:
  + *Locally* optimal choices &rArr; *global* optimum
+ **Not** all problems have greedy choice property!

<div class="imgbox"><div><ul>
<li> <strong>Hybrid</strong> strategies use large <em>jumps</em> to get to right "hill"</li>
<li> Then use greedy <strong>hill-climbing</strong> to get to the top </li>
</ul></div><div>
![Saddle point between maxima, [Wikimedia](https://commons.wikimedia.org/wiki/File%3ASaddle_Point_between_maxima.svg)](static/img/Saddle_Point_between_maxima.svg)
</div></div>

---
## Problem-solving outline
+ Describe optimial **substructure** (e.g., via *recurrence*)
+ Convert to naive **recursive** solution
  + Could then convert to *dynamic programming*
+ Use **greedy choice** to simplify recurrence
  + So only *one* subproblem remains
  + Don't have to *iterate* through all subproblems
  + Need to **prove** greedy choice yields *global* optimum
+ Convert to **recursive** greedy solution
+ Convert to **iterative** (bottom-up) greedy solution

---
## Outline

---
## Example: activity selection
+ **Activities** \`S = {a\_i}\_1^n\` each require *exclusive* use of some shared resource
  + e.g., database *table*, communication *bus*, conference *room*
+ Each activity has **start**/finish times \`[s\_i, f\_i)\` <!-- ] -->
  + Input is **sorted** by finish time
+ Task: **maximise** num activities that can be completed
  + i.e., find **largest** subset of S with **non-overlapping** activities

|  i |  1 |  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 |
|----|----|----|----|----|----|----|----|----|----|
|  s |  1 |  2 |  4 |  1 |  5 |  8 |  9 | 11 | 13 |
|  f |  3 |  5 |  7 |  8 |  9 | 10 | 11 | 14 | 16 |

---
## ActSel: example
|  i |  1 |  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 |
|----|----|----|----|----|----|----|----|----|----|
|  s |  1 |  2 |  4 |  1 |  5 |  8 |  9 | 11 | 13 |
|  f |  3 |  5 |  7 |  8 |  9 | 10 | 11 | 14 | 16 |

![Activity selection](static/img/activity_selection.png)

---
## ActSel: task substructure
+ Let \`S\_(ij) = {a\_k in S: f\_i <= s\_k < f\_k <= s\_j}\`
  + all activities that **start** after \`f\_i\` and **finish** before \`s\_j\`
+ Any activity in \`S\_(ij)\` will be **compatible** with:
  + Any activity that **finishes** no later than \`f\_i\`
  + Any activity that **starts** no earlier than \`s\_j\`
+ Let \`A\_(ij)\` be a **solution** for \`S\_(ij)\`:
  + i.e., largest *mutually-compatible* subset of \`S\_(ij)\`
+ Choose an activity \`a\_k in A\_(ij)\` and **partition** \`A\_(ij)\` into
  + \`A\_(ik) = A\_(ij) nn S\_(ik)\`: those that finish **before** \`a\_k\` starts
  + \`A\_(kj) = A\_(ij) nn S\_(kj)\`: those that start **after** \`a\_k\` finishes

---
## ActSel: prove opt substr
+ **Claim**: \`A\_(ik) and A\_(jk)\` are **optimal** solutions for \`S\_(ik), S\_(kj)\`
+ **Proof** (for \`A\_(ik)\`): assume **not**:
  + Let \`B\_(ik)\` be a **better** solution:
  + So \`B\_(ik)\` has non-overlapping elts, and \`|B\_(ik)| > |A\_(ik)|\`
  + Then \`B\_(ik) uu {a\_k} uu A\_(kj)\` would be a **solution** for \`S\_(ij)\`
  + Its size is **larger** than \`A\_(ij) = A\_(ik) uu {a\_k} uu A\_(kj)\`
  + This **contradicts** the assumption that \`A\_(ij)\` was **optimal**
+ Hence our optimal **substructure** is:
  + **Split** on \`a\_k\`,
  + **Recurse** twice on the two subproblems \`S\_(ik)\` and \`S\_(kj)\`
  + **Iterate** over all such choices of \`a\_k\` and choose the **best** split

---
## ActSel: naive recursive
+ Let *c(i, j)* be the **size** of an optimal solution for \`S\_(ij)\`:
  + **Splitting** on \`a\_k\` yields *c(i, j)* = *c(i, k)* + 1 + *c(k, j)*
  + Which **choice** of \`a\_k\` is the best?  Try **all** of them
+ **Recurrence**:
  \` c(i,j) = { (0, if S\_(ij) = O/),
  (max\_(a\_k in S\_(ij)) (c(i,k) + 1 + c(k,j)), if S\_(ij) != O/) :} \`
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
    + This is the choice in the given **optimal** solution
  + **Swap** them: let \`B\_i = A\_i - {a\_k} uu {a\_j}\`
+ Show the modified solution \`B\_i\` is just as **optimal**:
  + **Size** \`|B\_i|\` is same as \`|A\_i|\`
  + Activities are **non-overlapping**: \`f\_j <= f\_k\`

---
## ActSel: recursive greedy
+ **Input**: arrays `s[], f[]`, sorted by `f[]`
  + Add a **dummy** entry `f[0]=0` so that \`S\_0 = S\`
+ For each **subproblem** \`S\_i\`:
  + **Skip** over any activities that overlap with \`a\_i\`
  + Choose the **first** activity \`a\_j\` that *doesn't* overlap
  + **Recurse** on the remainder
+ **Complexity**?

```
def ActivitySelection( s, f, i ):
  for j in i+1 .. length( f ):
    if ( s[ j ] >= f[ i ] ):                        # compat?
      return [ j ] + ActivitySelection( s, f, j )   # concat
  return NULL

ActivitySelection( s, f, 0 )
```

---
## ActSel: iterative greedy
+ Easy to convert **tail-recursion** to **iteration**
+ **Complexity**: &Theta;(*n*)
  + or &Theta;(*n lg n*) if need to **pre-sort** `f[]`

```
def ActivitySelection( s, f ):
  A = [ 1 ]
  i = 1
  for j in 2 .. length( f ):
    if ( s[ j ] >= f[ i ] ):    # compat w/ a_j ?
      A = A + [ j ]             # append to list
      i = j
  return A
```

|  i |    1   |    2   |    3   |    4   |    5   |    6   |    7   |    8   |    9   |
|----|--------|--------|--------|--------|--------|--------|--------|--------|--------|
|  s |    1   |    2   |   *4*  |    1   |    5   |   *8*  |    9   |  *11*  |   13   |
|  f |  **3** |    5   |  **7** |    8   |    9   | **10** |   11   | **14** |   16   |

---
## Greedy vs dynamic programming
+ *Greedy*-solvable problems are a **subset** <br/>
  of *dynamic programming*-solvable problems
  + **Not** all problems have *greedy property*
+ Dynamic programming fills in *table* **bottom-up**
  + Greedy *choice* is done **top-down**
+ Dyn prog **choice** requires solutions to **all** subproblems
  + Greedy choice can be made **before** doing the (*single*) subproblem
+ To **prove** greedy property:
  + *Assume* an optimal solution
  + *Modify* it to include the greedy choice
  + *Show* that the modified solution is still optimal

---
## Optimising for greedy choice
+ Often, *greedy choice* is easier if input is **pre-processed**
+ E.g., **sorting** activities by *finish* time
  + Then the greedy **choice** can be made in *O(1)* each time
  + The **pre-sorting** takes *O(n lg n)*
+ If input is **dynamically** generated:
  + Can't **sort** whole list in advance, but we can
  + Use **priority queue** to pop the current most optimal choice

---
## Outline

---
## List merging
+ Given: **lists** of various lengths, \`l\_1 < l\_2 < ... < l\_n\`
  + Want: **sequence** of lists to merge, minimising total *merge cost*
+ **Merging** two lists \`l\_i, l\_j\` *costs* \`l\_i+l\_j\`
  + and *creates* a new list of length \`l\_i+l\_j\`
+ e.g., L = `{ 3, 4, 5, 6 }`:
  + merge schedule `( 3 + 4 ) + ( 5 + 6 )`:
  + merge 3+4: *cost* 7, lists = `{ 5, 6, 7 }`
  + merge 5+6: *cost* 11, lists = `{ 7, 11 }`
  + merge 7+11: *cost* 18, lists = `{ 18 }`
  + **Total cost**: 7+11+18 = *36*

>>>
TODO: svg figure?

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
+ Total merge **cost** is sum of all interior nodes:
  + \`= sum\_(i=1)^n d\_i l\_i\`, where \`d\_i\` is **depth** of leaf *i* in tree
+ Note: any leaf of **maximal** depth must have a **sibling**
  + Sibling must also be a **leaf**
  + Otherwise would not be **maximal** depth

---
## List merge: proof outline
+ **Prove** that greedy strategy gives an optimal solution:
  + Use **induction** on number *n* of lists
+ For inductive step, use a **cut-and-paste** style proof:
  + Pick an arbitrary **optimal** (not necessarily greedy) solution
  + **Modify** it to fit the greedy strategy, and
  + Show the modified solution is **no worse** than original solution
  + Apply **inductive hypothesis** on set of *n-1* lists
  + Thus greedy on *n* lists is **no worse** than the modified solution
  + Which is **no worse** than the original, optimal solution
+ This will demonstrate that greedy is **optimal**

---
## List merge: proof
+ Let *T* be the tree for **any** optimal solution
  + Let *u* and *v* be sibling **lists** of maximal depth, \`d\_(max)\` (wlog, *u* &le; *v*)
+ Consider two **smallest** lists, \`l\_1, l\_2\`, of depth \`d\_1, d\_2\`
  + **Greedy** strategy would put these two at *maximal depth*
+ **Swap** *u* &harr; \`l\_1\`, and *v* &harr; \`l\_1\`: call the modified tree *T'*
  + *First* merge in *T'* is same as **greedy**
  + *Remaining* merges in *T'* no better than greedy, by **inductive hyp**
+ How does this affect the **total merge cost**? <br/>
  cost(*T'*) - cost(*T*) =
  \`(l\_1-l\_u)(d\_(max)-d\_1) + (l\_2-l\_v)(d\_(max)-d\_2)\` &le; 0
  + since \`l\_1 <= l\_u\` and \`l\_2 <= l\_v\`,
    and \`d\_(max) >= max(d\_1, d\_2)\`
+ So the greedy *T'* is just as **optimal** as *T*

---
## Outline

---
## Knapsack problem
+ **Fractional** knapsack problem:
  + *n* items, each with **weight** \`w\_i\` and **value** \`v\_i\`
  + Maximise total **value**, subject to total **weight** limit *W*
  + Can take **fractions** of an item (think *liquids*)
+ **Greedy** solution: sort items by *value-to-weight* ratio
  + Greedy **choice**: take item of largest \`v\_i/w\_i\`
+ **Final** spot may be filled with a *fractional* item

```
def FractionalKnapsack( v, w, W ):
  while totWeight < W:
    add next item of largest value-to-weight
  replace last item with 1 - (totWeight - W) of itself
```

---
## 0-1 knapsack
+ Knapsack problem, but **no** fractional items allowed
+ Greedy strategy **no longer** works!
  + *Locally*-optimal choices made early on <br/>
    **lock** us out of later *globally*-optimal choices
+ Still can solve with **dynamic programming** *(#16.2-2)*

![Knapsack problems](static/img/Fig-16-2.svg)

---
## Outline

---
## Encoding
+ Given a **text** with known *character set*
  + **Encode** each character with a unique *codeword* in binary
+ **Fixed-length** code: all codewords *same* length
  + "`cafe`" &rArr; *010 000 101 100*
+ **Variable-length** code: some codes use *fewer* bits
  + "`cafe`" &rArr; *100 0 1100 1101*
  + **Compression**: more *frequent* chars get *shorter* codes
+ **Prefix** code: no code is a *prefix* of another
  + Makes **parsing** unique: don't need *delimiters*
  + "`cafe`" &rArr; *100011001101*

![Character encodings](static/img/Fig-16-3.svg)

---
## Binary code trees
+ **Prefixes** are *nodes*, **characters** are at *leaves*
  + Requires encoding to be a *prefix* code
+ **Decoding** strings = a series of *walks* down the tree
  + **Cost** of a character *c* = its *depth* \`d\_c\` in the tree
  + **Fixed-length** code &rArr; all *leaves* at **same level**
+ **Total cost** of encoding a *text* using a given *tree*:
  \` sum\_c ( f\_c d\_c )\`
  + where \`f\_c\` is the **frequency** of character *c* in the text

![Code tree](static/img/Fig-16-4.svg)

---
## Huffman coding
+ Build tree **bottom-up**:
  + Start with the two **least**-common characters
  + **Merge** them to make a new *subtree* with **combined** freq
+ Use **min-priority queue** to manage the greedy choice

```
def huffman( chars ):
  Q = new MinQueue( chars )
  for i in 1 .. length( chars ) - 1:
    z = new Node()
    z.left = Q.popmin()
    z.right = Q.popmin()
    z.freq = z.left.freq + z.right.freq
    Q.push( z )
  return Q.popmin()
```

| char |  a |  b |  c |  d |  e |  f |
|------|----|----|----|----|----|----|
| freq | 15 |  5 |  9 |  7 | 18 | 10 |

---
## Outline

---
## Caching
+ Cache has **capacity** to store *k* items
+ Input is a sequence of *m* **item requests** \`{d\_i}\_1^m\`
+ Cache **hit** if item already in cache when requested
+ If cache **miss**, need to **evict** an item from cache
  + Then bring requested item into cache
+ **Task**: eviction schedule to **minimise** evictions
+ E.g., k=*2*, intial cache = `ab`, requests: `a b c b c a b`
  + **Optimal** schedule has only 2 evictions:

|  reqst |   a   |   b   |   c   |   b   |   c   |   a   |   b   |
|--------|-------|-------|-------|-------|-------|-------|-------|
| cache1 |   a   |   a   | **c** |   c   |   c   | **a** |   a   |
| cache2 |   b   |   b   |   b   |   b   |   b   |   b   |   b   |

<div class="caption">
This section thanks to Kevin Wayne, for the textbook Kleinberg + Tardos, "Algorithm Design"
</div>

---
## Offline caching
+ **Offline** caching: entire request sequence known in advance
+ Various **greedy** algorithms:
+ **LIFO** / **FIFO**: evict *most* (*least*) recently added item
+ **LRU**: evict item whose most *recent* access is the earliest
+ **LFU**: evict item least *frequently* accessed

|  reqst |   a   |   d   |   a   |   b   |   c   |   e   |   g   |
|--------|-------|-------|-------|-------|-------|-------|-------|
| cache1 | **a** |   a   |   a   |   a   |   a   |   a   |   ?   |
| cache2 |   w   |   w   |   w   | **b** |   b   |   b   |   ?   |
| cache3 |   x   |   x   |   x   |   x   | **c** |   c   |   ?   |
| cache4 |   y   | **d** |   d   |   d   |   d   |   d   |   ?   |
| cache5 |   z   |   z   |   z   |   z   |   z   | **e** |   ?   |

**Which** to evict on cache miss for *g*?

---
## Farthest-in-future
+ Since this is **offline**, we can **peek** into the future
+ **Farthest-in-future** algorithm ("clairvoyant"):
  + Evict item whose *next* request is the **farthest** in the future
+ Can be **proven** (Bélády 1966) to be an **optimal** offline caching strategy
  + Proof is also a *cut-and-paste* greedy proof
+ Useful perspective to enable **online** algorithms
  + **LRU** is farthest-in-future with time run *backwards*!
  + **LIFO** can be arbitrarily *bad*

---
## Outline

---
<!-- .slide: data-background-image="https://sermons.seanho.com/img/bg/unsplash-8MbdD0pHXGY-italy_mtn.jpg" class="empty" -->
