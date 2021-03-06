# core.rrb-vector

An implementation of the confluently persistent vector data structure
introduced in Bagwell, Rompf, "RRB-Trees: Efficient Immutable
Vectors", EPFL-REPORT-169879, September, 2011.

RRB-Trees build upon Clojure's PersistentVectors, adding logarithmic
time concatenation and slicing.

The main API entry points are `clojure.core.rrb-vector/catvec`,
performing vector concatenation, and `clojure.core.rrb-vector/subvec`,
which produces a new vector containing the appropriate subrange of the
input vector (in contrast to `clojure.core/subvec`, which returns a
view on the input vector).

core.rrb-vector's vectors can store objects or unboxed primitives. The
implementation allows for seamless interoperability with
`clojure.lang.PersistentVector`, `clojure.core.Vec` (more commonly
known as gvec) and `clojure.lang.APersistentVector$SubVector`
instances: `clojure.core.rrb-vector/catvec` and
`clojure.core.rrb-vector/subvec` convert their inputs to
`clojure.core.rrb_vector.rrbt.Vector` instances whenever necessary
(this is a very fast constant time operation for PersistentVector and
gvec; for SubVector it is O(log n), where n is the size of the
underlying vector).

`clojure.core.rrb-vector` also exports its own versions of `vector`,
`vector-of` and `vec` which always produce
`clojure.core.rrb_vector.rrbt.Vector` instances. Note that `vector-of`
accepts `:object` as one of the possible type arguments, in addition
to keywords naming primitive types.

## Usage

    (require '[clojure.core.rrb-vector :as fv])

    ;; read the overview at the REPL
    (doc clojure.core.rrb-vector)

    ;; functions meant for public consumption
    (doc fv/subvec)
    (doc fv/catvec)
    (doc fv/vector)
    (doc fv/vector-of)
    (doc fv/vec)

    ;; apply catvec and subvec to regular Clojure vectors
    (fv/catvec (vec (range 1234)) (vec (range 8765)))
    (fv/subvec (vec (range 1024)) 123 456)

    ;; for peeking under the hood
    (require '[clojure.core.rrb-vector.debug :as dv])
    (dv/dbg-vec (fv/catvec (vec (range 1234)) (vec (range 8765))))

## Future development

core.rrb-vector is being developed as a Clojure Contrib project, see
the
[What is Clojure Contrib](http://dev.clojure.org/pages/viewpage.action?pageId=5767464)
page for details. Patches will only be accepted from developers who
have signed the Clojure Contributor Agreement.

Current TODO list:

 1. tests;

 2. benchmarks;

 3. performance: general perf tuning, efficient `catvec`
    implementation (to replace current seq-ops-based impl);

 4. transients;

 5. ClojureScript version.

## Clojure(Script) code reuse

core.rrb-vector's vectors support the same basic functionality regular
Clojure's vectors do (with the omissions listed above). Where
possible, this is achieved by reusing code from Clojure's gvec and
ClojureScript's PersistentVector implementations. The Clojure(Script)
source files containing the relevant code carry the following
copyright notice:

    Copyright (c) Rich Hickey. All rights reserved.
    The use and distribution terms for this software are covered by the
    Eclipse Public License 1.0 (http://opensource.org/licenses/eclipse-1.0.php)
    which can be found in the file epl-v10.html at the root of this distribution.
    By using this software in any fashion, you are agreeing to be bound by
      the terms of this license.
    You must not remove this notice, or any other, from this software.

## Licence

Copyright © 2012 Michał Marczyk, Rich Hickey

Distributed under the Eclipse Public License, the same as Clojure.
