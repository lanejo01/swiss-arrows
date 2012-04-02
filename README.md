# swiss-arrows

A collection of arrow macros.

![trystero](http://upload.wikimedia.org/wikipedia/en/archive/a/a9/20060119154504!Trystero-small.png)

## Usage

```
[swiss-arrows "0.0.1"]
```

```
(use '[swiss-arrows.core])
```

*The Diamond Wand*

A generalized arrow.  Similar to -> or ->> except that the flow of execution is passed through explicitly specified positions in each of the forms.

```
(-<> 0
     (* <> 5)
     (vector 1 2 <> 3 4))
 => [1 2 0 3 4]
```

Unlike -> and ->>, the diamond wand also supports literals and quoted forms:

```
 ;; quoted list
 (-<> 10 '(1 2 a <> 4 5)) => '(1 2 a 10 4 5)
 
 ;; map
 (-<> 'foo {:a <> :b 'bar}) => {:a 'foo :b 'bar}

 ;; quoted map
 (-<> foo '{:a a :b <>}) => {:a 'a :b 'foo})
```

*The Back Arrow*

This is simply ->> with its arguments reversed, convenient in some cases.

```
 (<<-
  (let [x 'nonsense])
  (if-not x 'foo)
  (let [more 'blah] more)) => 'blah
```

The following six arrows (three, and their parallel counterparts) are *branching* arrows, in contrast with the "threading" or "nesting" arrows we have seen thus far.  The following example demonstrates how branching and nesting arrows can work together to cleanly express a flow of control. Here our first branching arrow, The Furcula, passes the result of (+ 1 2) to each of the successive forms, which is then nested out horizontally into further expressions using traditional arrows.

```
(-< (+ 1 2)
    (->> vector (repeat 3))
    (-> (* 2) list)
    (list 4))                =>    '[([3] [3] [3]) (6) (3 4)]
```


*The Furcula*

A branching arrow using the -> form placement convention. Expands to a let performing the initial operation, and then individual expressions using it. In the parallel version, the individual expressions are evaluated in futures.

```
(-< (+ 1 2) (list 2) (list 3) (list 4)) => '[(3 2) (3 3) (3 4)]

;; The Parallel Furcula

(-<:p (+ 1 2) (list 2) (list 3) (list 4)) => '[(3 2) (3 3) (3 4)]
```

*The Trystero Furcula*

Another branching arrow. Same idea as -<, except it uses the ->> form placement convention.

```
(-<< (+ 1 2) (list 2 1) (list 5 7) (list 9 4)) => '[(2 1 3) (5 7 3) (9 4 3)]

;; Parallel Trystero Furcula

(-<<:p (+ 1 2) (list 2 1) (list 5 7) (list 9 4)) => '[(2 1 3) (5 7 3) (9 4 3)]
```

*The Diamond Fishing Rod*

Another branching arrow. Same idea as -< and -<<, except it uses the -<> form placement convention.

```
(-<>< (+ 1 2) [<> 2 1] [5 <> 7] [9 4 <>]) => '[(3 2 1) (5 3 7) (9 4 3)]

;; Parallel Diamond Fishing Rod

(-<><:p (+ 1 2) [<> 2 1] [5 <> 7] [9 4 <>]) => '[(3 2 1) (5 3 7) (9 4 3)]
```

See https://github.com/rplevy/swiss-arrows/blob/master/test/swiss_arrows/test/core.clj for more examples.

## License

Credits:

Walter Tetzner, Stephen Compall, and I designed and implemented something similar to the "diamond wand" a couple of years ago.

Stephen Compall suggested the "back-arrow" in a conversation about a recently announced library, as a better solution (TODO: remember the name of that library).

Copyright (C) 2012 Robert P. Levy

Distributed under the Eclipse Public License, the same as Clojure.