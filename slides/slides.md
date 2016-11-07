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


---
## ActSel: optimal substructure

---
## ActSel: prove opt substr

---
## ActSel: naive recursive

---
## Outline

---
## ActSel: greedy choice

---
## ActSel: prove greedy

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
## List Merging

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

