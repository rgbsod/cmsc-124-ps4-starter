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

Write one to three sentences here. Name the rule that fixes each field.

## P10

Write one to three sentences here. Name the rule that fixes each field.

## P11

Write one to three sentences here. Name the rule that fixes each field.

## P12

Write one to three sentences here. Name the rule that fixes each field.

## P13

Write one to three sentences here. Name the rule that fixes each field.

## P14

Write one to three sentences here. Name the rule that fixes each field.

## P15

Write one to three sentences here. Name the rule that fixes each field.

## P16

Write one to three sentences here. Name the rule that fixes each field.

