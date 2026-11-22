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

Write one to three sentences here. Name the rule that fixes each field.

## P02

Write one to three sentences here. Name the rule that fixes each field.

## P03

Write one to three sentences here. Name the rule that fixes each field.

## P04

Write one to three sentences here. Name the rule that fixes each field.

## P05

Write one to three sentences here. Name the rule that fixes each field.

## P06

Write one to three sentences here. Name the rule that fixes each field.

## P07

Write one to three sentences here. Name the rule that fixes each field.

## P08

Write one to three sentences here. Name the rule that fixes each field.

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

