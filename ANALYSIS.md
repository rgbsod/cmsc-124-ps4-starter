# The Search and the Stack

## ./trace P07
`== trace P07 ==
Call: grandparent(tom,X)
  Call: parent(tom,_)
  Exit: parent(tom,liz)
  Call: parent(liz,X)
  Fail: parent(liz,X)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
  Call: parent(bob,X)
  Exit: parent(bob,ann)
Exit: grandparent(tom,ann)
answer 1: X=ann
Redo: grandparent(tom,ann)
  Redo: parent(bob,ann)
  Exit: parent(bob,pat)
Exit: grandparent(tom,pat)
answer 2: X=pat
Redo: grandparent(tom,pat)
  Redo: parent(bob,pat)
  Fail: parent(bob,X)
  Redo: parent(tom,bob)
  Fail: parent(tom,_)
Fail: grandparent(tom,X)
== 2 answers ==`

- Which of its lines push a frame onto Activity 15's runtime stack? Which
  lines pop one?
  - Every `Call:` line pushes a new frame onto the runtime stack, storing that goal's variables, return address, and dynamic link to its caller. For Case `P07`, `Call: grandparent(tom,X)` pushes the outer frame, then `Call: parent(tom,_)` and `Call: parent(liz,X)` push  the child frames while `grandparent` stays suspended. On the other hand , `Fail:` lines pop a frame when a goal exhausts all clauses (`Fail: parent(liz,X)` pops back to `grandparent`), and a final `Exit:` pops a frame only when no untried clauses remain. Unlike Activity 15's return, an `Exit:` with remaining alternatives (such as `Exit: parent(tom,liz)`) suspends rather than destroying it so backtracking can revisit it.
- What does a `Redo` line resume that a returned frame cannot?
  - When a `Redo` line executes, it resumes a goal using a saved choice point. In Activity 15, returning from a procedure pops and discards its frame's locals, return address, and caller state forever. In Prolog, a choice point preserves the goal's clause pointer (which `parent/2` fact just matched and which untried facts remain below it), its variable environment, and the exact trail of variable bindings made since that choice point so Prolog can undo them (`liz` or `ann`) before trying the next clause.
- Why does swapping the two clauses of `reach/2` change whether the search
  ever answers, while swapping the two clauses of `ancestor/2` only changes
  the order of the answers?
  - In `reach/2`, the recursive clause puts the recursive call `reach(X, Z)` before reading `edge(Z, Y)`. When placed first, every `Call:` immediately pushes another `reach(a, Z)` frame with a new unbound variable before ever testing the base case `edge(X, Y)`, causing unbounded left recursion that never reaches an `Exit:` port. Putting the base clause first allows `edge(a, b)` to bind variables before recursing. In `ancestor/2`, however, the recursive clause reads `parent(A, P)` before calling `ancestor(P, D)`. Because `parent/2` has finitely many facts in the knowledge base, `parent(A, P)` bounds the recursion depth regardless of which clause comes first, so swapping clauses only changes whether shallow answers (like `Exit: parent(tom,liz)`) or deep descendants appear first.

