# 0.2.33 - Released 22 February 2014

- add clojure.core/trampoline annotation
- Fix pretty printing of dotted vars in polymorphic
  binders

# 0.2.32 - Released 19 February 2014

- check-ns and friends support :profile keyword argument that uses
  Timbre to profile the current type checking run
- pretty printing a `Fn` type now always prints the `:filters`,
  unless they are equal to `{:then tt :else tt}`
- Fix CTYP-105
  - subtyping fix for HMap optional keys

# 0.2.31 - Released 14 February 2014

## Breaking Changes

### HMap optional keys

There is a subtle change that isn't likely to affect many. Essentially,
these types are no longer interchangeable:

```clojure
(HMap :optional {:foo Number})
!=
(U (HMap :mandatory {:foo Number}) (HMap :absent-keys #{:foo}))
```

This subtyping relationship now holds:

```clojure
(U (HMap :mandatory {:foo Number}) (HMap :absent-keys #{:foo}))
<:
(HMap :optional {:foo Number})
    
(HMap :optional {:foo Number})
<!:
(U (HMap :mandatory {:foo Number}) (HMap :absent-keys #{:foo}))
```

You should prefer the :optional syntax where possible.

## Changes

- Changed the representation of :optional keys on HMap 
  - previously expanded into a combination of :mandatory and :absent-keys
  - now is an explicit field
- Type errors show the name of the source file where possible, falls back
  on the current namespace, or NO_SOURCE_FILE if neither are available

# 0.2.30 - Released 9 February 2014

- Don't try and aggressively eliminate nested intersections and unions
- check-ns and friends now cleanly catch more type errors (like int-error
  and tc-error) as "delayed" errors instead of letting them propagate

# 0.2.29 - Released 7 February 2014

- Add support for recursive types in the constraint resolution algorithm

# 0.2.28 - Released 5 February 2014

- Don't unfold recursive types while compacting union types
  - avoids infinite unfolding in some cases

# 0.2.27 - Released 5 February 2014

