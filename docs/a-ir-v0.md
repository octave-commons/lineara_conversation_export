# A-IR v0

Created with the assistance of an AI.

## Goals

- Easy to interpret (reference semantics)
- Easy to lower to Wasm predictably
- Not impossible to decompile / pretty-print
- Supports provenance (stable IDs)

#ir #anf #compiler

## Representation

Use EDN.

- Program = modules
- Module = functions + constants + metadata
- Function body uses **ANF** (A-normal form): a vector of steps ending with `:ret`

### Module

```clojure
{:ir/version 0
 :module/id  "mod-01H..."
 :meta       {:source/doc "specs/foo.md"
              :build/id   "bld-01H..."}
 :consts     {::pi {:op :const :type :f64 :value 3.14159}}
 :fns        {"main" {:fn/id "fn-01H..."
                      :params ["argv"]
                      :body   [...]}}}
```

### Step (ANF)

```clojure
{:id "n-01H..."
 :let "x3"
 :expr {:op :call :fn "add" :args ["x1" "x2"]}}
```

### Return

```clojure
{:id "n-01H..." :ret "x3"}
```

## Expression set (v0)

### Values

```clojure
{:op :const :type :i64 :value 42}
{:op :var   :name "x1"}
```

### Primitives (pure)

```clojure
{:op :prim :prim :num/add :args ["a" "b"]}
{:op :prim :prim :cmp/eq  :args ["a" "b"]}
{:op :prim :prim :map/get :args ["m" "k"]}
```

### Call

```clojure
{:op :call :fn "foo" :args ["x1" "x2"]}
```

### If (value-producing)

```clojure
{:op :if
 :test "t1"
 :then [ ... {:ret "y"}]
 :else [ ... {:ret "y"}]}
```

### Effect (only side effects)

```clojure
{:op :effect
 :cap :fs
 :call :read-text
 :args ["path"]
 :ret-type :string}
```

### Loop/recur (optional v0)

```clojure
{:op :loop
 :label "L0"
 :params [["i" :i64] ["acc" :i64]]
 :init   ["i0" "acc0"]
 :body   [... {:op :recur :label "L0" :args ["i1" "acc1"]}]
 :ret    "accFinal"}
```

## ANF lowering rules

**Simple expressions** are: var, const, prim(var args), call(var args), effect(var args).

Everything else gets named.

### Nested calls example

Surface:

```clojure
(+ (f a) (g (h b)))
```

ANF:

```clojure
[{:let "t1" :expr {:op :call :fn "f" :args ["a"]}}
 {:let "t2" :expr {:op :call :fn "h" :args ["b"]}}
 {:let "t3" :expr {:op :call :fn "g" :args ["t2"]}}
 {:let "t4" :expr {:op :prim :prim :num/add :args ["t1" "t3"]}}
 {:ret "t4"}]
```

### If lowering sketch

- lower test to temp
- branch bodies are step lists that end in `:ret`
- outer `:let` binds the `:if` result

## Wasm lowering strategy (v0)

### Runtime model

Start with a **value-tagged runtime**:
- use `i64` as universal carrier for tagged values
- heap pointers also stored in `i64` (with tags)
- numbers/booleans/nil are immediates
- strings/maps/vectors are heap objects

### Function lowering

- each temp becomes a Wasm local
- each ANF step becomes:
  - `local.get` args
  - `call` prim/function/capability
  - `local.set` result

### Effects as imports

`{:op :effect :cap :fs :call :read-text ...}` lowers to an imported function like:

- `(import "cap.fs" "read_text" (func $cap_fs_read_text (param i64) (result i64)))`

### Reversibility metadata

Emit a mapping:
- `wasm func index + instruction offset -> ir-node-id`
via Wasm custom sections (or a sidecar map keyed by wasm hash).

## Pretty-print strategy (A-IR -> targets)

- Keep **intent tags** during lowering (oop/pipeline/state-machine/etc)
- Two-phase printing:
  1) structure recovery (ANF -> let blocks, loop shapes)
  2) naming pass (stable temp renaming, deterministic)

Prefer printing from **A-IR**, not from Wasm (Wasm only as cross-check).

#transpile #pretty-print

## Spec IR v0 (English -> structured)

The LLM proposes Spec IR; a deterministic compiler turns it into tests + skeleton IR.

```clojure
{:spec/version 0
 :spec/id "spec-01H..."
 :domain {:name "forum"
          :entities [{:name "user"
                      :fields [{:name "id" :type :uuid :pk true}
                               {:name "email" :type :string :unique true}
                               {:name "screen_name" :type :string}]
                      :invariants ["email is valid"
                                   "screen_name length <= 32"]}]}
 :behaviors
 [{:name "signup"
   :inputs  [{:name "email" :type :string}
             {:name "password" :type :string}]
   :outputs [{:name "user" :type [:ref "user"]}]
   :acceptance
   [{:given  {:db :empty}
     :when   {:call "signup" :args {:email "a@b.com" :password "pw"}}
     :then   [{:assert :ok}
              {:assert :db/contains {:entity "user" :where {:email "a@b.com"}}}]}]}]
 :constraints
 {:determinism true
  :effects     #{:fs :db :clock :rand}
  :runtime-targets #{:wasm :js}}}
```

#specs #verification
