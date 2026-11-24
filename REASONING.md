<!--no-pdf-->
# Why Each Prediction Reads the Way It Does

Write one entry per case, before you run anything. Name the rule behind each
predicted field. One to three sentences is enough. The grader checks the
structure of this file only. It does not judge how good the reasoning is, and
it sets no word minimum.

For every case you predicted wrongly, add a short correction after you run
it: what you predicted, what the language printed, and which rule you
misread. Corrections belong in the correction commits, not the prediction
commit.

Commit this file together with `predictions.tsv`, before any case runs:

```bash
git add predictions.tsv REASONING.md
```

The example below uses a query that is not one of the 16 cases, so it gives
no answer away. `parent(pat, X)` reads `parent(pat, jim)` because the `parent`
facts are tried top to bottom and only that one has `pat` as its first
argument:

```text
## parent(pat, X)

The goal scans the parent facts in source order. Only parent(pat, jim) has
pat first, so X binds to jim. No second fact matches, so the first ; finds
nothing and the search ends after one answer.
```

## P01

The goal scans the parent facts in source order. `parent(tom, liz)` has tom first, so `X` first binds to liz. Backtracking `(;)` unifies with the second fact `parent(tom, bob)`, binding `X` to `bob`, and no further facts match `tom`, giving a count of 2.


## P02

Unification matches the query with `parent(tom, bob)` without binding with any variables, so first is true. Source-order finds no other matching facts, so second is none and count is 1.

## P03

Source-order scans every parent fact, and unification fails on every second argument because tom never appears as a child, thus false is printed `(first)`, and second is none as there are other no matching facts and count is 0 as nothing matches.

## P04

THe first goal unifies X with `f(Y, a)` and leaves `Y` unbound, and the second goal unifies `Y` with `b` to produce X`=f(b,a)`,`Y=b` (first). Because = is only unification, second is none and count is 1.

## P05

Since these two terms involve a variable and  an atom, these fit a functor `(f/2)`, so it would first bind X with a and Y with b simultaneously. Thus first has X=a since it is the first variable and Y=b as second, with count being 2 as there are two variables involved.


## P06

The query attempts to unify a and b, but since a is not the same atom as b, Prolog responds with false, and second is none as there are no matching facts and count would be 0.

## P07

Per the rules, the first goal expands the grandparent rules into `parent(tom, P)`, `parent(P, X)` and returns any binded X. grandparent(tom, X) matches with parent(tom, bob) and parent(bob, ann), so it binds X to ann, then the second goal backtracks and finds for another matching fact, where it matches with    parent(bob, pat), so it binds X to pat for the second. As two facts match, this gives a count of 2.


## P08

Because the base clause (`parent(A, D)`) comes first in `ancestor/2`, source-order search matches `parent(tom, liz)` for `X=liz` (first) and backtracks to `parent(tom, bob)` for `X=bob` (second). After exhausting the direct children, backtracking enters the recursive clause to find all four grandchildren and great-grandchildren, giving a total count of 6.

## P09

`ancestor_r` places the recursive clause first, so the rule keeps following the parent relationship deeper before checking the base clause. For tom, it follows tom → bob → ann → sue and backtracks only when it reaches a person with no children, meaning the deepest relationships are found first.

## P10

Pat is first: Prolog initially tries the first rule, parent(X, jim), and finds parent(pat, jim), so X = pat.
Tom is second. When Prolog backtracks, it tries the recursive rule and checks Tom's children; since Bob is an ancestor of Jim through Pat, Tom qualifies, so X = tom.
Bob is picked third and last since Prolog backtracks again and checks Bob's children; since Pat is Jim's parent, Bob also qualifies, so X = bob.

## P11

Basically the way the `reach` predicate works is that it loops recursively forever sicnce the recursion is before any edges are read. So the values are unbounded since it does not produce any edges. The count is unbounded since the recursion will never stop and it did not find any values.

## P12

This version of the `reach` predicate now produces a value, but because there is a cycle and we keep prompting `;` to find other values, it would recurse back to `(c, a)` and the cycle will continue once more. And the count is unbounded since there is infinite values produced if we continue.

## P13

Basically the `len` predicate recurses and every time it recurse it takes the first element off the list, and add 1 to M every time it recursion returns back up. So the returned value is N=3 since there are 3 elements inside the list and there is no second value since after N=3 it tries to backtrack but there are no other branches to follow.

## P14

Clause 1 (join([], L, L)) is tried first and unifies at once with X=[] and Y=[1,2], giving answer 1. Pressing ; triggers a Redo that moves to clause 2 (with H=1, R=[2]) and calls join(_,Y,[2]), whose base case gives T=[], so X=[1] and Y=[2] is answer 2. The base case fires once at each recursion depth (X=[], [1], [1,2]), and clause 2 can't match [H|R] against [], so the trace ends in Fail after 3 answers.

## P15

X > 2 runs first, and at that moment X is unbound. > is an arithmetic comparison that needs actual numbers on both sides, so Prolog throws an error

## P16

elem(X, [1,2,3]) is the functor, it hands X one candidate at a time (1, then 2, then 3) as we can see the recursion takes the first element of the 2nd argument. X > 2 is the filter it only passes or fails, and each failure sends Prolog back to elem for the next X. Only 3 passes since X=3 > 2, so X = 3 is printed