- Properly handle variance checks for DataType's
- Add annotation for 
  - clojure.core.typed/*collect-on-eval*
  - clojure.core/ns-aliases
- Only support aget up to 6 arguments
  - This may change back in the future to any arguments

## Clojurescript-specific

- cljs.c.t/{check-ns,cf} are both macros to facilitate calling from CLJS REPL
- cljs.c.t/{check-ns*,cf*} are Clojure functions for checking of
  Clojurescript code from Clojure

# 0.2.26 - Released 28 January 2014

## Completely disable ClojureScript Integration

There is a conflict with the latest Clojurescript versions. Since
Clojurescript checking support is not working anyway, we temporarily
disable Clojurescript support.

This should enable Clojurescript 0.0-2138 and core.typed to work
together.

# 0.2.25 - Released 23 January 2014 (0.2.24 is broken)

## Turn off *collect-on-eval* by default

The type checker no longer loads by default when evaluating global
annotations. Bind clojure.core/*collect-on-eval* to true to re-enable
this feature.

# 0.2.23 - Released 22 January 2014

## Correctly munge Protocol methods

In cases where protocol methods are munged (eg. - turns into _)
core.typed now follows the munging correctly and picks the right annotations.
There is a new restriction that the set of munged method names must
not contain duplicates (already enforced by defprotocol).

# 0.2.22 - Released 21 January 2014

## BREAKING CHANGES

- Erase type arguments for:
  - clojure.lang.IMeta
  - clojure.lang.IReference
  - clojure.lang.AReference
- Less accurate type for:
  - `clojure.core/meta` and `clojure.core/with-meta`
    - see note on metadata support below
- `clojure.core.typed/Ref{1,2}` aliases now alias an instance
  of `clojure.lang.Ref`, not `clojure.lang.IRef`

## New Aliases

- `clojure.core.typed/Agent1`
- `clojure.core.typed/Agent2`
- `clojure.core.typed/Atom2`
- `clojure.core.typed/Future`
- `clojure.core.typed/Namespace`
- `clojure.core.typed/Promise`
- `clojure.core.typed/Ref2`
- `clojure.core.typed/Var2`

## New class annotations

- `clojure.lang.Agent`
- `clojure.lang.IBlockingDeref`
- `clojure.lang.Ref`

## New var annotations (and inlinings)

[List of unannotated clojure.core vars](https://github.com/clojure/core.typed/wiki/clojure.core-Annotations)

```
clojure.core/*1
clojure.core/*2
clojure.core/*3
clojure.core/aclone
clojure.core/add-watch
clojure.core/agent
clojure.core/*agent*
clojure.core/agent-error
clojure.core/agent-errors
clojure.core/aget
clojure.core/alength
clojure.core/*allow-unresolved-vars*
clojure.core/alter
clojure.core/alter-meta!
clojure.core/*assert*
clojure.core/associative?
clojure.core/assoc-in
clojure.core/bean
clojure.core/bit-and
clojure.core/bit-and-not
clojure.core/bit-clear
clojure.core/bit-flip
clojure.core/bit-not
clojure.core/bit-or
clojure.core/bit-set
clojure.core/bit-shift-left
clojure.core/bit-shift-right
clojure.core/bit-test
clojure.core/bit-xor
clojure.core/bound-fn*
clojure.core/cast
clojure.core/char-escape-string
clojure.core/char-name-string
clojure.core/clear-agent-errors
clojure.core/*clojure-version*
clojure.core/clojure-version
clojure.core/*command-line-args*
clojure.core/commute
clojure.core/comparator
clojure.core/compare-and-set!
clojure.core/compile
clojure.core/*compile-files*
clojure.core/*compile-path*
clojure.core/*compiler-options*
clojure.core/construct-proxy
clojure.core/create-ns
clojure.core/create-struct
clojure.core/cycle
clojure.core/*data-readers*
clojure.core/dec'
clojure.core/decimal?
clojure.core/*default-data-reader-fn*
clojure.core/deliver
clojure.core/denominator
clojure.core/destructure
clojure.core/distinct
clojure.core/distinct?
clojure.core/drop
clojure.core/drop-last
clojure.core/*e
clojure.core/error-handler
clojure.core/error-mode
clojure.core/*file*
clojure.core/find-ns
clojure.core/flatten
clojure.core/*flush-on-newline*
clojure.core/*fn-loader*
clojure.core/format
clojure.core/future?
clojure.core/future-call
clojure.core/future-cancel
clojure.core/future-cancelled?
clojure.core/future-done?
clojure.core/get-in
clojure.core/get-proxy-class
clojure.core/get-thread-bindings
clojure.core/get-validator
clojure.core/hash
clojure.core/hash-combine
clojure.core/identical?
clojure.core/ifn?
clojure.core/*in*
clojure.core/init-proxy
clojure.core/instance?
clojure.core/list?
clojure.core/list*
clojure.core/load-reader
clojure.core/macroexpand
clojure.core/macroexpand-1
clojure.core/*math-context*
clojure.core/max-key
clojure.core/methods
clojure.core/method-sig
clojure.core/min-key
clojure.core/mod
clojure.core/munge
clojure.core/namespace-munge
clojure.core/neg?
clojure.core/newline
clojure.core/ns-map
clojure.core/nthrest
clojure.core/pcalls
clojure.core/peek
clojure.core/pmap
clojure.core/pop
clojure.core/pos?
clojure.core/prefer-method
clojure.core/primitives-classnames
clojure.core/print-ctor
clojure.core/*print-dup*
clojure.core/printf
clojure.core/*print-length*
clojure.core/*print-level*
clojure.core/println-str
clojure.core/*print-meta*
clojure.core/*print-readably*
clojure.core/print-simple
clojure.core/print-str
clojure.core/promise
clojure.core/proxy-call-with-super
clojure.core/proxy-mappings
clojure.core/proxy-name
clojure.core/rationalize
clojure.core/read
clojure.core/*read-eval*
clojure.core/read-line
clojure.core/realized?
clojure.core/reduce-kv
clojure.core/release-pending-sends
clojure.core/remove-ns
clojure.core/remove-watch
clojure.core/reset-meta!
clojure.core/restart-agent
clojure.core/select-keys
clojure.core/send
clojure.core/send-off
clojure.core/send-via
clojure.core/set-agent-send-executor!
clojure.core/set-agent-send-off-executor!
clojure.core/set-error-handler!
clojure.core/set-error-mode!
clojure.core/set-validator!
clojure.core/shuffle
clojure.core/shutdown-agents
clojure.core/sort
clojure.core/*source-path*
clojure.core/special-symbol?
clojure.core/supers
clojure.core/take-last
clojure.core/take-nth
clojure.core/the-ns
clojure.core/unchecked-add
clojure.core/unchecked-add-int
clojure.core/unchecked-dec
clojure.core/unchecked-dec-int
clojure.core/unchecked-divide-int
clojure.core/unchecked-inc
clojure.core/unchecked-inc-int
clojure.core/*unchecked-math*
clojure.core/unchecked-multiply
clojure.core/unchecked-multiply-int
clojure.core/unchecked-negate
clojure.core/unchecked-negate-int
clojure.core/unchecked-remainder-int
clojure.core/unchecked-subtract
clojure.core/unchecked-subtract-int
clojure.core/unsigned-bit-shift-right
clojure.core/update-proxy
clojure.core/*use-context-classloader*
clojure.core/val
clojure.core/var-get
clojure.core/var-set
clojure.core/vary-meta
clojure.core/*verbose-defrecords*
```

# Var annotation changes

- `clojure.core/ns-name`
  - now accepts a symbol
- `clojure.core/some-fn`
  - more special cases for predicates
- `clojure.core/every-pred`
  - more special cases for predicates
- `clojure.core/deref`
  - supports Java futures
  - supports 3 arg IBlockingDeref usage
- `clojure.core/force`
  - returns `Any` if argument isn't a delay
- `clojure.core/symbol`
  - namespace can be `nil` in 2-arity usage
- `clojure.core/into`
  - added base case for `IPersistentCollection`s
  - conjing maps together is the same as `merge`
- `clojure.core/conj`
  - conjing maps together is the same as `merge`
- `clojure.core/find
  - first arg can be `nil`
  - supports `Associative`s instead of just `IPersistentMap`s
- `clojure.core/get
  - rearrange arities, semantics should be preserved
- `clojure.core/ref`
  - supports keyword arguments
- `clojure.core/all-ns`
  - return value is now nil, or a non-empty seq
  - previously a seq
- `clojure.core/compare`
  - fix first argument to be `Any`, previously `Comparable`

## Abandoning static metadata support

- Adds too much complexity to the type system
  - `meta` now returns `Any`
  - `with-meta` simply returns its first argument
    - this is probably sound, since a correct implementation of IObj
      always recreates the same instance, the only difference being metadata
      which we don't track at all statically
    - same with `vary-meta`
  - `reset-meta! just requires an IReference argument
    - ditto with `alter-meta!`
  - planning to improve runtime casting to make metadata somewhat useable

## Fix Map's Seqable ancestor

- This was changed in 0.2.20
  - reverting back to previous behaviour
  - See [CTYP-101](http://dev.clojure.org/jira/browse/CTYP-101)


# 0.2.21 - Released 30 December 2013

## BREAKING CHANGES

- (Duplicated from 0.2.20) Fix core.async annotations
  - c.c.a.i.p/Channel takes zero arguments (previously two)
  - c.c.a.i.c.ManyToManyChannel has contravariant/covariant
    arguments (previously vice-versa)

## Fixes

- Instantiate any abstract objects in a result type before using
  it to check a fn body
  - eg. Checking against this function type:
         [Any Any
          -> (HVec [(U nil Class) (U nil Class)]
                   :objects [{:path [Class], :id 0} {:path [Class], :id 1}])]))
        means we need to instantiate the HVec type to the actual argument
        names with open-Result.
    
        If the actual function method is (fn [a b] ...) we check against:
    
          (HVec [(U nil Class) (U nil Class)]
                 :objects [{:path [Class], :id a} {:path [Class], :id b}])
- Fix `check-ns` with keyword arguments (was broken in 0.2.20)
- Better error checking on badly formed `Rec` types
  - eg. (Rec [x] x), (Rec [x] (U x Number))
  - See `parse-forbidden-rec-test`
- Better error checking on `Value` types
  - See `parse-value-test`
- Protocol methods now implicitly adds polymorphic type parameters to the *vars*
  that represent the protocol methods
  - eg. 
```clojure
        (ann-protocol [[foo :variance :covariant]]
                      Pro
                      pmethod
                      (All [c]
                        [(Pro foo) -> c]))
```

        This generates the equivalent of: 
        `(ann pmethod (All [foo c] [(Pro foo) -> c]))`
- Improvements to subtyping between functions with complicated filters and objects

## Enhancements

- Multimethod dispatch environment is reset between calls to `check-ns`
  - Should avoid the type checker complaining if you are changing your
    defmulti definition in the same JVM session
- It is now a type error (rather than a warning) to pass incorrect keyword
  arguments to `HMap`.
- `ann-datatype` and `ann-record` take a keyword argument `:extends` to override
  polymorphic ancestors
  - if a `deftype` implements a protocol, the annotation must override the ancestor
    eg. 
```clojure
        (t/ann-protocol [[foo :variance :covariant]]
                        Foo

                        bar-
                        [(Foo foo) -> foo])

        (t/ann-datatype FooD [t :- t/Symbol]
                        :extends
                        [(Foo t/Symbol)])
        (deftype FooD [t]
          Foo
          (bar- [this] t))
```
- Add `clojure.core/ex-info` annotation
- First order calls to `clojure.core/swap!` are flattened to reuse special cases
  in the type checker
  - eg. `(swap! a assoc :a 1)` is rewritten to `(swap! a (fn> [d :- D] (assoc d :a 1)))`
- Global annotations are added to the type environment as they are evaluated
  - only effective outside of cf/check-ns calls
  - cf/check-ns semantics unaffected

## Dependency changes

- Migrate to `jvm.tools.analyzer` 0.6.0

# 0.2.20 - Released 27th November 2013

## BREAKING CHANGES

- All functions in base-env that return LazySeq have been changed to Seq
  - also includes for>

- core.async annotations
  - Channel takes zero parameters
  - ManyToManyChannel type parameters now have correct variance
    - contravariant and covariant instead of vice-versa

## Fixes

- Fix [CTYP-95](http://dev.clojure.org/jira/browse/CTYP-95) Wrong annotation for `reset!`
- Fix the `clojure.lang.Associative` ancestor for `APersistentMap` and `PersistentHashMap`
- Type errors in `ann-protocol` et al. now have line numbers
- Fix [CTYP-86](http://dev.clojure.org/jira/browse/CTYP-86) Fix `interleave` annotation
- Protocols type binders are scoped like TFn, with bounds being parsed with any type variables
  that appear above it in scope.

## Enhancements

- `check-ns` and `check-ns-info` now can take a collection of namespace/symbols to check.
  The namespaces will be checked in batch, without rechecking common dependencies.
  - around 2-5x faster for batches of interconnected namespaces
- Polymorphic errors print bounds more consistently
  - when errors have type variables, they include upper/lower bounds if they are anything
    other than Any/Nothing respectively.
    
    Previously a type variable `a` would print `Lower :< a :< Higher` in an error.

    This is a problem if Lower is itself type variable because it is hard to tell where
    the lower bound is. The same error now prints `a :> Lower :< Higher`
- add annotation for `clojure.core/partition-all`
- Various tweaks to type signatures changing Seqable return types to Seq

# 0.2.19 - Released 11th November 2013

## Fixes

- Fix [CTYP-94](http://dev.clojure.org/jira/browse/CTYP-94) Better return type for `derive`

## Misc

- Remove a post-condition that incorrectly blows up with 0.2.17-0.2.18

# 0.2.18 - Released 11th November 2013

## Notes

- Contains unintentional regression, fixed in 0.2.19

## Enhancements

- Better error reporing for HMap syntax

## Fixes

- Better occurrence typing updates for maps
- Add KwArgsSeq fold cases
- Fix copy/paste typo about lower bounds in docstrings

# 0.2.17 - Released 8th November 2013

## Notes

- Contains unintentional regression, fixed in 0.2.19

## Enhancements

- Propagate more type information to map literals
  - eg. vals are checked against [Number Number -> Number]
    `(cf {:a #(+ %1 %2)} (Map Any [Number Number -> Number]))`
  - currently Only works with Map/IPersistentMap
- [CTYP-92](http://dev.clojure.org/jira/browse/CTYP-92) - Add [defn>](http://clojure.github.io/core.typed/#clojure.core.typed/defn>>)
- Add [def>](http://clojure.github.io/core.typed/#clojure.core.typed/def>>)

```
(defn> add-two :- AnyInteger [a :- AnyInteger]
  (+ a 2))

(def> vname :- Long 1)
```

## Fixes

- Better internal error checking in subtyping
  - Found various bad arguments to subtype where it would previously
    return `false` silently

## Misc

- `check-form-info` does not implicitly wrap the checked form with `ann-form`
  when an expected type is provided. Doing so conflicts with the simple `collect`
  strategy of only looking under `do` forms for global annotations.

# 0.2.15-16

## Enhancements

- improvements to Assoc
  - keyword invoke understands how to use an Assoc target

## Fixes

- [CTYP-91](http://dev.clojure.org/jira/browse/CTYP-91) Correctly instatiate polymorphic Protocol instances
  - More work needed to make polymorphic protocols usable
- add promote/demote case for KwArgsSeq
  - Reported by [Allen Rohner](https://groups.google.com/forum/#!topic/clojure-core-typed/W9bndgskRtI)
- docstring for `ann-protocol` now has correct example usage

## Internal

- split type variable environments
  - tvar-env & tvar-bnds
- type variables are now always named with fresh names
  - original name can be retrieved via c.c.t.type-ctors/F-original-name

# 0.2.14 - Released 21 October 2013

## *Breaking* Changes

- clojure.lang.Var now takes 2 parameters
  - bivariant like Atom
  - for common usages see [c.c.t/Var1](http://clojure.github.io/core.typed/#clojure.core.typed/Var1)
- Change AReference/IReference ancestors in base-env to (IReference Any Any)
  - unlikely to affect anyone

## Enhancements

- add aliases
  - [c.c.t/Var1](http://clojure.github.io/core.typed/#clojure.core.typed/Var1)
  - [c.c.t/Keyword](http://clojure.github.io/core.typed/#clojure.core.typed/Keyword)
  - [c.c.t/Symbol](http://clojure.github.io/core.typed/#clojure.core.typed/Symbol)
  - [c.c.t/Ref1](http://clojure.github.io/core.typed/#clojure.core.typed/Ref1)
- add typed helper functions
  - [c.c.t/ref>](http://clojure.github.io/core.typed/#clojure.core.typed/ref>)
- add annotations
  - c.c/alter-var-root
- improve docstrings of Java interop annotators
- CLJS checker
  - add Symbol, Keyword
  - add value subtyping cases
- ns-resolve can take a symbol as first arg
- clarify var> in docstring
- add HVec constructor (unqualified syntax)
  - eg. (HVec [Number Number])
- [CTYP-89](http://dev.clojure.org/jira/browse/CTYP-89) Heterogeneous vectors now take rest & dotted rest args, similar to function types
  - dotted args WIP
  - eg. (cf [1 2 3 4] '[Number Number *])
- [CTYP-90](http://dev.clojure.org/jira/browse/CTYP-90) Assoc on the type level
  - eg. (Assoc '{} ':a Number) <: '{:a Number}
  - can abstract over arbitrary maps:

```clojure
(ann (All [[x :< (Map Any Any)]]
       [x -> (Assoc x ':a Number)]))
(defn f [m] (assoc m :a 1))

(ann-form (f {:b 2}) '{:a Number :b Number})
```

- [CTYP-73](http://dev.clojure.org/jira/browse/CTYP-73) Support `reduced`

## Fixes

- [CTYP-67](http://dev.clojure.org/jira/browse/CTYP-67) Fix `compare` inlining
- [CTYP-74](http://dev.clojure.org/jira/browse/CTYP-74) Better syntax errors
- [CTYP-78](http://dev.clojure.org/jira/browse/CTYP-78) Fix `finally` blocks incorrectly using propagated types
- [CTYP-79](http://dev.clojure.org/jira/browse/CTYP-79) assoc optional HMap keys
- [CTYP-82](http://dev.clojure.org/jira/browse/CTYP-82) Fix `alts!!` annotation
- [CTYP-84](http://dev.clojure.org/jira/browse/CTYP-84) Fix heterogeneous seq/list subtyping
- Fix first arity of map type
- [CTYP-85](http://dev.clojure.org/jira/browse/CTYP-85) - abstract-object failure

## Other
- core.cache used for caching

# 0.2.13 - Released 25 September 2013
- Syntax parsing errors have line numbers
- Add [var>](http://clojure.github.io/core.typed/#clojure.core.typed/var>)
- Re-enable type checking of the checker

# 0.2.12
- Contains defect with var> (fixed 0.2.13)

# 0.2.11 - Released 24 September 2013
- Fix [CTYP-56](http://dev.clojure.org/jira/browse/CTYP-56)
  Support 3-argument isa?
- Much better support for mutually recursive protocols/records

# 0.2.10 - Released 24 September 2013
- [CTYP-69](http://dev.clojure.org/jira/browse/CTYP-69)
  Good error for unannotated deftype
- New aliases
  - [Nilable](http://clojure.github.io/core.typed/#clojure.core.typed/Nilable)
  - [NilableNonEmptySeq](http://clojure.github.io/core.typed/#clojure.core.typed/NilableNonEmptySeq)
  - [Hierarchy](http://clojure.github.io/core.typed/#clojure.core.typed/Hierarchy)
- New annotations
  - bound?, thread-bound?, bases, make-hierarchy, 
    isa? ([CYP-56](http://dev.clojure.org/jira/browse/CTYP-56), 3-arg version will work in next release),
    rand-int
- Datatype methods support recur
- Partial fix for [CTYP-64](http://dev.clojure.org/jira/browse/CTYP-64)
  It is an explicit error to annotate a datatype as a record or vice-versa
- Providing a rest type or a multiple arity function intersection type to deftype methods throw
  a delayed error. Multiple arities throw a NYI message.

# 0.2.9 - Released 22 September 2013
- Partially fix [CTYP-64](http://dev.clojure.org/jira/browse/CTYP-64)
  - comprehensible error when annotating a defrecord with ann-datatype
- Fix [CTYP-65](http://dev.clojure.org/jira/browse/CTYP-65)
  - annotate protocols in single-level namespaces
- Upgrade to jvm.tools.analyzer 0.5.1
  - AST now has source path information

# 0.2.8 - Released 21 September 2013
- Some Clojurescript fixes/cleanup
- [CTYP-61](http://dev.clojure.org/jira/browse/CTYP-61)
- [CTYP-66](http://dev.clojure.org/jira/browse/CTYP-66)
  Big improvements to merge/assoc/dissoc/conj by cspencer

# 0.2.7 - Released 17 September 2013
- Fix regression: Load implementation on cf/check-form*
- Stable core.async dep in project.clj

# 0.2.6 - Released 15 September 2013

## BUG FIXES

- [CTYP-53](http://dev.clojure.org/jira/browse/CTYP-53)
  [CTYP-60](http://dev.clojure.org/jira/browse/CTYP-60)
  HMap bug fixes
- [CTYP-37](http://dev.clojure.org/jira/browse/CTYP-37)
  Better error for deftype
- [CTYP-40](http://dev.clojure.org/jira/browse/CTYP-40)
  Clojurescript fix

## DOCUMENTATION

- [CTYP-46](http://dev.clojure.org/jira/browse/CTYP-46)
  Document ann-record

# 0.2.5
- HMap: fix subtyping and preserve absent keys with type updates
- Add clojure.core.typed/NonEmptyLazySeq alias
- `map` knows non-empty arguments returns NonEmptyLazySeq
- add check-form-info and check-ns-info that returns a map of type
  checkingr results
- add check-form*, like cf but a function
- Fix CTYP-41 (note recursive cf and check-ns are disallowed)
- Fix CTYP-42 (class not found in particular situations)
- Fix CTYP-45 (better error for bare defprotocol)
- Fix CTYP-48
- Generate slim jar that excludes AOT files
  - add :classifier "slim" after version string

# 0.2.4
- Add atom>
- Made Clojurescript an optional dependency

# 0.2.3
- Handle expected types for def forms
- Add clojure.core.typed/statistics, clojure.core.typed/var-coverage

# 0.2.2
- Add AOT classes to jar
  - decreases load time from 20s to 5s
  - adds 10mb to jar
- Error and warning message improvements
  - CTYP-41

# 0.2.1
- Improve errors messages
  - add hint to annotate vars via cf
  - remove "missing line number" message
  - more descriptive error for recursive check-ns

# 0.2.0
- No changes

0.1.26
- CTYP-33 Vars as functions
- Lots of cleaning up
- [API Reference](http://clojure.github.io/core.typed/) now builds properly
  - type aliases have nice headers (thanks Tom Faulhaber!)
- Some slightly better error messages
  - will throw a delayed error instead of an internal error on unannotated
    defs and other things.
  - instance fields throws delayed errors
- CTYP-27 Support `clojure.core/future`
- CTYP-37 (Progress) Better errors in deftype.

- *BREAKING CHANGE*
  - clojure.lang.Var now takes a single invariant parameter, the type contained in the var

0.1.23-24
- Support extending protocols to nil
- Fix subtyping between protocols and datatypes/records
- Fix extending protocols to datatypes both in deftype's
  and extend.
- Can attach :nocheck and :ann metadata to def's
  instead of using clojure.core.typed/ann.
  Note :ann type must be quoted.
  - eg. 

```clojure
     (defn ^:nocheck ^{:ann '[Any -> Any]}
        my-fn [a] a)
```

- *BREAKING CHANGE* remove ann-pprotocol
  - use ann-protocol with vector as first argument
  - see doc for ann-protocol

- *BREAKING CHANGE* remove ann-pdatatype
  - use ann-datatype with vector as first argument
  - see doc for ann-datatype

- *BREAKING CHANGE* change ann-datatype syntax
  - see ann-datatype doc

- *BREAKING CHANGE* change ann-protocol syntax
  - see ann-protocol doc

0.1.22
- Fix HMap's new keyword options
  - Documented here https://github.com/clojure/core.typed/wiki/Types

0.1.21
- Add docstrings to core def-alias's
- DEPRECATION: use :no-check instead of :nocheck for ann.
  See ann docstring.
- Fix map destructuring support for records

0.1.20
- Fix core.async support

0.1.19
- Start supporting core.async
  - see examples/async for examples
  - clojure.core.typed.async is the companion namespace. Just require it
    and the annotations will be registered during type checking.
- Deprecate implicit optional keyword parameters
  - old syntax: [args & {opts} :mandatory {mand...} -> t]
  - new syntax (interchangable keyword args): [& :optional {opts} :mandatory {mand...} -> t]
- Add lots of useful aliases in clojure.core.typed namespace
  - Int, Num 
    - Same as AnyInteger & j.l.Number
  - (Coll x), (NonEmptyColl x) 
    - IPersistentCollection
  - (Vec x), (NonEmptyVec x)
    - IPersistentVector
  - (Map k v) 
    - IPersistentMap
  - (Set x), (SortedSet x) 
    - IPersistentMap w/ clojure.lang.Sorted
  - (Seqable x), (NonEmptySeqable x), (EmptySeqable x) 
    - c.l.Seqable
  - (Seq x), (NonEmptySeq x)
    - clojure.lang.ISeq
  - (EmptyCount NonEmptyCount)
    - (ExactCount 0) and (CountRange 1)
- Update base env with new aliases
- Fixes for Extends type
  - subtyping, cs gen
- defmethods now respect warn-on-unannotated-var (reported by Allen Rohner)

0.1.18
- Update to tools.namespace SNAPSHOT, avoids StackOverflow error
  with malformed ns forms
- check-ns now takes either a symbol or a namespace
- Fix unsoundness where we assume incorrectly that interfaces cannot
  be combined arbitrarily.
- Add Extends type, which records +ve and -ve information on interfaces implemented
- Don't check unreachable branches in a `do`. eg. (do (assert false) (+ 1 'a))
- Performance work
  - quick hashing for types, a la Typed Racket
- Fix set!
- Support every?
- Support (every? p? (keys m)), (every? p? (vals m))
- Add annotation: dorun
- Add correct Seqable ancestor to IPersistentCollection
- RClasses with same base combine in an intersection
  - (I (ISeq Number) (ISeq Long)) => (ISeq (I Number Long)) => (ISeq Long)
- Misc bug fixes
- RClass's can now have bounded tvars (syntax didn't exist before)
- clojure.core/filter can sometimes understand predicates that have negative information.
  - eg. identity: true iff argument is *not* nil/false
  - (filter identity coll) should work, may have to instantiate identity.
    - (filter (inst identity (U nil Number)) [1 nil])
      => (Seqable Number)

0.1.17
- Automatically infer typed namespaces
- Add `warn-on-unannotated-vars` to allow partially typed namespaces
- Add :absent-keys and :complete options to HMap
- Fix bugs with filter constructors

0.1.15
- Code successfully AOT compiles, some exclusions (see pom.xml)
- Error messages use abbreviated types and forms
  - can be customised via *verbose-{types,forms}*, see check-ns docstring
  - types are printed according to the interns, aliases and imports of
    the currently checked namespace.

0.1.14
- Add support for mandatory and optional function keyword parameters
  - `[& {:a Number} :mandatory {:b Number} -> Any]`  takes an optional :a parameter (Number)
    and a mandatory :b parameter.
  - It is a type error to provide parameters not explicitly declared
  - can also check the `[& {:keys [a b] :as opt}]` idiom
    - here opt is a complete HMap, ie. has no unknown keys
- HMaps can track known absent keys
- Collect and display multiple type errors
- Fix type resolve for classes when checking from other namespaces
- def-alias supports docstring + var metadata
  - `(def-alias Alias "This is an alias for Number" Number)`
- Subtyping with protocols takes `extenders` into account.
- `ann-protocol` can annotate protocols in other namespaces
- Change for>, loop>, doseq>, fn> syntax
  - old syntax still supported, but shows warning
  - old way: `(fn> [[a :- Number], [b :- Number]] ...)`
  - new way: `(fn> [a :- Number, b :- Number] ...)`
- Support letfn via letfn>
```clojure
   (letfn> [foo :- [Any -> Any]
             (foo [x] x)
             bar :- [Number -> Any]
             (bar [y] y)]
      ...)
```
- Support clojure.tools.cli/cli
- Add various annotations

DEPRECATED
- Old syntax deprecated for for>, loop>, doseq>, fn>
  - still works but emits a warning

0.1.13 - Released 9 April 2013
- Add :nocheck metadata for `ann` to avoid checking `def`s. (CTYP-25)
- Warn on missing defs

0.1.12 - Released 8 April 2013
BREAKING CHANGES
- Evaluting forms like `ann` and `ann-record` no longer rely on side effects
  while evaluting. Instead the side effects are performed during the first phase
  of type checking. In practice, this means REPL interactions with `ann` should
  be wrapped in `cf`, and all typed namespaces should declare their typed 
  dependencies with `typed-deps`.
- `check-ns` resets global type state before checking a namespace.

ENHANCEMENTS
- Add `clojure.core.typed/typed-deps` for declaring typed dependencies. (CTYP-7)
- Basic support for records. See `ann-record` and `ann-precord`. Can associate known keys with assoc. (Progress on CTYP-10)

FIXES
- Type resolution works correctly when checking namespaces other than the current one. (CTYP-19)

MISC
- Refactored files into separate namespaces (previously everything was in `clojure.core.typed`)
- Maintain the base type environment in vars for easy refreshing of global state. (See `c.c.t.base-env` and `c.c.t.init`)
- Type checking is split into two phases:
    1. Collect type annotations (`c.c.t.collect-phase`)
    2. Type Checking (`c.c.t.check`)

0.1.10
- Added: doseq>, for>, dotimes>. (CTYP-2)
- Recognise filters from macroexpansion of `and`

0.1.9 - Released 25 March 2013
- Move to contrib infrastructure
- Misc minor bugfixes
- Getting keyword value keys from map-like things is more flexible
- Eliminate reflection warnings in deps

0.1.8 - Released 17 March 2013
ENHANCEMENTS
- Better line numbers for checking `do`
- Distinguish between complete and partial hmap types
- Support static fields
- Java enums are non-nil
- Strings and CharSequences are (Seqable Character)
- `cf` can be inserted as a top level expression in a typed namespace

FIXES
- Fix bug when comparing a dotted function to a normal one
- Fix constraint generation/subtyping between heterogeneous sequence-like types
- Resolve Names in constraint generation
- check-ns cannot loop infinitely when placed in a typed namespace
- re-* functions too specific

MISC
- Move to core.contracts from Trammel
- Move to jvm.tools.analyzer from analyze
- Add hole-driven core.typed tutorial
- Java enums are non-nil

0.1.7 - Released 26 February 2013
- Support simple multimethods 
  - no multi-dispatch
- Support simple flow analysis
  - most sequential asserts recognised eg.
    ```clojure 
    (let [a (read-string "1")
          _ (assert (integer? a))]
      (+ 10 a))
    ```

0.1.6
- Ensure `Result` is not introduced when performing type inference on drest fn apps
- `tc-ignore` is more do-like. 
  
  Workaround for a quirk in the Clojure compiler where protocols only get generated in
  a top-level `do`.

```clojure
  (identity (do (def-protocol foo (bar [this]))
              bar)) ;; <-- bar cannot be resolved
  vs.
  (do (def-protocol foo (bar [this]))
    bar)  ;; <-- bar is resolvable
```

  (patch by Stephen Compall, issue #3)
- Fix typo in `into-array` logic
  (patch by Stephen Compall, issue #4)
- `into-array>` generalises Java types, does not need redundant type annotations. See User Documentation in wiki.
- Improve type of `clojure.core/class`.
  (class <non-nil>) is always a Class
  (class nil) is always a nil
- Move documentation to [wiki](https://github.com/frenchy64/typed-clojure/wiki).

0.1.5
- Better errors for Java methods and polymorphic function applications, borrow error messages from Typed Racket
- Change `ann-datatype`, `ann-protocol`, `ann-pprotocol` syntax to be flatter
  (ann-protocol pname
                method-name method-type ...)
  (ann-dataype dname
               [field-name :- field-type ...])
- Add `defprotocol>`

0.1.4
- Support Clojure 1.4.0+
- Better errors, print macro-expanded form from AST

0.1.3
  - Refactor typed.core into individual files
  - Add `method-type`
    - `(method-type 'java.io.File/getName)` prints the current Typed Clojure type for the getName method of File
  - Add types for some clojure.core coersion functions
  - Preliminary support for ClojureScript

0.1.2
  - Fix objects and filters being lost during polymorphic and dotted function applications
    - Add tests for (if (seq a) (first a) 0) filter example.
  - Can annotate datatypes outside current namespace
  - Improve type of `seq`, `next`, `conj`
  - tc-pr-env -> print-env
  - tc-pr-filters -> print-filterset
  - Alter APersistentMap
  - Check that local binding occurrences match with expected types
  - Heterogeneous maps are APersistentMap's instead of IPersistentMap's
  - Heterogeneous vectors are APersistentVector's instead of IPersistentVector's

0.1.1

- Ensure `ann-form` finally checks its expression is of the expected type
- Improve simplifying of intersections involving Java classes
