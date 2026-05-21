# Welcome

Welcome to the website for COMP90077 Advanced Algorithms and Data
Structures!

# Shortcuts

- @sec-schedule
- @sec-a2 **Due: 30 May 17:00**
- Current week's notes: [Annotated Slides](./slides-post-w11.pdf)
- Current week's tutorial: TBD

# Subject Overview

Many real-world optimization problems such as scheduling and resource
allocation involve making decisions to optimize a function subject to
some set of constraints. These are called combinatorial optimization
problems. In this subject, we will learn general mathematical techniques
for designing and analyzing algorithms for combinatorial optimization
problems with a focus on provable guarantees.

The subject is divided roughly into 3 parts:

1.  Greedy algorithms are often the first type of algorithm that
    practitioners reach for due to their simplicity. In the first part
    of this subject, we will see that when the constraints form what is
    called a "matroid", then greedy will always produce the optimal
    solution. This explains why, for example, the greedy algorithm
    solves the classic minimum spanning tree problem. We will also see
    that when the function we are optimizing satisfies a property called
    "submodularity", the problems become NP-hard but greedy gives a good
    approximate solution. Roughly speaking, submodular functions have
    diminishing marginals and appear in many contexts such as machine
    learning.

2.  In the second part, we will learn a highly successful paradigm for
    designing approximation algorithms based on linear programming. At a
    high level, the basic idea is that even if it is NP-hard to find the
    optimal solution, we can still efficiently find the optimal
    "fractional" solution and then convert it into an actual solution
    without much loss. We will also learn how to use linear programming
    duality to design and analyze algorithms.

3.  In the last part, we will cover online algorithms, which make
    decisions over time without exact knowledge of the future. Online
    algorithms are needed in many settings. For example, in ride-sharing
    platforms such as Uber, the algorithm has to allocate drivers to
    passengers, as requests arrive over time.

(sec-schedule)=

# Schedule

| Weeks | Lecture | Tasks |
|----|----|----|
| W1: 2/3 - 6/3 | [Administrivia](./admin), [Background](./background), [Minimum Spanning Trees](./mst), [Slides](./slides-w1.pdf), [Annotated Slides](./slides-post-w1.pdf) | @sec-tut1 |
| W2: 9/3 - 13/3 | @sec-matroids, [Slides](./slides-w2.pdf), [Annotated Slides](./slides-post-w2.pdf), [Slides Errata](https://edstem.org/au/courses/34455/discussion/3169257) | @sec-tut2 |
| W3: 16/3 - 20/3 | @sec-approx, @sec-submodular-max, [Slides](./slides-w3.pdf), [Annotated Slides (corrected)](./slides-post-w3.pdf) | @sec-a1, @sec-tut3 |
| W4: 23/3 - 27/3 | @sec-cover, [Annotated Slides](./slides-post-w4.pdf) | @sec-tut4 |
| W5: 30/3 - 3/4 | Matching and Vertex Cover, [Annotated Slides](./slides-post-w5.pdf) | ****Assignment 1 deadline: Apr 1 17:00**** |
| Mid-semester break |  |  |
| W6: 13/4 - 17/4 | [Bipartite Matching](#sec-bipartite), [Linear Programming I](#sec-linear-programming-1), [Annotated Slides](./slides-post-w6.pdf) |  |
| W7: 20/4 - 24/4 | ****Lecture replaced by MST**** | ****Mid-sem test during lecture**** |
| W8: 27/4 - 1/5 | @sec-lp-rounding, [Annotated Slides](./slides-post-w8.pdf) | @sec-tut8 |
| W9: 4/5 - 8/5 | @sec-lp, @sec-lp-duality, [Annotated Slides](./slides-post-w9.pdf) | @sec-tut9 |
| W10: 11/5 - 15/5 | @sec-dual-fitting-setcover, @sec-primal-dual, @sec-pd-shortest-paths, @sec-pd-mst, [Annotated Slides](./slides-post-w10.pdf) | Assignment 2 released on May 14 |
| W11: 18/5 - 22/5 | Online Algorithms [Annotated Slides](./slides-post-w11.pdf) |  |
| W12: 25/5 - 29/5 | Online Algorithms, Wrap Up | ****Assignment 2 deadline: May 30 17:00**** |

# Resources

- Matroids
  - [Spanning trees and
    matroids](https://theory.stanford.edu/~jvondrak/CS369P/lec7.pdf),
    [Greedy algorithm for
    matroids](https://theory.stanford.edu/~jvondrak/CS369P/lec8.pdf)
    (scribe notes from Stanford CS 369P: Polyhedral techniques in
    combinatorial optimization)
  - [Notes on
    Matroids](https://jeffe.cs.illinois.edu/teaching/algorithms/notes/E-matroids.pdf)
    by Jeff Erickson (UIUC)
- [Lecture Notes on Approximation
  Algorithms](https://www.cs.dartmouth.edu/~deepc/appx-lecture-notes.htm)
  by Deeparnab Chakrabarty (Dartmouth)
- [CS 583: Approximation
  Algorithms](https://courses.grainger.illinois.edu/cs583/fa2021/approx-algorithms-lecture-notes.pdf)
  by Chandra Chekuri (UIUC)
- [The Design of Approximation
  Algorithms](https://www.designofapproxalgs.com/book.pdf) by David
  Williamson and David Shmoys (Cornell)
- [Approximation Algorithms](https://ics.uci.edu/~vazirani/book.pdf) by
  Vijay Vazirani
- [Understanding and Using Linear
  Programming](https://link.springer.com/book/10.1007/978-3-540-30717-4)
  by Jiří Matoušek, Bernd Gärtner
