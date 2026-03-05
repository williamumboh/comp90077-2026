# Welcome

Welcome to the website for COMP90077 Advanced Algorithms and Data
Structures!

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

| Weeks | Lecture | Assessment |
|----|----|----|
| W1: 2/3 - 6/3 | [Administrivia](./admin), [Background](./background), [Minimum Spanning Trees](./mst), [Slides](./slides-w1.pdf) |  |
| W2: 9/3 - 13/3 | Matroids | ****Mon tut rescheduled TBD due to public holiday**** |
| W3: 16/3 - 20/3 | Greedy Approximation Algorithms | Assignment 1 released on Mar 16 |
| W4: 23/3 - 27/3 | Submodular Functions | ****Assignment 1 deadline: Mar 29 17:00**** |
| W5: 30/3 - 3/4 | Linear Programming I |  |
| Mid-semester break |  |  |
| W6: 13/4 - 17/4 | Linear Programming II |  |
| W7: 20/4 - 24/4 | ****Lecture replaced by MST**** | ****Mid-sem test during lecture**** |
| W8: 4/5 - 8/5 | Linear Programming III |  |
| W9: 11/5 - 15/5 | Online Algorithms I | Assignment 2 released on May 11 |
| W11: 18/5 - 22/5 | Online Algorithms II | ****Assignment 2 deadline: May 24 17:00**** |
| W12: 25/5 - 29/5 | Online Algorithms III, Wrap Up |  |

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
