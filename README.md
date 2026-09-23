<div align="center">

# Roblox Luau Reference

**Lesser known Luau features, syntax and patterns, with working examples.**

The language, the type system, the standard library, metatables, buffers, network
code, the Roblox API, engine lifecycle, replication and performance.<br>
Everything in one place.

![Luau](https://img.shields.io/badge/language-Luau-00A2FF?style=flat-square&labelColor=1F2328)
![Studio 733](https://img.shields.io/badge/verified_on-Roblox_Studio_733-E2231A?style=flat-square&labelColor=1F2328)
![New type solver](https://img.shields.io/badge/type_solver-new-7D5BED?style=flat-square&labelColor=1F2328)
![August 2026](https://img.shields.io/badge/updated-August_2026-57606A?style=flat-square&labelColor=1F2328)

---

## Jump straight to it

**Language**

[![Bindings](https://img.shields.io/badge/01-Bindings-0B84FF?style=for-the-badge&labelColor=1F2328)](#1-bindings-const-and-local)
[![Syntax](https://img.shields.io/badge/02-Syntax-0B84FF?style=for-the-badge&labelColor=1F2328)](#2-syntax)
[![Type system](https://img.shields.io/badge/03-Type_system-0B84FF?style=for-the-badge&labelColor=1F2328)](#3-type-system)
[![Directives](https://img.shields.io/badge/04-Directives-0B84FF?style=for-the-badge&labelColor=1F2328)](#4-directives-and-attributes)

**Library and internals**

[![Standard library](https://img.shields.io/badge/05-Standard_library-2DA44E?style=for-the-badge&labelColor=1F2328)](#5-standard-library)
[![Tables](https://img.shields.io/badge/06-Tables-2DA44E?style=for-the-badge&labelColor=1F2328)](#6-tables-from-the-inside)
[![Metatables](https://img.shields.io/badge/07-Metatables-2DA44E?style=for-the-badge&labelColor=1F2328)](#7-metatables)
[![Closures](https://img.shields.io/badge/08-Closures-2DA44E?style=for-the-badge&labelColor=1F2328)](#8-closures-and-coroutines)
[![Errors](https://img.shields.io/badge/09-Errors-2DA44E?style=for-the-badge&labelColor=1F2328)](#9-error-handling)

**Craft**

[![Patterns](https://img.shields.io/badge/10-Patterns-BF8700?style=for-the-badge&labelColor=1F2328)](#10-patterns)
[![Performance](https://img.shields.io/badge/11-Performance-BF8700?style=for-the-badge&labelColor=1F2328)](#11-performance)

**Roblox**

[![Networking](https://img.shields.io/badge/12-Networking-E2231A?style=for-the-badge&labelColor=1F2328)](#12-buffers-and-network-code)
[![Roblox API](https://img.shields.io/badge/13-Roblox_API-E2231A?style=for-the-badge&labelColor=1F2328)](#13-roblox-api)
[![Working practice](https://img.shields.io/badge/14-Working_practice-E2231A?style=for-the-badge&labelColor=1F2328)](#14-working-practice)

**Reference**

[![Quick reference](https://img.shields.io/badge/15-Quick_reference-57606A?style=for-the-badge&labelColor=1F2328)](#15-quick-reference)
[![Sources](https://img.shields.io/badge/16-Sources-57606A?style=for-the-badge&labelColor=1F2328)](#16-sources)

</div>

---

## Does it work in the live game?

Every subsection opens with one line telling you whether the feature works in a
published experience that someone plays in the Roblox app, and where it runs.
Anything still new or in beta is marked as such, and anywhere Roblox differs from
standard Luau that is called out explicitly.

| Badge | Meaning |
|---|---|
| ✅ `server + client` | Works everywhere in the live game |
| 🖥️ `server only` | The client does not have it, or is not allowed to use it |
| 🧩 `type-check only` | Compiled away, no runtime effect |
| ⚠️ *anything else* | It works, but with a catch that is spelled out on the line |
| 🚫 `not shipped` | Studio, tooling or CI, it does not ship with the game |
| 📐 `pattern, not a feature` | A way of working, nothing engine specific |

> [!NOTE]
> A bare ✅ `server + client` with no note after it means plain Luau, running in
> the VM that ships with the Roblox app. Wherever Studio behaves differently from
> the real client, that is stated explicitly. That gap is the source of
> *"but it worked in Studio"* bugs.

---

## Contents

| # | Section | What is in it |
|:--:|---|---|
| **1** | **[Bindings: const and local](#1-bindings-const-and-local)** | `const` next to `local`, and what `table.freeze` adds on top |
| **2** | **[Syntax](#2-syntax)** | Compound assignment, `continue`, interpolation, varargs, tail calls |
| **3** | **[Type system](#3-type-system)** | Singletons, tagged unions, generics, type packs, branding, `never` |
| **4** | **[Directives and attributes](#4-directives-and-attributes)** | `--!strict`, `--!native`, `@deprecated`, parameterized attributes |
| **5** | **[Standard library](#5-standard-library)** | table, string, patterns, `utf8`, `bit32`, math, debug, clocks, `vector` |
| **6** | **[Tables from the inside](#6-tables-from-the-inside)** | Array part vs hash part, `#` as a border, swap-remove |
| **7** | **[Metatables](#7-metatables)** | Every metamethod, `__iter`, weak tables, callable objects |
| **8** | **[Closures and coroutines](#8-closures-and-coroutines)** | Closure caching, the upvalue trap, generators |
| **9** | **[Error handling](#9-error-handling)** | Error levels, error objects, `xpcall` with a traceback |
| **10** | **[Patterns](#10-patterns)** | OOP with inferred types, signals, Trove, pooling, memoization |
| **11** | **[Performance](#11-performance)** | Hot paths, preallocation, benchmarking, `--!native`, Luau limits |
| **12** | **[Buffers and network code](#12-buffers-and-network-code)** | Writer/Reader, quantization, batching, delta, server-side safety |
| **13** | **[Roblox API](#13-roblox-api)** | `task`, streaming, DataStore, actors, replication, lifecycle |
| **14** | **[Working practice](#14-working-practice)** | Script headers, config modules, tooling, testing, CI |
| **15** | **[Quick reference](#15-quick-reference)** | Syntax cheat sheet and the mistakes that keep coming back |
| **16** | **[Sources](#16-sources)** | Luau docs, Creator Docs, tooling repos |

<details>
<summary><b>Full index — every subsection, click to jump</b></summary>

<br>

**1. Bindings: const and local**

- [1.1 const bindings](#11-const-bindings)

**2. Syntax**

- [2.1 Compound assignment](#21-compound-assignment)
- [2.2 `continue`](#22-continue)
- [2.3 String interpolation (backticks)](#23-string-interpolation-backticks)
- [2.4 If-then-else as an expression](#24-if-then-else-as-an-expression)
- [2.5 Generalized iteration](#25-generalized-iteration)
- [2.6 Number literals](#26-number-literals)
- [2.7 Floor division](#27-floor-division)
- [2.8 String escapes](#28-string-escapes)
- [2.9 Type casts with `::`](#29-type-casts-with-)
- [2.10 The `select` function](#210-the-select-function)
- [2.11 Using varargs correctly](#211-using-varargs-correctly)
- [2.12 Multiple returns get truncated](#212-multiple-returns-get-truncated)
- [2.13 Checking for an empty table](#213-checking-for-an-empty-table)
- [2.14 Number precision, important for UserIds](#214-number-precision-important-for-userids)
- [2.15 `tonumber` with a base](#215-tonumber-with-a-base)
- [2.16 Tail calls do not exist in Luau](#216-tail-calls-do-not-exist-in-luau)

**3. Type system**

- [3.1 Basic annotations](#31-basic-annotations)
- [3.2 Generics](#32-generics)
- [3.3 Function types](#33-function-types)
- [3.4 Type aliases and export](#34-type-aliases-and-export)
- [3.5 Singleton types (literal types)](#35-singleton-types-literal-types)
- [3.6 Tagged unions and narrowing](#36-tagged-unions-and-narrowing)
- [3.7 Type refinements](#37-type-refinements)
- [3.8 Type packs (variadic generics)](#38-type-packs-variadic-generics)
- [3.9 Read-only and write-only properties](#39-read-only-and-write-only-properties)
- [3.10 Intersection types](#310-intersection-types)
- [3.11 Function overloads via intersection types](#311-function-overloads-via-intersection-types)
- [3.12 Recursive and mutually recursive types](#312-recursive-and-mutually-recursive-types)
- [3.13 `never` for exhaustiveness checking](#313-never-for-exhaustiveness-checking)
- [3.14 Branded types do not hold up in Luau](#314-branded-types-do-not-hold-up-in-luau)
- [3.15 `typeof()` in type context](#315-typeof-in-type-context)
- [3.16 User-defined type functions](#316-user-defined-type-functions)

**4. Directives and attributes**

- [4.1 Script directives](#41-script-directives)
- [4.2 Parameterized attributes](#42-parameterized-attributes)

**5. Standard library**

- [5.1 Table functions](#51-table-functions)
- [5.2 String functions](#52-string-functions)
- [5.3 `%b` matches balanced pairs](#53-b-matches-balanced-pairs)
- [5.4 `%f` frontier pattern](#54-f-frontier-pattern)
- [5.5 Position captures](#55-position-captures)
- [5.6 `gsub` with a table or function](#56-gsub-with-a-table-or-function)
- [5.7 Escaping user input in patterns](#57-escaping-user-input-in-patterns)
- [5.8 The `utf8` library](#58-the-utf8-library)
- [5.9 Bit32](#59-bit32)
- [5.10 Math](#510-math)
- [5.11 Raw access, skipping metamethods](#511-raw-access-skipping-metamethods)
- [5.12 The debug library](#512-the-debug-library)
- [5.13 Picking the right clock](#513-picking-the-right-clock)
- [5.14 `os.date` with tables](#514-osdate-with-tables)
- [5.15 `DateTime` in Roblox](#515-datetime-in-roblox)
- [5.16 The `vector` library](#516-the-vector-library)

**6. Tables from the inside**

- [6.1 Array part and hash part](#61-array-part-and-hash-part)
- [6.2 `#` is a border, not a length](#62--is-a-border-not-a-length)
- [6.3 NaN as a key crashes](#63-nan-as-a-key-crashes)
- [6.4 `table.clear` versus `= {}`](#64-tableclear-versus--)
- [6.5 Swap-remove for unordered lists](#65-swap-remove-for-unordered-lists)

**7. Metatables**

- [7.1 All metamethods](#71-all-metamethods)
- [7.2 `__iter`, Luau specific and heavily underused](#72-__iter-luau-specific-and-heavily-underused)
- [7.3 Weak tables for caching](#73-weak-tables-for-caching)
- [7.4 `__newindex` for runtime read-only](#74-__newindex-for-runtime-read-only)
- [7.5 `__call` for callable objects](#75-__call-for-callable-objects)

**8. Closures and coroutines**

- [8.1 Closure caching](#81-closure-caching)
- [8.2 Closures and the upvalue trap](#82-closures-and-the-upvalue-trap)
- [8.3 Coroutines for state machines and generators](#83-coroutines-for-state-machines-and-generators)

**9. Error handling**

- [9.1 Error levels in `error()`](#91-error-levels-in-error)
- [9.2 `assert` returns its arguments](#92-assert-returns-its-arguments)
- [9.3 Error objects instead of strings](#93-error-objects-instead-of-strings)
- [9.4 `xpcall` with a traceback](#94-xpcall-with-a-traceback)

**10. Patterns**

- [10.1 OOP class with full type inference](#101-oop-class-with-full-type-inference)
- [10.2 Inheritance](#102-inheritance)
- [10.3 Enum pattern](#103-enum-pattern)
- [10.4 Signal / event emitter](#104-signal--event-emitter)
- [10.5 Cleanup pattern (Trove / Maid)](#105-cleanup-pattern-trove--maid)
- [10.6 Reusing objects (pooling)](#106-reusing-objects-pooling)
- [10.7 Memoization](#107-memoization)
- [10.8 Symbols as unique keys](#108-symbols-as-unique-keys)

**11. Performance**

- [11.1 Localizing globals does not help in Luau](#111-localizing-globals-does-not-help-in-luau)
- [11.2 Preallocate tables](#112-preallocate-tables)
- [11.3 `#t` in a loop condition is fine](#113-t-in-a-loop-condition-is-fine)
- [11.4 `getfenv`/`setfenv` destroy your performance](#114-getfenvsetfenv-destroy-your-performance)
- [11.5 Benchmarking](#115-benchmarking)
- [11.6 When `--!native` pays off and when it does not](#116-when---native-pays-off-and-when-it-does-not)
- [11.7 `_G` and `shared`](#117-_g-and-shared)
- [11.8 Measuring memory](#118-measuring-memory)
- [11.9 Luau limits](#119-luau-limits)

**12. Buffers and network code**

- [12.1 The buffer library](#121-the-buffer-library)
- [12.2 Why it matters](#122-why-it-matters)
- [12.3 UnreliableRemoteEvent](#123-unreliableremoteevent)
- [12.4 BufferWriter and BufferReader](#124-bufferwriter-and-bufferreader)
- [12.5 Quantization: compressing floats](#125-quantization-compressing-floats)
- [12.6 Compressing CFrames](#126-compressing-cframes)
- [12.7 Packing bits](#127-packing-bits)
- [12.8 Varint encoding](#128-varint-encoding)
- [12.9 Packet registry with type safety](#129-packet-registry-with-type-safety)
- [12.10 Batching, critical for performance](#1210-batching-critical-for-performance)
- [12.11 Delta compression](#1211-delta-compression)
- [12.12 Server-side safety](#1212-server-side-safety)
- [12.13 Time synchronization](#1213-time-synchronization)
- [12.14 Avoid RemoteFunction on the server](#1214-avoid-remotefunction-on-the-server)
- [12.15 Type-safe RemoteEvents](#1215-type-safe-remoteevents)

**13. Roblox API**

- [13.1 The `task` library, not `spawn`/`delay`/`wait`](#131-the-task-library-not-spawndelaywait)
- [13.2 Require by string](#132-require-by-string)
- [13.3 Attributes with types](#133-attributes-with-types)
- [13.4 CollectionService tags](#134-collectionservice-tags)
- [13.5 Tags on the instance itself](#135-tags-on-the-instance-itself)
- [13.6 `WaitForChild` with a timeout](#136-waitforchild-with-a-timeout)
- [13.7 `:Changed` is inconsistent](#137-changed-is-inconsistent)
- [13.8 Instance lookups you are probably missing](#138-instance-lookups-you-are-probably-missing)
- [13.9 RunService events, picking the right one](#139-runservice-events-picking-the-right-one)
- [13.10 Raycasts and shapecasts](#1310-raycasts-and-shapecasts)
- [13.11 Overlap queries](#1311-overlap-queries)
- [13.12 Allocation-free constants](#1312-allocation-free-constants)
- [13.13 Network ownership](#1313-network-ownership)
- [13.14 Parallel Luau with Actors](#1314-parallel-luau-with-actors)
- [13.15 Actor messaging for parallel Luau](#1315-actor-messaging-for-parallel-luau)
- [13.16 MemoryStoreService for cross-server](#1316-memorystoreservice-for-cross-server)
- [13.17 MessagingService](#1317-messagingservice)
- [13.18 DataStore: UpdateAsync and session locking](#1318-datastore-updateasync-and-session-locking)
- [13.19 Retry with exponential backoff](#1319-retry-with-exponential-backoff)
- [13.20 Deferred events](#1320-deferred-events)
- [13.21 Instance streaming](#1321-instance-streaming)
- [13.22 Modules: lifetime and require semantics](#1322-modules-lifetime-and-require-semantics)
- [13.23 Player and character lifecycle](#1323-player-and-character-lifecycle)
- [13.24 Shutting down with BindToClose](#1324-shutting-down-with-bindtoclose)
- [13.25 What replicates and what does not](#1325-what-replicates-and-what-does-not)
- [13.26 HttpService and JSON](#1326-httpservice-and-json)
- [13.27 EditableImage and EditableMesh](#1327-editableimage-and-editablemesh)
- [13.28 Small things that add up](#1328-small-things-that-add-up)
- [13.29 Enums are objects, not numbers](#1329-enums-are-objects-not-numbers)

**14. Working practice**

- [14.1 Standard script header](#141-standard-script-header)
- [14.2 Validate at the edge, trust on the inside](#142-validate-at-the-edge-trust-on-the-inside)
- [14.3 No magic numbers](#143-no-magic-numbers)
- [14.4 Tooling](#144-tooling)
- [14.5 Measure, do not guess](#145-measure-do-not-guess)
- [14.6 Testing](#146-testing)
- [14.7 CI](#147-ci)

**15. Quick reference**

- [Common mistakes](#common-mistakes)

**16. Sources**

</details>

---

## 1. Bindings: const and local

### 1.1 const bindings

> ✅ **Live game** &nbsp;`server + client` &nbsp;— new enough that older tooling (StyLua, Selene, luau-lsp) does not know `const` until you update it.

Luau has its own `const` binding next to `local`. A const binding behaves like
`local` but blocks reassignment after initialization.

```luau
const MAX_PLAYERS = 10
const SPAWN_RATE: number = 2.5

MAX_PLAYERS = 20   -- Compile error: cannot rebind a const binding
MAX_PLAYERS += 1   -- Compound assignment is blocked too
```

> [!IMPORTANT]
> Luau does **not** use the Lua 5.4 syntax `local x <const> = 0`.
> That was explicitly rejected because it is ugly and buys no performance in Luau
> (the multi-pass compiler already knows whether a local is ever rewritten).
> Luau went with the shorter `const x = 0` form.

What `const` does and does not do:

```luau
-- The binding is immutable
const config = { speed = 16 }
config = {}           -- ERROR: rebinding is blocked
config.speed = 100    -- OK: the CONTENTS are not protected

-- For value immutability you need table.freeze
const frozenConfig = table.freeze({ speed = 16 })
frozenConfig.speed = 100   -- ERROR at runtime
```

`table.freeze` is covered in [5.1](#51-table-functions).

`const` is a contextual keyword. Existing code that uses `const` as a variable
name keeps working. It also works with multi-assignment and function
declarations.

**Practical rule:** `const` for everything you never rebind (services, required
modules, constants), `local` only where you actually rebind.

```luau
const Players = game:GetService("Players")
const ReplicatedStorage = game:GetService("ReplicatedStorage")
const Signal = require(ReplicatedStorage.Shared.Signal)

const MAX_HEALTH = 100
const RESPAWN_TIME = 5

local currentWave = 1   -- this does change, so local
```

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 2. Syntax

### 2.1 Compound assignment


> ✅ **Live game** &nbsp;`server + client`

```luau
local x = 10

x += 5      -- 15
x -= 3      -- 12
x *= 2      -- 24
x /= 4      -- 6
x //= 4     -- 1   (floor division)
x %= 2      -- 1
x ^= 3      -- 1

local s = "hello"
s ..= " world"   -- "hello world"
```

> [!NOTE]
> `x += 1` evaluates `x` only once. With `t[expensiveCall()] += 1`,
> `expensiveCall()` runs once, not twice.

### 2.2 `continue`


> ✅ **Live game** &nbsp;`server + client`

Vanilla Lua does not have this, Luau does.

```luau
for i = 1, 10 do
    if i % 2 == 0 then
        continue
    end
    print(i)  -- odd numbers only
end
```

### 2.3 String interpolation (backticks)


> ✅ **Live game** &nbsp;`server + client`

```luau
local name = "Nathan"
local level = 42

print(`Player {name} is level {level}`)
print(`Next level: {level + 1}`)
print(`Table length: {#someTable}`)

-- Escaping with a backslash
print(`Literal brace: \{not interpolated\}`)
```

Works multiline too. Faster and more readable than `string.format` or `..`.

### 2.4 If-then-else as an expression


> ✅ **Live game** &nbsp;`server + client`

```luau
-- Instead of:
local status
if health > 50 then
    status = "healthy"
else
    status = "hurt"
end

-- You can write:
local status = if health > 50 then "healthy" else "hurt"

-- With elseif chains:
local tier =
    if score >= 1000 then "gold"
    elseif score >= 500 then "silver"
    elseif score >= 100 then "bronze"
    else "none"
```

`else` is mandatory. This is an expression, not a statement.

### 2.5 Generalized iteration


> ✅ **Live game** &nbsp;`server + client`

```luau
local t = {"a", "b", "c"}

-- The old way
for i, v in ipairs(t) do end
for k, v in pairs(t) do end

-- The Luau way (works for both)
for i, v in t do
    print(i, v)
end
```

Performance is comparable to `pairs` and `ipairs`, so this is about readability,
not speed. It is the recommended form unless you need vanilla Lua compatibility
or the exact `ipairs` behavior of stopping at the first nil.

Note: this works on tables, **not** on varargs. See [2.11](#211-using-varargs-correctly).

### 2.6 Number literals


> ✅ **Live game** &nbsp;`server + client`

```luau
local hex = 0xFF              -- 255
local binary = 0b1010         -- 10
local separated = 1_000_000   -- 1000000, underscores are cosmetic
local combined = 0b1010_1010  -- 170

local thousand = 1e3          -- 1000
local micro = 1e-6            -- 0.000001
local avogadro = 6.02e23
```

Binary literals plus underscores make bitflags a lot more readable. The `bit32`
functions that go with them are in [5.9](#59-bit32).

`1e3` is scientific notation: the number before the `e`, times ten to the power
after it. A negative exponent goes the other way, so `1e-6` is a millionth.

In Lua 5.3 and later this distinction matters, because `1e3` produces a float
while `1000` produces an integer. In Luau it does not. There is one number type,
a 64-bit double ([2.14](#214-number-precision-important-for-userids)), so `1e3`
and `1000` are the same value and the notation is purely about readability.

Which one to reach for depends on what you want the reader to see:

```luau
-- the magnitude is the point
const EPSILON = 1e-4
const NS_PER_SECOND = 1e9

-- the digits are the point
const MAX_COINS = 1_000_000
const PORT = 49_152
```

### 2.7 Floor division


> ✅ **Live game** &nbsp;`server + client`

```luau
print(7 / 2)    -- 3.5
print(7 // 2)   -- 3
print(-7 // 2)  -- -4  (rounds down, not toward zero)
```

### 2.8 String escapes


> ✅ **Live game** &nbsp;`server + client`

```luau
local hex = "\x41"        -- "A"
local unicode = "\u{1F600}"  -- emoji, braces required
local japanese = "\u{3042}"

-- \z skips all whitespace up to the next non-whitespace character
local long = "This is a very long string \z
              that spans multiple lines \z
              without newlines in the result"
```

`\z` is perfect for long error messages and SQL-like strings without unintended
newlines.

### 2.9 Type casts with `::`


> ✅ **Live game** &nbsp;`server + client`

```luau
local instance = workspace:FindFirstChild("Part") :: Part
local data = HttpService:JSONDecode(json) :: {name: string, id: number}

-- Force it through any when the compiler pushes back
local weird = (someValue :: any) :: MyType
```

This only exists for the type checker, it is not a runtime conversion. You are telling the type
checker "trust me". If you lie, it still crashes at runtime.

### 2.10 The `select` function

> ✅ **Live game** &nbsp;`server + client`

`select` reads a vararg pack without turning it into a table. Two forms:

```luau
local function demo(...)
    print(select("#", ...))   -- how many arguments were passed, nils included
    print(select(2, ...))     -- everything from position 2 onward
end

demo("a", "b", "c")   -- 3        then    b  c
```

`select("#", ...)` is the only reliable argument count. `#{...}` stops at the
first nil, so a call like `f(1, nil, 3)` gives you 3 from `select` and 1 from
the length operator.

Negative indices count from the end:

```luau
local function last(...)
    return (select(-1, ...))
end

local function lastTwo(...)
    return select(-2, ...)
end

print(last(1, 2, 3))       -- 3
print(lastTwo(1, 2, 3))    -- 2  3
```

That is handy for variadic APIs where the last argument is an optional callback
or an options table:

```luau
local function connect(...)
    const argCount = select("#", ...)
    const handler = select(-1, ...)

    if type(handler) ~= "function" then
        error("last argument must be a function", 2)
    end

    for i = 1, argCount - 1 do
        const signal = select(i, ...)
        signal:Connect(handler)
    end
end
```

### 2.11 Using varargs correctly


> ✅ **Live game** &nbsp;`server + client`

> [!WARNING]
> This is a classic mistake.

```luau
-- WRONG: this does not do what you think
local function sum(...: number)
    for _, n in ... do end  -- takes the first 3 varargs as iterator/state/control
end
```

Correct:

```luau
-- Option 1: table.pack (also gives you .n for nils)
local function sum(...: number): number
    local args = table.pack(...)
    local total = 0
    for i = 1, args.n do
        total += args[i]
    end
    return total
end

-- Option 2: select (no allocation)
local function sum2(...: number): number
    local total = 0
    for i = 1, select("#", ...) do
        total += (select(i, ...))
    end
    return total
end

-- Option 3: {...} (fastest, but does not count trailing nils)
local function sum3(...: number): number
    local total = 0
    for _, n in {...} do
        total += n
    end
    return total
end
```

`select("#", ...)` gives the argument count including nils. `#{...}` does not.

### 2.12 Multiple returns get truncated


> ✅ **Live game** &nbsp;`server + client`

```luau
local function two() return 1, 2 end

print(two())           -- 1  2
print(two(), 3)        -- 1  3   (truncated to 1 value!)
print((two()))         -- 1      (parentheses always truncate)

local t = {two()}      -- {1, 2}
local t2 = {two(), 3}  -- {1, 3}

-- It only expands as the last element
local t3 = {3, two()}  -- {3, 1, 2}
```

> [!WARNING]
> This rule bites often with `table.insert(t, f())` where `f` returns several values.

### 2.13 Checking for an empty table


> ✅ **Live game** &nbsp;`server + client`

```luau
-- Wrong: #t is 0 for dictionaries with only string keys
if #dictionary == 0 then end

-- Right
if next(dictionary) == nil then
    print("empty")
end
```

`next` is also the fastest way, because it stops after the first element.

### 2.14 Number precision, important for UserIds


> ✅ **Live game** &nbsp;`server + client`

Luau has one number type: a 64-bit double. Integers are exact up to 2^53.

```luau
print(2^53)      -- 9007199254740992
print(2^53 + 1)  -- 9007199254740992, precision loss!

-- Roblox UserIds sit well below 2^53, so those are safe
-- But be careful with hashes, nanosecond timestamps, and IDs from external APIs
```

> [!IMPORTANT]
> When serializing to buffers: `writef64` preserves the full double. `writeu32`
> only goes up to 4294967295. A UserId does not fit in a u32.

```luau
-- Wrong for UserIds
buffer.writeu32(b, 0, player.UserId)   -- overflow on large IDs

-- Right
buffer.writef64(b, 0, player.UserId)   -- 8 bytes, exact
```

### 2.15 `tonumber` with a base


> ✅ **Live game** &nbsp;`server + client`

```luau
tonumber("ff", 16)      -- 255
tonumber("1010", 2)     -- 10
tonumber("z", 36)       -- 35

-- Handy for hex colors ( gsub: 5.6, bit32: 5.9 )
local function hexToColor3(hex: string): Color3
    const value = assert(tonumber(hex:gsub("#", ""), 16), "invalid hex")
    return Color3.fromRGB(
        bit32.band(bit32.rshift(value, 16), 0xFF),
        bit32.band(bit32.rshift(value, 8), 0xFF),
        bit32.band(value, 0xFF)
    )
end
```

### 2.16 Tail calls do not exist in Luau


> ✅ **Live game** &nbsp;`server + client`

Lua 5.1 has proper tail calls. Luau deliberately does not, because they ruin the
quality of stack traces.

```luau
-- In Lua 5.1: constant stack. In Luau: stack overflow.
local function countdown(n: number): string
    if n == 0 then return "done" end
    return countdown(n - 1)
end

countdown(100000)  -- stack overflow in Luau
```

Write deep recursion as a loop:

```luau
local function countdown(n: number): string
    while n > 0 do
        n -= 1
    end
    return "done"
end
```

Or use an explicit stack for tree traversal:

```luau
local function walk(root: Instance, visit: (Instance) -> ())
    const stack = { root }
    local top = 1

    while top > 0 do
        const current = stack[top]
        stack[top] = nil
        top -= 1

        visit(current)

        for _, child in current:GetChildren() do
            top += 1
            stack[top] = child
        end
    end
end
```

For deep instance trees this is safer than recursion.


<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 3. Type system

### 3.1 Basic annotations


> 🧩 **Live game** &nbsp;`type-check only`

```luau
local name: string = "Nathan"
local count: number = 0
local active: boolean = true
local maybe: string? = nil          -- string | nil
local anything: any = whatever
local unknownThing: unknown = value -- must be narrowed before use
local impossible: never             -- can never hold a value

local list: {number} = {1, 2, 3}
local map: {[string]: number} = {a = 1}
local mixed: {name: string, age: number} = {name = "x", age = 1}
```

`any` turns type checking off. `unknown` is the safe variant: you have to narrow
it before you may do anything with it.

### 3.2 Generics


> 🧩 **Live game** &nbsp;`type-check only`

```luau
-- Simple generic
local function identity<T>(value: T): T
    return value
end

-- Multiple type params
local function map<T, U>(list: {T}, transform: (T) -> U): {U}
    local out = table.create(#list)
    for i, v in list do
        out[i] = transform(v)
    end
    return out
end

-- Generic type alias
type Dictionary<K, V> = {[K]: V}
type Stack<T> = { items: {T} }

-- Default type parameters
type Response<T = string> = { ok: boolean, body: T }
local r: Response = { ok = true, body = "hi" }        -- T = string
local r2: Response<number> = { ok = true, body = 42 }
```

### 3.3 Function types


> 🧩 **Live game** &nbsp;`type-check only`

```luau
type Callback = (player: Player, amount: number) -> boolean
type Supplier<T> = () -> T
type Predicate<T> = (T) -> boolean
type NoReturn = () -> ()              -- no return value, NOT "void"
type Multi = (number) -> (string, boolean)  -- multiple returns
type Variadic = (...string) -> ()
```

There is no `void` type in Luau. Use `()`.

### 3.4 Type aliases and export


> 🧩 **Live game** &nbsp;`type-check only`

```luau
-- In a ModuleScript
export type PlayerData = {
    userId: number,
    coins: number,
    inventory: {string},
}

export type Rarity = "common" | "rare" | "epic" | "legendary"

return {}
```

```luau
-- In another script
local Types = require(ReplicatedStorage.Types)
type PlayerData = Types.PlayerData

local data: PlayerData = { userId = 1, coins = 0, inventory = {} }
```

### 3.5 Singleton types (literal types)


> 🧩 **Live game** &nbsp;`type-check only`

Underrated and very powerful.

```luau
type Rarity = "common" | "rare" | "epic" | "legendary"

local function getColor(rarity: Rarity): Color3
    if rarity == "common" then return Color3.new(1, 1, 1)
    elseif rarity == "rare" then return Color3.new(0, 0.5, 1)
    elseif rarity == "epic" then return Color3.new(0.6, 0, 1)
    else return Color3.new(1, 0.8, 0) end
end

getColor("rare")     -- OK
getColor("legendry") -- Type error, the typo gets caught
```

Works with booleans too:

```luau
type Loaded = { loaded: true, data: string }
type Loading = { loaded: false }
type State = Loaded | Loading
```

### 3.6 Tagged unions and narrowing


> 🧩 **Live game** &nbsp;`type-check only`

This is the correct way to build a Result type. A union of `{ok: T} | {err: E}`
does not work well because you are not allowed to read `.ok` on the err variant.
Use a **tag** field with singleton types:

```luau
type Result<T, E> =
    { success: true, value: T }
    | { success: false, error: E }

local function divide(a: number, b: number): Result<number, string>
    if b == 0 then
        return { success = false, error = "Division by zero" }
    end
    return { success = true, value = a / b }
end

local result = divide(10, 2)
if result.success then
    print(result.value)   -- Luau knows: this is the success variant
else
    warn(result.error)    -- Luau knows: this is the error variant
end
```

Same pattern for game events:

```luau
type NetworkMessage =
    { kind: "chat", text: string, sender: number }
    | { kind: "purchase", itemId: string, price: number }
    | { kind: "join", userId: number }

local function handle(msg: NetworkMessage)
    if msg.kind == "chat" then
        print(msg.sender, msg.text)
    elseif msg.kind == "purchase" then
        print(msg.itemId, msg.price)
    else
        print(msg.userId)
    end
end
```

### 3.7 Type refinements


> 🧩 **Live game** &nbsp;`type-check only`

Luau narrows types automatically based on checks.

```luau
local function process(value: string | number | nil)
    if value == nil then
        return  -- value: nil
    end

    if type(value) == "string" then
        print(value:upper())   -- value: string
    else
        print(value + 1)       -- value: number
    end
end

-- typeof for Roblox types
local function onHit(part: Instance)
    if part:IsA("BasePart") then
        print(part.Size)  -- BasePart, .Size is known
    end
end

-- assert narrows too
local function needsValue(v: string?)
    assert(v, "v is required")
    print(v:len())  -- v: string, no warning
end

-- and/or narrows
local function safe(v: string?)
    local length = v and #v or 0
    return length
end
```

### 3.8 Type packs (variadic generics)


> 🧩 **Live game** &nbsp;`type-check only`

```luau
-- T... is a pack of types
local function pack<T...>(...: T...): () -> T...
    local stored = table.pack(...)
    return function(): T...
        return table.unpack(stored, 1, stored.n)
    end
end

type Handler<A...> = (A...) -> ()

-- Practical: a spawn wrapper that passes arguments through with types intact
local function safeSpawn<A...>(fn: (A...) -> (), ...: A...)
    task.spawn(function(...)
        local ok, err = pcall(fn, ...)
        if not ok then warn(err) end
    end, ...)
end
```

### 3.9 Read-only and write-only properties


> 🧩 **Live game** &nbsp;`type-check only`

Available in the new type solver.

```luau
type ReadOnlyConfig = {
    read maxPlayers: number,
    read spawnRate: number,
}

type WriteOnlyLog = {
    write message: string,
}

local config: ReadOnlyConfig = { maxPlayers = 10, spawnRate = 2.5 }
print(config.maxPlayers)   -- OK
config.maxPlayers = 20     -- Type error
```

This only exists for the type checker. For real runtime protection use `table.freeze`.

### 3.10 Intersection types


> 🧩 **Live game** &nbsp;`type-check only`

```luau
type Named = { name: string }
type Aged = { age: number }
type Person = Named & Aged   -- has both

local p: Person = { name = "x", age = 1 }
```

Useful for mixins and for extending existing types.

### 3.11 Function overloads via intersection types


> 🧩 **Live game** &nbsp;`type-check only`

Underused and very handy for utility modules.

```luau
type Getter =
    ((instance: Instance, name: "Position") -> Vector3)
    & ((instance: Instance, name: "Name") -> string)
    & ((instance: Instance, name: string) -> unknown)

local get: Getter = function(instance, name)
    return (instance :: any)[name]
end

local pos = get(part, "Position")  -- Luau knows: Vector3
local nm = get(part, "Name")       -- Luau knows: string
```

### 3.12 Recursive and mutually recursive types


> 🧩 **Live game** &nbsp;`type-check only`

```luau
type TreeNode = {
    value: number,
    children: {TreeNode},
}

-- Mutually recursive
type Expression = Literal | BinaryOp
type Literal = { kind: "literal", value: number }
type BinaryOp = { kind: "binary", op: string, left: Expression, right: Expression }

-- JSON type
type JSONValue = string | number | boolean | nil | {JSONValue} | {[string]: JSONValue}
```

### 3.13 `never` for exhaustiveness checking


> 🧩 **Live game** &nbsp;`type-check only`

This is the killer feature for tagged unions. If you add a variant later and
forget to handle it, the type checker flags it.

```luau
type Packet =
    { kind: "move", x: number, y: number }
    | { kind: "shoot", targetId: number }
    | { kind: "reload" }

local function assertNever(value: never): never
    error(`Unhandled variant: {value}`)
end

local function handle(packet: Packet)
    if packet.kind == "move" then
        move(packet.x, packet.y)
    elseif packet.kind == "shoot" then
        shoot(packet.targetId)
    elseif packet.kind == "reload" then
        reload()
    else
        assertNever(packet)  -- packet is `never` here
    end
end
```

Add `{ kind: "jump" }` to Packet later and forget the branch, and `packet` in the
else is no longer `never`, so you get a type error. Exactly what you want in a
networker with many packet types.

A type error is not a compile error. The script still compiles and runs with the
missing branch, so this only protects you if type errors actually block
something, for example a CI step that fails on them ([14.7](#147-ci)).

### 3.14 Branded types do not hold up in Luau

> 🧩 **Live game** &nbsp;`type-check only`

Branding is a TypeScript idiom for getting nominal typing out of a structural type
system: intersect a primitive with a marker, so two ids that are both numbers stop
being interchangeable.

```luau
type UserId = number & { __brand: "UserId" }
type ItemId = number & { __brand: "ItemId" }
```

In Luau this is not a supported pattern. A `number` is never a table, so
`number & { __brand: "UserId" }` describes a value that cannot exist, and once the
type goes through normalization it simplifies to `never`. It only appears to work
where that normalization happens not to run, which means it can stop working
without you changing a line.

If two ids really need to be distinct, make the difference real. A wrapper table
costs an allocation but is an honest type:

```luau
type UserId = { read userId: number }
type ItemId = { read itemId: number }

local function giveItem(user: UserId, item: ItemId) end

giveItem({ userId = 1 }, { userId = 2 })   -- Type error: itemId is missing
```

For most code the cheaper answer is naming parameters well and validating at the
boundary ([14.2](#142-validate-at-the-edge-trust-on-the-inside)).

### 3.15 `typeof()` in type context


> 🧩 **Live game** &nbsp;`type-check only`

```luau
local Template = {
    health = 100,
    speed = 16,
    name = "default",
}

type Stats = typeof(Template)   -- {health: number, speed: number, name: string}

-- Very handy for OOP, see the Patterns chapter
```

### 3.16 User-defined type functions


> 🧩 **Live game** &nbsp;`type-check only` &nbsp;— and only with the new type solver enabled.

A newer feature (new type solver). You can compute types at compile time.

```luau
type function MakeOptional(t)
    local newType = types.newtable()
    for property, propType in t:properties() do
        newType:setproperty(property, types.unionof(propType.read, types.singleton(nil)))
    end
    return newType
end

type User = { id: number, name: string }
type PartialUser = MakeOptional<User>   -- { id: number?, name: string? }
```

Type functions need the new type solver. The official reference, including the
built-ins such as `keyof` and `setmetatable`, is
[Type functions](https://luau.org/types/type-functions).


<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 4. Directives and attributes

### 4.1 Script directives

> ⚠️ **Live game** &nbsp;`partly` &nbsp;— `--!strict`, `--!nonstrict` and `--!nocheck` are analysis only. Published games already compile at optimization level 2, so `--!optimize 2` only changes Studio. `--!native` is documented as **server side**. There are reports of it being enabled on Android clients but no announcement, so do not count on it on the client.

At the top of the script, they must be the first lines:

```luau
--!strict          -- full type checking
--!nonstrict       -- light checking (the default in most cases)
--!nocheck         -- no checking
--!native          -- compile this script to machine code
--!optimize 2      -- maximum optimization, published games already use it
```

Function-level attributes:

```luau
@native
local function hotPath(x: number): number
    return x * x + x
end

@deprecated
local function oldFunction() end
```

The bare `@deprecated` takes no arguments. To give a reason, use the bracket form
in [4.2](#42-parameterized-attributes).

`--!native` only pays off for CPU-heavy code (math, loops). For scripts that
mostly wait on Roblox APIs it buys nothing and costs compile time. When it is
worth it and when it is not is in [11.6](#116-when---native-pays-off-and-when-it-does-not).

### 4.2 Parameterized attributes

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `@deprecated` only affects the linter, `@native` follows the same rules as `--!native`.

Beyond the bare `@native` and `@deprecated` from the previous section, the syntax
is richer:

```luau
-- Three spellings of the same attribute. Pick one, the parser rejects duplicates
@native
local function a() end

@[native]
local function b() end

@[native()]
local function c() end

-- Multiple attributes on one line
@[native, deprecated]
local function oldFast() end

-- Attributes with parameters
@[deprecated { use = "Networker.send()", reason = "Old API is not type-safe" }]
local function sendData(...) end
```

The linter then reports: `Function 'sendData' is deprecated, use 'Networker.send()' instead. Old API is not type-safe`

For a member of a table it becomes `Member 'Networker.sendData' is deprecated`.

`@native` does **not** work recursively. Inner functions need their own marker:

```luau
@native
local function outer(n: number)
    @native
    local function inner(x: number)
        return x * x
    end
    return inner(n)
end
```

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 5. Standard library

### 5.1 Table functions

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Preallocate, saves rehashing in loops
local arr = table.create(1000)          -- capacity 1000, length 0
local filled = table.create(10, "x")    -- 10x "x"

-- Shallow copy
local copy = table.clone(original)

-- Make immutable
local frozen = table.freeze({a = 1})
print(table.isfrozen(frozen))  -- true (note: lowercase f)

-- Fast copying between tables
table.move(source, 1, #source, 1, destination)

-- Searching
local index = table.find(list, value)
local indexFrom = table.find(list, value, 5)  -- start at index 5

-- Varargs
local packed = table.pack(1, nil, 3)   -- {1, nil, 3, n = 3}
local a, b, c = table.unpack(packed, 1, packed.n)

-- Sorting with a comparator
table.sort(players, function(a, b)
    return a.score > b.score
end)
```

`table.freeze` is shallow. Nested tables have to be frozen separately:

```luau
local function deepFreeze<T>(t: T & {}): T
    for _, v in t :: any do
        if type(v) == "table" and not table.isfrozen(v) then
            deepFreeze(v)
        end
    end
    return table.freeze(t)
end
```

### 5.2 String functions

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Splitting
local parts = string.split("a,b,c", ",")   -- {"a", "b", "c"}

-- Formatting
print(string.format("%.2f coins", 10.567))   -- "10.57 coins"
print(string.format("%d/%d", 5, 10))         -- "5/10"
print(string.format("%*", someValue))        -- Luau: tostring on any type

-- Patterns (Lua patterns, not regex)
for word in string.gmatch(text, "%a+") do print(word) end
local cleaned = string.gsub(input, "%s+", " ")

-- Handy checks
print(("hello"):find("ell"))       -- 2 4
print(("hello"):sub(2, 4))         -- "ell"
print(("hello"):rep(3, "-"))       -- "hello-hello-hello" (separator!)
print(("  x  "):gsub("^%s*(.-)%s*$", "%1"))  -- trim
```

> [!TIP]
> Heavy string concatenation in a loop is slow. Use `table.concat`:

```luau
-- Slow
local s = ""
for i = 1, 1000 do s ..= tostring(i) end

-- Fast
local buf = table.create(1000)
for i = 1, 1000 do buf[i] = tostring(i) end
local s = table.concat(buf)
```

### 5.3 `%b` matches balanced pairs

> ✅ **Live game** &nbsp;`server + client`

```luau
const code = "call(a, (b + c), d) rest"
print(code:match("%b()"))   -- (a, (b + c), d)

const json = '{"a": {"b": 1}} trailing'
print(json:match("%b{}"))   -- {"a": {"b": 1}}
```

`%bxy` means: start at x, count up on every x and down on every y, stop at
balance zero. Works for parentheses, braces and brackets.

### 5.4 `%f` frontier pattern

> ✅ **Live game** &nbsp;`server + client`

Matches a transition between character classes without consuming characters.
This is the Lua way of doing word boundaries.

```luau
const text = "cat concatenate cathedral cat"

-- Without frontier: also matches inside "concatenate"
for w in text:gmatch("cat") do end       -- 4 matches

-- With frontier: whole words only
for w in text:gmatch("%f[%a]cat%f[%A]") do
    print(w)                              -- 2 matches
end
```

`%f[%a]` means "a letter starts here after a non-letter".
`%f[%A]` means "a letter ends here".

Useful for chat filters and command parsing.

### 5.5 Position captures

> ✅ **Live game** &nbsp;`server + client`

Empty parentheses `()` capture the position instead of text.

```luau
const s = "hello world"
print(s:match("()world"))          -- 7
print(s:match("()(world)()"))      -- 7  world  12
```

Handy when you want to replace a substring without searching the string again.

### 5.6 `gsub` with a table or function

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Table: the capture becomes the key
const values = { name = "Nathan", coins = "500" }
print(("Hello {name}, {coins} coins"):gsub("{(%w+)}", values))

-- Function: the capture becomes the argument
const doubled = ("a1b2c3"):gsub("%d", function(d)
    return tostring((tonumber(d) :: number) * 2)
end)
print(doubled)   -- a2b4c6

-- Returning nil or false from the function leaves the match untouched
const selective = ("a1b2c3"):gsub("%d", function(d)
    if d == "2" then return "X" end
    return nil   -- leave it
end)
print(selective)  -- a1bXc3
```

The fourth argument limits the number of replacements:

```luau
print(("aaa"):gsub("a", "b", 2))  -- bba  2
```

### 5.7 Escaping user input in patterns

> ✅ **Live game** &nbsp;`server + client`

If a player types a pattern character in their search term, your match breaks.

```luau
local function escapePattern(s: string): string
    return (s:gsub("[%^%$%(%)%%%.%[%]%*%+%-%?]", "%%%1"))
end

const search = escapePattern(userInput)
if text:find(search) then end
```

Or use `string.find(text, needle, 1, true)`. The fourth argument `true` disables
patterns entirely and is faster on top of that.

### 5.8 The `utf8` library

> ✅ **Live game** &nbsp;`server + client`

Important anywhere players type names or chat in something other than English.

```luau
const text = "Café 日本語"

print(#text)             -- byte length, NOT characters
print(utf8.len(text))    -- character length

-- Iterating over real characters
for _, codepoint in utf8.codes(text) do
    print(utf8.char(codepoint))
end

-- Roblox only: graphemes (emoji with modifiers as one unit)
for first, last in utf8.graphemes(text) do
    print(text:sub(first, last))
end

utf8.codepoint(text, 1)      -- codepoint at a byte position
utf8.offset(text, 3)         -- byte position of the 3rd character
utf8.nfcnormalize(text)      -- Roblox only, normalizing
utf8.nfdnormalize(text)      -- Roblox only
```

`utf8.graphemes`, `utf8.nfcnormalize` and `utf8.nfdnormalize` are Roblox
additions. They are not in the [Luau standard library](https://luau.org/library),
so they do not exist in Lune or Lute.

To truncate a display name use `utf8.offset`, not `string.sub`, otherwise you cut
in the middle of a multi-byte character.

```luau
local function truncate(s: string, maxChars: number): string
    if utf8.len(s) <= maxChars then return s end
    const cutoff = utf8.offset(s, maxChars + 1)
    return s:sub(1, cutoff - 1) .. "..."
end
```

### 5.9 Bit32

> ✅ **Live game** &nbsp;`server + client`

```luau
local READ   = 0b0001
local WRITE  = 0b0010
local DELETE = 0b0100
local ADMIN  = 0b1000

-- Combining
local perms = bit32.bor(READ, WRITE)

-- Checking
local function has(flags: number, flag: number): boolean
    return bit32.band(flags, flag) ~= 0
end

print(has(perms, READ))    -- true
print(has(perms, ADMIN))   -- false

-- Adding / removing
perms = bit32.bor(perms, ADMIN)             -- add
perms = bit32.band(perms, bit32.bnot(READ)) -- remove

-- The rest
bit32.bxor(a, b)              -- XOR
bit32.lshift(1, 4)            -- 16
bit32.rshift(16, 4)           -- 1
bit32.extract(value, 4, 8)    -- 8 bits starting at position 4
bit32.replace(value, 5, 0, 4) -- replace 4 bits from 0 with 5
bit32.countlz(x)              -- leading zeros
bit32.countrz(x)              -- trailing zeros
```

### 5.10 Math

> ✅ **Live game** &nbsp;`server + client`

```luau
math.clamp(value, min, max)
math.sign(-5)          -- -1
math.round(2.5)        -- 3
math.fmod(7, 3)        -- 1
math.noise(x, y, z)    -- Perlin noise (Roblox)

-- Random with its own seed, does not affect math.random
local rng = Random.new(12345)          -- Roblox
print(rng:NextInteger(1, 100))

-- Pure Luau
math.randomseed(os.time())
print(math.random(1, 100))
```

Also in the standard library and easy to miss:

```luau
math.lerp(0, 100, 0.25)          -- 25
math.map(5, 0, 10, 0, 100)       -- 50, remaps from one range to another
math.isnan(0/0)                  -- true
math.isinf(math.huge)            -- true
math.isfinite(1/0)               -- false
```

### 5.11 Raw access, skipping metamethods

> ✅ **Live game** &nbsp;`server + client`

Skips metamethods. Handy inside `__index` implementations to avoid infinite
recursion.

```luau
rawget(t, key)        -- read without __index
rawset(t, key, value) -- write without __newindex
rawequal(a, b)        -- compare without __eq
rawlen(t)             -- length without __len

-- the metamethods these skip are in section 7
```

### 5.12 The debug library

> ✅ **Live game** &nbsp;`server + client` &nbsp;— the MicroProfiler and the memory categories are in the live client too, not just in Studio.

```luau
-- Profiling (visible in the MicroProfiler), Roblox only
debug.profilebegin("PathfindingUpdate")
-- heavy code
debug.profileend()

-- Memory categories in the Developer Console, Roblox only
debug.setmemorycategory("EnemyAI")

-- Stack info, standard Luau
print(debug.traceback())
local name, line = debug.info(1, "nl")   -- level, what you want to know
```

`debug.info` options: `s` source, `l` line, `n` name, `f` function, `a` arity.

`debug.traceback` and `debug.info` are standard Luau. The profiling and memory
category functions are Roblox additions and do not exist outside the engine.

### 5.13 Picking the right clock

> ✅ **Live game** &nbsp;`server + client`

| Function | What | For what |
|---|---|---|
| `os.clock()` | monotonic, high precision | benchmarks, deltas |
| `os.time()` | Unix seconds | storing, dates |
| `tick()` | epoch, deprecated | do not use |
| `workspace:GetServerTimeNow()` | server synchronized | hit reg, replay, timers |
| `DateTime.now()` | object with formatting | UI, logging |

`os.clock()` does not necessarily count wall time, but it is the only one with
enough precision for microbenchmarks.

### 5.14 `os.date` with tables

> ✅ **Live game** &nbsp;`server + client`

```luau
const t = os.date("*t")       -- local time as a table
const utc = os.date("!*t")    -- UTC, mind the exclamation mark

print(t.year, t.month, t.day)
print(t.hour, t.min, t.sec)
print(t.wday)    -- 1 = Sunday
print(t.yday)    -- day of the year
print(t.isdst)   -- daylight saving time
```

The other way around, a table to a timestamp:

```luau
const timestamp = os.time({
    year = 2026,
    month = 8,
    day = 8,
    hour = 12,
    min = 0,
    sec = 0,
})
```

Formatting:

```luau
print(os.date("%Y-%m-%d %H:%M:%S"))
print(os.date("!%Y-%m-%dT%H:%M:%SZ"))   -- ISO 8601 in UTC
```

### 5.15 `DateTime` in Roblox

> ✅ **Live game** &nbsp;`server + client` &nbsp;— a Roblox datatype, it does not exist in standard Luau.

```luau
const now = DateTime.now()

print(now.UnixTimestamp)
print(now.UnixTimestampMillis)

print(now:ToIsoDate())
print(now:FormatUniversalTime("LLL", "en-us"))
print(now:FormatLocalTime("HH:mm", "en-us"))

const parsed = DateTime.fromIsoDate("2026-08-08T12:00:00Z")
const fromUnix = DateTime.fromUnixTimestamp(1754654400)

const parts = now:ToUniversalTime()   -- table with Year, Month, Day, ...
```

For daily rewards and cooldowns: store `os.time()` in the DataStore and compare
against `os.time()` on the server. Never against client time.

### 5.16 The `vector` library

> ✅ **Live game** &nbsp;`server + client` &nbsp;— the friction between `vector` and `Vector3` is purely a type checker thing, at runtime it is the same value type.

Next to `Vector3`, Luau itself has a `vector` type with its own library. It is a
VM primitive, not userdata, so nothing is allocated and the GC has nothing to
clean up.

```luau
const v = vector.create(1, 2, 3)

vector.magnitude(v)
vector.normalize(v)
vector.cross(a, b)
vector.dot(a, b)
vector.angle(a, b)          -- radians, optional axis as the third argument
vector.floor(v)
vector.abs(v)
vector.max(a, b)

vector.zero
vector.one
```

Components are read with lowercase names: `v.x`, `v.y`, `v.z`. Vectors are
immutable, writing to a component is not possible. The normal operators work as
you expect, including multiplying by a scalar.

The confusing part in Roblox: `typeof(vector.create(1, 2, 3))` returns
`"Vector3"`, because underneath it is the same value type. The type checker has
not always treated `vector` and `Vector3` as the same type though, so under
`--!strict` you can get a complaint that `vector` cannot be converted to
`Vector3`. Two options:

```luau
-- Cast at the boundary with the Roblox API
part.Position = vector.create(x, y, z) :: any

-- Or just use Vector3.new wherever you talk to instances,
-- and keep vector for your own math loops
```

Practical rule: `vector` in hot math (raymarching, physics loops, buffer
serialization), `Vector3` at the boundary with the engine API. If your code also
has to run outside Roblox through Lune or Lute, `vector` is the only option
anyway, because `Vector3` does not exist there.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 6. Tables from the inside

### 6.1 Array part and hash part

> ✅ **Live game** &nbsp;`server + client`

Every Luau table has two internal storage parts. Consecutive integer keys
starting at 1 land in the array part (compact, fast indexing). Everything else
goes to the hash part (more memory, hash lookup).

```luau
const fast = { 1, 2, 3 }               -- entirely array part
const slow = { [1] = 1, [3] = 3 }      -- hole at 2, partly hash
const mixed = { 1, 2, name = "x" }     -- both parts
```

Anti-pattern:

```luau
-- Puts everything in the hash part, because no array part was reserved
const t = {}
t[1000] = "x"
t[999] = "x"
```

Always fill upward from 1, or use `table.create(n)` to reserve the array part in
advance.

### 6.2 `#` is a border, not a length

> ✅ **Live game** &nbsp;`server + client`

```luau
const t = { 1, 2, 3 }
t[5] = 5
print(#t)  -- may return 3 or 5, both are correct
```

The definition of `#` is "an index n where t[n] is non-nil and t[n+1] is nil".
With holes there are several valid answers and the implementation may pick.

As soon as holes are possible, track your own count:

```luau
export type List<T> = { items: {T}, count: number }
```

Or use `table.pack(...)` and read `.n`, which counts trailing nils too.

### 6.3 NaN as a key crashes

> ✅ **Live game** &nbsp;`server + client`

```luau
const t = {}
const bad = 0/0
t[bad] = 1   -- error: table index is NaN
```

This happens easily with computed keys. NaN is the only number that is not equal
to itself, so this is the check:

```luau
if value ~= value then
    error("NaN detected")
end
```

Or say what you mean with the standard library:

```luau
if math.isnan(value) then
    error("NaN detected")
end

math.isinf(value)      -- true for math.huge and -math.huge
math.isfinite(value)   -- false for NaN and both infinities
```

Also relevant when deserializing: a corrupt float in a buffer can produce NaN.
`math.isfinite` is usually the check you want at a network boundary, because an
infinite position breaks your math just as badly as a NaN one.

If the check sits in a hot loop: `value ~= value` compiles to a single
compare-and-branch instruction, while `math.isnan` is a fastcall that writes a
boolean and then branches on it. In the interpreter that makes the self compare
about 1.5x faster in a tight branch. With native codegen both compile to the same
machine code. It comes down to a few nanoseconds per check, so outside a hot loop
use whichever reads better.

### 6.4 `table.clear` versus `= {}`

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Allocates a new table, the old one becomes GC material
hits = {}

-- Reuses the existing capacity, no allocation
table.clear(hits)
```

In a `Heartbeat` loop that empties a buffer table every frame, this is the
difference between constant GC pressure and almost none.

Note: `table.clear` keeps the capacity. If the table once held 10000 entries,
that memory stays reserved.

### 6.5 Swap-remove for unordered lists

> ✅ **Live game** &nbsp;`server + client`

`table.remove(t, i)` shifts everything after it: O(n). If order does not matter,
do this:

```luau
local function swapRemove<T>(list: {T}, index: number)
    const n = #list
    list[index] = list[n]
    list[n] = nil
end
```

O(1) instead of O(n). For bullet lists, particle pools and enemy arrays this
matters enormously.

Removing while iterating, always backwards:

```luau
for i = #list, 1, -1 do
    if shouldRemove(list[i]) then
        table.remove(list, i)
    end
end
```

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 7. Metatables

### 7.1 All metamethods

> ✅ **Live game** &nbsp;`server + client`

```luau
local mt = {
    __index = function(t, k) end,       -- reading a missing key
    __newindex = function(t, k, v) end, -- writing to a missing key
    __call = function(self, ...) end,   -- calling the object like a function
    __len = function(self) end,         -- #object
    __eq = function(a, b) end,          -- a == b
    __lt = function(a, b) end,          -- a < b
    __le = function(a, b) end,          -- a <= b
    __add = function(a, b) end,         -- a + b
    __sub = function(a, b) end,
    __mul = function(a, b) end,
    __div = function(a, b) end,
    __idiv = function(a, b) end,        -- a // b
    __mod = function(a, b) end,
    __pow = function(a, b) end,
    __unm = function(a) end,            -- -a
    __concat = function(a, b) end,      -- a .. b
    __tostring = function(self) end,    -- tostring(object)
    __iter = function(self) end,        -- for x in object
    __mode = "k",                       -- weak keys/values
    __metatable = "locked",             -- blocks getmetatable
}
```

### 7.2 `__iter`, Luau specific and heavily underused

> ✅ **Live game** &nbsp;`server + client`

```luau
local Inventory = {}
Inventory.__index = Inventory

function Inventory.new()
    return setmetatable({ items = {} }, Inventory)
end

function Inventory:add(item: string)
    table.insert(self.items, item)
end

-- This lets you iterate the object directly
function Inventory.__iter(self)
    return next, self.items
end

local inv = Inventory.new()
inv:add("sword")
inv:add("shield")

for i, item in inv do   -- no inv.items needed
    print(i, item)
end
```

### 7.3 Weak tables for caching

> ✅ **Live game** &nbsp;`server + client`

```luau
-- __mode = "k" : keys are weak, the entry disappears when the key is collected
local instanceCache = setmetatable({}, { __mode = "k" })

local function getData(instance: Instance)
    local cached = instanceCache[instance]
    if cached then return cached end

    local data = expensiveComputation(instance)
    instanceCache[instance] = data
    return data
end
```

When the Instance goes away, the garbage collector cleans up the cache entry
automatically. No memory leak. Options: `"k"`, `"v"`, `"kv"`.

Add `s` to any of those (`"ks"`, `"vs"`, `"kvs"`) and the garbage collector is
also allowed to shrink the table once fewer than three eighths of its slots are
in use. Without it, a cache that once held 10000 entries keeps that capacity
after it empties out. The `s` does nothing on its own, it only applies to tables
that are already weak.

### 7.4 `__newindex` for runtime read-only

> ✅ **Live game** &nbsp;`server + client`

```luau
local function readOnly(t: {[any]: any})
    return setmetatable({}, {
        __index = t,
        __newindex = function()
            error("Attempt to modify read-only table", 2)
        end,
        __len = function() return #t end,
        __iter = function() return next, t end,
        __metatable = "locked",
    })
end

local Config = readOnly({ speed = 16 })
print(Config.speed)   -- 16
Config.speed = 100    -- error
```

`table.freeze` is better and faster for this these days. The pattern is still
useful if you want custom behavior on write attempts (logging, for example).

### 7.5 `__call` for callable objects

> ✅ **Live game** &nbsp;`server + client`

```luau
local Counter = {}
Counter.__index = Counter

function Counter.new()
    return setmetatable({ count = 0 }, Counter)
end

function Counter.__call(self)
    self.count += 1
    return self.count
end

local tick = Counter.new()
print(tick())  -- 1
print(tick())  -- 2
```

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 8. Closures and coroutines

### 8.1 Closure caching

> ✅ **Live game** &nbsp;`server + client`

Luau caches closures that capture no mutable upvalues. This is about the
allocation itself; the upvalue trap is in the next section.

```luau
local function make()
    return function() print("hi") end
end

print(make() == make())  -- true, identical closure object, no allocation
```

As soon as you capture a mutable local, every call is a new allocation:

```luau
local function makeCounter()
    local count = 0
    return function()
        count += 1
        return count
    end
end

print(makeCounter() == makeCounter())  -- false
```

Practical consequence: a callback that only uses its parameters is free, a
callback that captures something from scope is not.

```luau
-- Allocated per entity, per frame
for _, entity in entities do
    schedule(function() update(entity, dt) end)
end

-- No allocation, arguments are passed through
for _, entity in entities do
    schedule(update, entity, dt)
end
```

This is the same reason `pcall(fn, a, b)` beats
`pcall(function() fn(a, b) end)`.

### 8.2 Closures and the upvalue trap

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Works as expected in Luau: every iteration gets a fresh `i`
local fns = {}
for i = 1, 3 do
    fns[i] = function() return i end
end
print(fns[1](), fns[2](), fns[3]())  -- 1 2 3

-- But this does share the same upvalue
local shared = 0
local fns2 = {}
for i = 1, 3 do
    fns2[i] = function() shared += 1 return shared end
end
```

Closures that capture upvalues are not free. In a hot loop that creates
thousands of closures, reuse one function with parameters.

### 8.3 Coroutines for state machines and generators

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Generator pattern
local function range(from: number, to: number, step: number?)
    return coroutine.wrap(function()
        for i = from, to, step or 1 do
            coroutine.yield(i)
        end
    end)
end

for i in range(1, 5) do print(i) end

-- Coroutine status and cleanup
const co = coroutine.create(worker)
print(coroutine.status(co))    -- "suspended" | "running" | "normal" | "dead"
print(coroutine.isyieldable())
coroutine.close(co)            -- force dead, releases resources
```

`coroutine.close` matters when you cancel threads. `task.cancel(thread)` is the
Roblox variant and also works on threads from `task.spawn`.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 9. Error handling

### 9.1 Error levels in `error()`

> ✅ **Live game** &nbsp;`server + client`

The second argument of `error()` is almost never used and is at least as
important as the message itself.

```luau
local function setHealth(amount: number)
    if amount < 0 then
        error("health cannot be negative", 2)
    end
end

setHealth(-5)
```

| Level | Points at |
|---|---|
| `0` | no position info, just the message |
| `1` (default) | the line where `error()` sits |
| `2` | the **caller** |
| `3+` | further up the stack |

With level 1 the user sees `Module:47: health cannot be negative`, meaning your
internal line. With level 2 they see their own line where they called
`setHealth(-5)`. That is what you want in every module other people use.

Level 0 is useful for error objects, because you do not want a prefix there:

```luau
error(setmetatable({ code = 429 }, ErrorMeta), 0)
```

Without level 0, Luau tries to glue position info onto the string and that does
not work nicely on a table.

### 9.2 `assert` returns its arguments

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Instead of
local part = workspace:FindFirstChild("Spawn")
assert(part, "Spawn missing")

-- This works in one line
const part = assert(workspace:FindFirstChild("Spawn"), "Spawn missing")

-- Chaining works too
const humanoid = assert(assert(player.Character, "no character"):FindFirstChildOfClass("Humanoid"), "no humanoid")
```

### 9.3 Error objects instead of strings

> ✅ **Live game** &nbsp;`server + client`

```luau
-- error() accepts any type, not only strings
export type NetworkError = {
    code: "RATE_LIMITED" | "INVALID_PAYLOAD" | "UNAUTHORIZED",
    detail: string,
}

local function validate(payload: unknown)
    if type(payload) ~= "buffer" then
        error({ code = "INVALID_PAYLOAD", detail = "expected buffer" } :: NetworkError, 0)
    end
end

const ok, err = pcall(validate, "not a buffer")
if not ok then
    const e = err :: NetworkError
    if e.code == "INVALID_PAYLOAD" then
        warn(e.detail)
    end
end
```

Note the `0` as the second argument: with a table as the error, level 0 is
required, otherwise Luau tries to prefix position info.

### 9.4 `xpcall` with a traceback

> ✅ **Live game** &nbsp;`server + client`

```luau
const ok, err = xpcall(riskyFunction, function(e)
    return {
        message = tostring(e),
        traceback = debug.traceback(nil, 2),
    }
end, arg1, arg2)

if not ok then
    warn(err.message)
    warn(err.traceback)
end
```

`pcall` gives you only the message. `xpcall` calls the handler **before** the
stack is unwound, so you get a usable traceback. Use this in your networker and
in every `task.spawn` wrapper.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 10. Patterns

### 10.1 OOP class with full type inference

> ✅ **Live game** &nbsp;`server + client`

The common way, and the one that needs no hand-maintained type alias. It has a
tradeoff, covered underneath.

```luau
--!strict

local Vehicle = {}
Vehicle.__index = Vehicle

-- Define the shape through the constructor
function Vehicle.new(speed: number, wheels: number)
    local self = setmetatable({}, Vehicle)
    self.speed = speed
    self.wheels = wheels
    self.distance = 0
    return self
end

-- Now Luau infers the type from the constructor
export type Vehicle = typeof(Vehicle.new(0, 0))

function Vehicle.drive(self: Vehicle, seconds: number)
    self.distance += self.speed * seconds
end

function Vehicle.destroy(self: Vehicle)
    setmetatable(self :: any, nil)
end

return Vehicle
```

Usage:

```luau
local car = Vehicle.new(30, 4)
car:drive(10)
print(car.distance)  -- 300
print(car.speeed)    -- Type error, the typo gets caught
```

Why `function Vehicle.drive(self: Vehicle, ...)` instead of
`function Vehicle:drive(...)`? With dot syntax and an explicit `self`, Luau knows
exactly what type `self` is. You still call it as `car:drive(10)`.

#### What you give up

`typeof(Vehicle.new(0, 0))` reads the type off the implementation, so everything
the constructor assigns lands in the exported type. Add an internal field and it
is public:

```luau
function Vehicle.new(speed: number, wheels: number)
    local self = setmetatable({}, Vehicle)
    self.speed = speed
    self.wheels = wheels
    self.distance = 0
    self._lastTick = 0      -- meant to be internal
    return self
end

export type Vehicle = typeof(Vehicle.new(0, 0))

-- and now this autocompletes for everyone who requires the module
print(car._lastTick)
```

The type is derived from how the class is built, not from what you decided to
expose. That is convenient right up to the point where the two are supposed to
differ, and then it quietly shapes your class around a type you did not write.

If the public surface matters, write that part by hand and keep the inferred
type for the inside:

```luau
type VehicleData = {
    speed: number,
    wheels: number,
    distance: number,
    _lastTick: number,
}

type Self = setmetatable<VehicleData, typeof(Vehicle)>

export type Vehicle = {
    read speed: number,
    read wheels: number,
    read distance: number,
    drive: (Vehicle, seconds: number) -> (),
    destroy: (Vehicle) -> (),
}

function Vehicle.drive(self: Self, seconds: number)
    self.distance += self.speed * seconds
    self._lastTick = os.clock()
end
```

Methods take `Self` and see everything, consumers get `Vehicle` and see only what
you put in it. You pay for it by maintaining the alias, which is exactly what the
inferred version was avoiding.

`setmetatable<Data, typeof(Class)>` is a built-in type function, and declaring
the data type yourself and attaching the metatable in the type is the pattern the
official [Luau OOP guide](https://luau.org/types/object-oriented-programs/) uses.
Worth reading next to this section.

Neither is the right answer everywhere. Inference for small classes and internal
code, a written type once the module is something other people consume and the
difference between public and internal starts to matter.

### 10.2 Inheritance

> ✅ **Live game** &nbsp;`server + client`

```luau
local Truck = setmetatable({}, { __index = Vehicle })
Truck.__index = Truck

function Truck.new(speed: number, capacity: number)
    local self = setmetatable(Vehicle.new(speed, 6), Truck)
    self.capacity = capacity
    return self
end

export type Truck = typeof(Truck.new(0, 0)) & Vehicle.Vehicle

function Truck.load(self: Truck, amount: number)
    self.capacity -= amount
end
```

### 10.3 Enum pattern

> ✅ **Live game** &nbsp;`server + client`

```luau
local GameState = table.freeze({
    Lobby = "Lobby",
    Loading = "Loading",
    Playing = "Playing",
    Ended = "Ended",
})

-- keyof turns the table's keys into a union of string singletons
export type GameStateValue = keyof<typeof(GameState)>
-- "Lobby" | "Loading" | "Playing" | "Ended"

local function transition(from: GameStateValue, to: GameStateValue) end

transition(GameState.Lobby, GameState.Playing)  -- OK
transition("Lobyy", "Playing")                  -- Type error
```

`typeof(GameState.Lobby)` looks like it should give you the same thing and does
not: the field widens to `string`, so the resulting type accepts any string.
`keyof` reads the keys instead, which is why the table uses the same text for
its keys and values.

If you do not need the table at runtime, skip it. A union of string singletons is
already an enum. It autocompletes, it type checks, and there is nothing to keep
in sync:

```luau
type Movement = "Walk" | "Jump" | "Reset"

local function toggle(option: Movement) end

toggle("Walk")   -- autocompletes
toggle("Wlak")   -- Type error
```

Numeric variant when you want to send it compactly over the network:

```luau
local Action = table.freeze({
    Jump = 0,
    Shoot = 1,
    Reload = 2,
})

local ActionNames = table.freeze({ [0] = "Jump", [1] = "Shoot", [2] = "Reload" })
```

### 10.4 Signal / event emitter

> ✅ **Live game** &nbsp;`server + client`

```luau
--!strict

local Signal = {}
Signal.__index = Signal

type Connection = { disconnect: (Connection) -> () }

function Signal.new<A...>()
    local self = setmetatable({}, Signal)
    self._handlers = {} :: {(A...) -> ()}
    return self
end

export type Signal<A...> = typeof(Signal.new())

function Signal.connect<A...>(self: Signal<A...>, handler: (A...) -> ())
    table.insert(self._handlers, handler)

    local connected = true
    return {
        disconnect = function()
            if not connected then return end
            connected = false
            local index = table.find(self._handlers, handler)
            if index then
                table.remove(self._handlers, index)
            end
        end,
    }
end

function Signal.fire<A...>(self: Signal<A...>, ...: A...)
    -- Copy so that disconnecting during fire causes no trouble
    for _, handler in table.clone(self._handlers) do
        task.spawn(handler, ...)
    end
end

return Signal
```

### 10.5 Cleanup pattern (Trove / Maid)

> ✅ **Live game** &nbsp;`server + client`

```luau
local Trove = {}
Trove.__index = Trove

function Trove.new()
    return setmetatable({ _objects = {} }, Trove)
end

export type Trove = typeof(Trove.new())

function Trove.add<T>(self: Trove, object: T): T
    table.insert(self._objects, object)
    return object
end

function Trove.clean(self: Trove)
    for _, obj in self._objects do
        if typeof(obj) == "RBXScriptConnection" then
            obj:Disconnect()
        elseif typeof(obj) == "Instance" then
            obj:Destroy()
        elseif type(obj) == "function" then
            obj()
        elseif type(obj) == "table" and (obj :: any).destroy then
            (obj :: any):destroy()
        end
    end
    table.clear(self._objects)
end
```

Usage:

```luau
local trove = Trove.new()
trove:add(part.Touched:Connect(onTouch))
trove:add(Instance.new("Part"))
trove:add(function() print("cleanup") end)

-- Later, clean everything up at once
trove:clean()
```

### 10.6 Reusing objects (pooling)

> ✅ **Live game** &nbsp;`server + client`

```luau
local Pool = {}
Pool.__index = Pool

function Pool.new<T>(factory: () -> T, reset: (T) -> (), initial: number?)
    local self = setmetatable({}, Pool)
    self._factory = factory
    self._reset = reset
    self._available = table.create(initial or 0)

    for _ = 1, initial or 0 do
        table.insert(self._available, factory())
    end
    return self
end

export type Pool<T> = typeof(Pool.new(function(): T end, function() end))

function Pool.get<T>(self: Pool<T>): T
    local obj = table.remove(self._available)
    if obj then return obj end
    return self._factory()
end

function Pool.release<T>(self: Pool<T>, obj: T)
    self._reset(obj)
    table.insert(self._available, obj)
end
```

For bullets, particles and UI elements: much cheaper than constant
`Instance.new` and `Destroy`.

### 10.7 Memoization

> ✅ **Live game** &nbsp;`server + client`

```luau
local function memoize<K, V>(fn: (K) -> V): (K) -> V
    local cache = {}
    return function(key: K): V
        local cached = cache[key]
        if cached ~= nil then
            return cached
        end
        local result = fn(key)
        cache[key] = result
        return result
    end
end

local expensiveCalc = memoize(function(n: number): number
    task.wait(1)
    return n * n
end)

print(expensiveCalc(5))  -- takes 1 sec
print(expensiveCalc(5))  -- instant
```

### 10.8 Symbols as unique keys

> ✅ **Live game** &nbsp;`server + client`

```luau
local function Symbol(name: string)
    local self = newproxy(true)
    getmetatable(self).__tostring = function()
        return `Symbol({name})`
    end
    return self
end

local NONE = Symbol("None")

-- Now you can distinguish "not set" from "explicitly nil"
local settings = { volume = NONE }
if settings.volume == NONE then
    print("not configured")
end
```

`newproxy(true)` creates a userdata with a metatable. Always unique.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 11. Performance

### 11.1 Localizing globals does not help in Luau

> ✅ **Live game** &nbsp;`server + client`

In vanilla Lua, caching `math.sqrt` in a local before a hot loop is a classic
speedup. In Luau it does nothing:

```luau
-- Both loops run at the same speed
for i = 1, 1e7 do
    local x = math.sqrt(i)
end

local sqrt = math.sqrt
for i = 1, 1e7 do
    local x = sqrt(i)
end
```

Two things make the local pointless. A global chain like `math.sqrt` is resolved
once when the script loads (an *import*), not looked up on every call. And many
builtins, `math.sqrt` included, are *fastcalled*: the VM runs a specialized
implementation directly without setting up a call frame, and it does that whether
you call `math.sqrt` or a local pointing at it. The
[Luau performance guide](https://luau.org/performance) says it plainly: caching
methods in locals is not productive in Luau and not recommended.

The exception is [11.4](#114-getfenvsetfenv-destroy-your-performance). `getfenv`
and `setfenv` mark the environment impure, which switches imports and fastcalls
off for the whole script. Localizing helps again at that point, but the real fix
is removing the `getfenv`.

### 11.2 Preallocate tables

> ✅ **Live game** &nbsp;`server + client`

```luau
-- Slow: the table grows and rehashes several times
local t = {}
for i = 1, 10000 do t[i] = i end

-- Faster
local t = table.create(10000)
for i = 1, 10000 do t[i] = i end
```

### 11.3 `#t` in a loop condition is fine

> ✅ **Live game** &nbsp;`server + client`

A common piece of advice is to cache the length before a numeric loop. It does
not apply:

```luau
for i = 1, #list do end
```

The limit of a numeric `for` is evaluated once, before the first iteration, not on
every pass. On top of that, Luau caches table lengths and keeps them current
through `table.insert` and `table.remove`, so `#t` is close to constant time
anyway.

What does matter is changing the table inside that loop. The limit was captured at
the start, so removing items skips elements and reads past the new end:

```luau
for i = 1, #list do
    if shouldRemove(list[i]) then
        table.remove(list, i)   -- the next item slides into i and gets skipped
    end
end
```

Iterate backwards for that, as in [6.5](#65-swap-remove-for-unordered-lists).

### 11.4 `getfenv`/`setfenv` destroy your performance

> ✅ **Live game** &nbsp;`server + client`

```luau
-- DO NOT
local function slow()
    getfenv()  -- disables all global optimizations for this script
end
```

> [!CAUTION]
> The moment Luau sees `getfenv` or `setfenv`, it has to assume every global can
> change and it can no longer optimize a single global lookup. This applies to the
> whole script, not just that function.

### 11.5 Benchmarking

> ✅ **Live game** &nbsp;`server + client`

```luau
local function benchmark(name: string, iterations: number, fn: () -> ())
    -- Warmup
    for _ = 1, 100 do fn() end

    local start = os.clock()
    for _ = 1, iterations do
        fn()
    end
    local elapsed = os.clock() - start

    print(string.format(
        "%s: %.4fs total, %.4fµs per call",
        name, elapsed, (elapsed / iterations) * 1e6
    ))
end

benchmark("table.create", 100000, function()
    local t = table.create(100)
end)
```

Use `os.clock()`, not `tick()` or `os.time()`. `os.clock` has the highest
precision.

### 11.6 When `--!native` pays off and when it does not

> ⚠️ **Live game** &nbsp;`server, officially` &nbsp;— documented as server side. There are reports of it being enabled on Android clients, but no announcement. Studio compiles LocalScripts natively either way, so a Studio benchmark tells you nothing about what players get.

Yes: math-heavy code, physics, procedural generation, image processing,
pathfinding, compression.

No: scripts that mostly wait on events, DataStore calls or UI updates. Native
compilation costs startup time and buys nothing there.

### 11.7 `_G` and `shared`

> ✅ **Live game** &nbsp;`server + client`

```luau
_G.Something = 5
shared.Other = 10
```

Two separate global tables, per VM. Server and client share nothing. They are not
type-safe, autocomplete does not work, and you have no idea who writes what.

Use ModuleScripts. The only decent use case for `_G` is quick debugging in the
command bar.

### 11.8 Measuring memory

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `gcinfo` and the memory categories are visible in the live Developer Console too (F9).

```luau
print(gcinfo())                    -- KB in use, Roblox specific
print(collectgarbage("count"))     -- same, in KB

-- Roblox only supports "count" as an option, not "collect" or "step"
```

Categories for the Developer Console memory tab:

```luau
debug.setmemorycategory("ProjectileSystem")
-- everything this thread allocates from here on falls under this category
debug.resetmemorycategory()
```

Indispensable when tracking down leaks in a large project.

### 11.9 Luau limits

> ✅ **Live game** &nbsp;`server + client`

Relevant if you generate code or inline very large data tables:

| Limit | Value |
|---|---|
| Locals per function | 200 (including parameters) |
| Upvalues per function | 200 |
| Registers per function | 255 |
| Constants per function | 2^23 |
| Instructions per function | 1,000,000,000 |

Values from the Luau compiler source. If you run into "too many local
variables", split the function up. Large lookup
tables belong in a ModuleScript, not inline in a function.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 12. Buffers and network code

### 12.1 The buffer library

> ✅ **Live game** &nbsp;`server + client`

Binary data, far more memory efficient than tables. Perfect for network packets
and large datasets.

```luau
local buf = buffer.create(64)   -- 64 bytes

-- Writing (offset in bytes)
buffer.writeu8(buf, 0, 255)       -- unsigned 8-bit
buffer.writei8(buf, 1, -128)      -- signed 8-bit
buffer.writeu16(buf, 2, 65535)
buffer.writei32(buf, 4, -100000)
buffer.writef32(buf, 8, 3.14)
buffer.writef64(buf, 12, 3.14159265)
buffer.writestring(buf, 20, "hello")

-- Reading
local a = buffer.readu8(buf, 0)
local b = buffer.readf32(buf, 8)
local s = buffer.readstring(buf, 20, 5)   -- offset, length

-- Utility
print(buffer.len(buf))
buffer.fill(buf, 0, 0, 64)                -- fill with 0
buffer.copy(dest, 0, source, 0, 32)
local str = buffer.tostring(buf)
local fromStr = buffer.fromstring("data")
```

Practical example, compact player state:

```luau
-- 9 bytes instead of a table with 3 keys
local function packState(x: number, y: number, health: number): buffer
    local b = buffer.create(9)
    buffer.writef32(b, 0, x)
    buffer.writef32(b, 4, y)
    buffer.writeu8(b, 8, health)
    return b
end

local function unpackState(b: buffer): (number, number, number)
    return buffer.readf32(b, 0), buffer.readf32(b, 4), buffer.readu8(b, 8)
end
```

Buffers can be sent over RemoteEvents and are much cheaper there than tables.

### 12.2 Why it matters

> ✅ **Live game** &nbsp;`server + client`

Roblox serializes Lua values with serious overhead. Rough indication per value
over a remote:

| Type | Cost |
|---|---|
| number | ~9 bytes (8 data + 1 type tag) |
| boolean | ~2 bytes |
| string | ~2 bytes + length |
| Vector3 | ~13 bytes |
| CFrame | ~25+ bytes |
| table | ~2 bytes overhead + per key/value |

A table `{x = 1.5, y = 2.5, z = 3.5}` easily costs 40+ bytes. The same data in a
buffer with float32: **12 bytes**. With quantization to int16: **6 bytes**.

For a game with 30 players receiving position updates 20 times per second, that
is the difference between smooth and unplayable.

### 12.3 UnreliableRemoteEvent

> ✅ **Live game** &nbsp;`server + client` &nbsp;— fully rolled out, no longer a beta.

```luau
const unreliable = Instance.new("UnreliableRemoteEvent")
```

Use this for data that is refreshed every frame: positions, rotations, animation
state, health bars. If a packet is lost, the next update arrives anyway.

Rules:
- No delivery guarantee
- No ordering guarantee
- A payload above 1000 bytes is **silently dropped** (in Studio you get a warning
  telling you how far over you went, in the real client you do not)
- Roblox compresses buffers before sending, so you cannot reliably measure the
  payload size up front. Aim well under 900 bytes per fire

Use a normal `RemoteEvent` for everything that must arrive exactly once:
purchases, damage, inventory changes, chat.

### 12.4 BufferWriter and BufferReader

> ✅ **Live game** &nbsp;`server + client`

Buffers cannot grow, so you need a wrapper.

```luau
--!strict
--!optimize 2

const Writer = {}
Writer.__index = Writer

function Writer.new(initialSize: number?)
    const self = setmetatable({}, Writer)
    self.buf = buffer.create(initialSize or 64)
    self.cursor = 0
    return self
end

export type Writer = typeof(Writer.new())

function Writer.ensure(self: Writer, bytes: number)
    const needed = self.cursor + bytes
    const size = buffer.len(self.buf)
    if needed <= size then return end

    local newSize = size * 2
    while newSize < needed do
        newSize *= 2
    end

    const newBuf = buffer.create(newSize)
    buffer.copy(newBuf, 0, self.buf, 0, self.cursor)
    self.buf = newBuf
end

function Writer.u8(self: Writer, value: number)
    self:ensure(1)
    buffer.writeu8(self.buf, self.cursor, value)
    self.cursor += 1
end

function Writer.u16(self: Writer, value: number)
    self:ensure(2)
    buffer.writeu16(self.buf, self.cursor, value)
    self.cursor += 2
end

function Writer.i16(self: Writer, value: number)
    self:ensure(2)
    buffer.writei16(self.buf, self.cursor, value)
    self.cursor += 2
end

function Writer.u32(self: Writer, value: number)
    self:ensure(4)
    buffer.writeu32(self.buf, self.cursor, value)
    self.cursor += 4
end

function Writer.f32(self: Writer, value: number)
    self:ensure(4)
    buffer.writef32(self.buf, self.cursor, value)
    self.cursor += 4
end

function Writer.f64(self: Writer, value: number)
    self:ensure(8)
    buffer.writef64(self.buf, self.cursor, value)
    self.cursor += 8
end

function Writer.string(self: Writer, value: string)
    const length = #value
    assert(length < 65536, "string too long for a u16 prefix")
    self:u16(length)
    self:ensure(length)
    buffer.writestring(self.buf, self.cursor, value)
    self.cursor += length
end

function Writer.boolean(self: Writer, value: boolean)
    self:u8(if value then 1 else 0)
end

-- Returns a buffer that is exactly as large as needed
function Writer.finish(self: Writer): buffer
    const out = buffer.create(self.cursor)
    buffer.copy(out, 0, self.buf, 0, self.cursor)
    return out
end

return Writer
```

```luau
--!strict
--!optimize 2

const Reader = {}
Reader.__index = Reader

function Reader.new(buf: buffer)
    const self = setmetatable({}, Reader)
    self.buf = buf
    self.cursor = 0
    self.size = buffer.len(buf)
    return self
end

export type Reader = typeof(Reader.new(buffer.create(1)))

-- IMPORTANT: always bounds check, this is data from the client
function Reader.check(self: Reader, bytes: number)
    if self.cursor + bytes > self.size then
        error("buffer overread: corrupt or malicious payload", 0)
    end
end

function Reader.u8(self: Reader): number
    self:check(1)
    const value = buffer.readu8(self.buf, self.cursor)
    self.cursor += 1
    return value
end

function Reader.u16(self: Reader): number
    self:check(2)
    const value = buffer.readu16(self.buf, self.cursor)
    self.cursor += 2
    return value
end

function Reader.i16(self: Reader): number
    self:check(2)
    const value = buffer.readi16(self.buf, self.cursor)
    self.cursor += 2
    return value
end

function Reader.u32(self: Reader): number
    self:check(4)
    const value = buffer.readu32(self.buf, self.cursor)
    self.cursor += 4
    return value
end

function Reader.f32(self: Reader): number
    self:check(4)
    const value = buffer.readf32(self.buf, self.cursor)
    self.cursor += 4
    return value
end

function Reader.f64(self: Reader): number
    self:check(8)
    const value = buffer.readf64(self.buf, self.cursor)
    self.cursor += 8
    return value
end

function Reader.string(self: Reader): string
    const length = self:u16()
    self:check(length)
    const value = buffer.readstring(self.buf, self.cursor, length)
    self.cursor += length
    return value
end

function Reader.boolean(self: Reader): boolean
    return self:u8() ~= 0
end

function Reader.atEnd(self: Reader): boolean
    return self.cursor >= self.size
end

return Reader
```

> [!CAUTION]
> The `check` function is not optional. Without bounds checking an exploiter can
> send a short buffer and cause errors or undefined behavior on your server.

### 12.5 Quantization: compressing floats

> ✅ **Live game** &nbsp;`server + client`

A position rarely needs float64 precision. If your map is 2048 studs across and
1cm precision is enough, that fits in 16 bits.

```luau
const MAP_SIZE = 2048
const HALF = MAP_SIZE / 2

-- 4 bytes -> 2 bytes per axis
local function quantizePosition(value: number): number
    const clamped = math.clamp(value, -HALF, HALF)
    return math.round((clamped + HALF) / MAP_SIZE * 65535)
end

local function dequantizePosition(value: number): number
    return (value / 65535) * MAP_SIZE - HALF
end

-- Precision: 2048 / 65535 = 0.031 studs. Fine for visual replication.
```

For rotation, angles are always 0 to 2pi:

```luau
local function quantizeAngle(radians: number): number
    return math.round((radians % (math.pi * 2)) / (math.pi * 2) * 65535)
end

local function dequantizeAngle(value: number): number
    return (value / 65535) * math.pi * 2
end

-- Precision: 0.005 degrees. More than enough.
```

For normalized floats (health percentage, alpha values) 1 byte is often enough:

```luau
local function quantizeUnit(value: number): number   -- 0..1 to 0..255
    return math.round(math.clamp(value, 0, 1) * 255)
end

local function dequantizeUnit(value: number): number
    return value / 255
end
```

### 12.6 Compressing CFrames

> ✅ **Live game** &nbsp;`server + client`

A full CFrame is 12 floats. For character replication you usually only need
position plus Y rotation:

```luau
-- 8 bytes instead of 48+
local function writeCharacterCFrame(writer: Writer.Writer, cf: CFrame)
    const pos = cf.Position
    writer:u16(quantizePosition(pos.X))
    writer:u16(quantizePosition(pos.Y))
    writer:u16(quantizePosition(pos.Z))

    const _, yaw = cf:ToEulerAnglesYXZ()
    writer:u16(quantizeAngle(yaw))
end

local function readCharacterCFrame(reader: Reader.Reader): CFrame
    const x = dequantizePosition(reader:u16())
    const y = dequantizePosition(reader:u16())
    const z = dequantizePosition(reader:u16())
    const yaw = dequantizeAngle(reader:u16())

    return CFrame.new(x, y, z) * CFrame.Angles(0, yaw, 0)
end
```

If you need full rotation, use axis-angle (4 values) or a quaternion with the
"smallest three" trick (3 components plus 2 bits for which component you left
out).

### 12.7 Packing bits

> ✅ **Live game** &nbsp;`server + client`

Eight booleans in one byte:

```luau
local function packFlags(flags: {boolean}): number
    local result = 0
    for i, flag in flags do
        if flag then
            result = bit32.bor(result, bit32.lshift(1, i - 1))
        end
    end
    return result
end

local function unpackFlags(packed: number, count: number): {boolean}
    const out = table.create(count)
    for i = 1, count do
        out[i] = bit32.band(packed, bit32.lshift(1, i - 1)) ~= 0
    end
    return out
end

-- Practical: player input state in 1 byte
const InputFlags = table.freeze({
    Forward  = 0b0000_0001,
    Backward = 0b0000_0010,
    Left     = 0b0000_0100,
    Right    = 0b0000_1000,
    Jump     = 0b0001_0000,
    Sprint   = 0b0010_0000,
    Crouch   = 0b0100_0000,
    Fire     = 0b1000_0000,
})
```

### 12.8 Varint encoding

> ✅ **Live game** &nbsp;`server + client`

For numbers that are usually small but occasionally large (item counts, IDs):

```luau
local function writeVarint(writer: Writer.Writer, value: number)
    assert(value >= 0 and value % 1 == 0, "varint requires a non-negative integer")
    while value >= 0x80 do
        writer:u8(bit32.bor(bit32.band(value, 0x7F), 0x80))
        value = bit32.rshift(value, 7)
    end
    writer:u8(value)
end

local function readVarint(reader: Reader.Reader): number
    local result = 0
    local shift = 0
    while true do
        const byte = reader:u8()
        result = bit32.bor(result, bit32.lshift(bit32.band(byte, 0x7F), shift))
        if bit32.band(byte, 0x80) == 0 then break end
        shift += 7
        if shift > 28 then error("varint too long", 0) end
    end
    return result
end
```

Numbers below 128 cost 1 byte, below 16384 cost 2 bytes, and so on.

### 12.9 Packet registry with type safety

> ✅ **Live game** &nbsp;`server + client`

```luau
--!strict
-- ReplicatedStorage/Net/Packets.luau

const Writer = require(script.Parent.Writer)
const Reader = require(script.Parent.Reader)

export type PacketDefinition<T> = {
    id: number,
    unreliable: boolean,
    write: (Writer.Writer, T) -> (),
    read: (Reader.Reader) -> T,
}

export type MovePayload = { x: number, y: number, z: number, yaw: number }
export type DamagePayload = { targetId: number, amount: number }

const Packets = {}

Packets.Move = {
    id = 1,
    unreliable = true,
    write = function(w: Writer.Writer, data: MovePayload)
        w:u16(quantizePosition(data.x))
        w:u16(quantizePosition(data.y))
        w:u16(quantizePosition(data.z))
        w:u16(quantizeAngle(data.yaw))
    end,
    read = function(r: Reader.Reader): MovePayload
        return {
            x = dequantizePosition(r:u16()),
            y = dequantizePosition(r:u16()),
            z = dequantizePosition(r:u16()),
            yaw = dequantizeAngle(r:u16()),
        }
    end,
} :: PacketDefinition<MovePayload>

Packets.Damage = {
    id = 2,
    unreliable = false,
    write = function(w: Writer.Writer, data: DamagePayload)
        w:f64(data.targetId)
        w:u16(data.amount)
    end,
    read = function(r: Reader.Reader): DamagePayload
        return { targetId = r:f64(), amount = r:u16() }
    end,
} :: PacketDefinition<DamagePayload>

return table.freeze(Packets)
```

### 12.10 Batching, critical for performance

> ✅ **Live game** &nbsp;`server + client`

One remote call per frame per player is far cheaper than ten separate calls.
Roblox has fixed overhead per remote fire.

```luau
--!strict
-- Server: collect everything and flush once per frame

const RunService = game:GetService("RunService")
const Writer = require(ReplicatedStorage.Net.Writer)

const queues: {[Player]: {{id: number, write: (Writer.Writer) -> ()}}} = {}

local function queue(player: Player, packetId: number, writeFn: (Writer.Writer) -> ())
    local q = queues[player]
    if not q then
        q = {}
        queues[player] = q
    end
    table.insert(q, { id = packetId, write = writeFn })
end

RunService.Heartbeat:Connect(function()
    for player, q in queues do
        if #q == 0 then continue end

        const writer = Writer.new(256)
        writer:u8(#q)   -- number of packets in this batch

        for _, entry in q do
            writer:u8(entry.id)
            entry.write(writer)
        end

        unreliableRemote:FireClient(player, writer:finish())
        table.clear(q)
    end
end)
```

The client reads the batch:

```luau
unreliableRemote.OnClientEvent:Connect(function(buf: buffer)
    const reader = Reader.new(buf)
    const count = reader:u8()

    for _ = 1, count do
        const id = reader:u8()
        const handler = handlers[id]
        if not handler then
            warn(`Unknown packet id: {id}`)
            return   -- stop, we do not know the length and cannot keep reading
        end
        handler(reader)
    end
end)
```

### 12.11 Delta compression

> ✅ **Live game** &nbsp;`server + client`

Only send what changed.

```luau
const lastSent: {[Player]: {x: number, y: number, z: number}} = {}

local function shouldSendPosition(player: Player, pos: Vector3): boolean
    const last = lastSent[player]
    if not last then return true end

    const threshold = 0.05
    return math.abs(pos.X - last.x) > threshold
        or math.abs(pos.Y - last.y) > threshold
        or math.abs(pos.Z - last.z) > threshold
end
```

Combine it with a dirty-flag bitmask: one byte saying which fields are in this
packet.

```luau
const DirtyFlags = table.freeze({
    Position = 0b0001,
    Rotation = 0b0010,
    Health   = 0b0100,
    State    = 0b1000,
})

local function writeEntity(w: Writer.Writer, entity, dirty: number)
    w:u8(dirty)
    if bit32.band(dirty, DirtyFlags.Position) ~= 0 then
        w:u16(quantizePosition(entity.x))
        w:u16(quantizePosition(entity.y))
        w:u16(quantizePosition(entity.z))
    end
    if bit32.band(dirty, DirtyFlags.Health) ~= 0 then
        w:u8(quantizeUnit(entity.health / entity.maxHealth))
    end
end
```

### 12.12 Server-side safety

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— by definition this belongs nowhere else.

> [!CAUTION]
> Rule one: the client always lies. Every byte that comes in is hostile.

```luau
--!strict

const RATE_LIMIT = 30          -- max calls per second
const RATE_WINDOW = 1

const callCounts: {[Player]: {count: number, windowStart: number}} = {}

local function checkRateLimit(player: Player): boolean
    const now = os.clock()
    local entry = callCounts[player]

    if not entry or now - entry.windowStart > RATE_WINDOW then
        callCounts[player] = { count = 1, windowStart = now }
        return true
    end

    entry.count += 1
    if entry.count > RATE_LIMIT then
        return false
    end
    return true
end

remote.OnServerEvent:Connect(function(player: Player, payload: unknown)
    -- 1. Rate limit
    if not checkRateLimit(player) then
        warn(`Rate limit exceeded by {player.UserId}`)
        return
    end

    -- 2. Type check
    if type(payload) ~= "buffer" then
        warn(`Invalid payload type from {player.UserId}`)
        return
    end

    -- 3. Size check before you start parsing
    if buffer.len(payload) > 256 then
        warn(`Payload too large from {player.UserId}`)
        return
    end

    -- 4. Parse inside a pcall, because the Reader can throw
    const ok, result = pcall(function()
        const reader = Reader.new(payload)
        return Packets.Move.read(reader)
    end)

    if not ok then
        warn(`Corrupt payload from {player.UserId}: {result}`)
        return
    end

    -- 5. Validate the values themselves
    const move = result :: Packets.MovePayload
    if move.x ~= move.x then return end   -- NaN check
    if math.abs(move.x) > 1024 then return end

    -- 6. Validate against server state
    const character = player.Character
    if not character then return end
    const root = character:FindFirstChild("HumanoidRootPart") :: BasePart?
    if not root then return end

    const distance = (Vector3.new(move.x, move.y, move.z) - root.Position).Magnitude
    if distance > MAX_MOVE_PER_TICK then
        -- teleport back, do not trust the client
        return
    end

    applyMove(player, move)
end)
```

Clean up on leave, otherwise your rate limit table leaks:

```luau
Players.PlayerRemoving:Connect(function(player)
    callCounts[player] = nil
    lastSent[player] = nil
    queues[player] = nil
end)
```

### 12.13 Time synchronization

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `GetServerTimeNow` is synchronized on both.

For lag compensation you need a shared clock.

```luau
-- Works identically on server and client, synchronized by the engine
const now = workspace:GetServerTimeNow()
```

This is the right source for timestamps in packets. `os.time()` only has second
precision and `os.clock()` is per machine.

```luau
-- In the packet
writer:f64(workspace:GetServerTimeNow())

-- On receipt
const sentAt = reader:f64()
const latency = workspace:GetServerTimeNow() - sentAt
```

### 12.14 Avoid RemoteFunction on the server

> 🖥️ **Live game** &nbsp;`server only`

```luau
-- DANGEROUS: an exploiter can yield here forever and block your server thread
remoteFunction.OnServerInvoke = function(player, ...)
    return doSomething(...)
end
```

A client that never answers an `InvokeClient` blocks your server thread
permanently. Always use two RemoteEvents with a request id:

```luau
-- Request
requestRemote:FireServer(requestId, packetBuffer)

-- Response
responseRemote.OnClientEvent:Connect(function(requestId, resultBuffer)
    const resolve = pending[requestId]
    if resolve then
        pending[requestId] = nil
        resolve(resultBuffer)
    end
end)
```

With a timeout on top, so pending never grows forever.

### 12.15 Type-safe RemoteEvents

> ✅ **Live game** &nbsp;`server + client`

```luau
--!strict
-- ReplicatedStorage/Remotes.luau

export type RemotePayloads = {
    PurchaseItem: (itemId: string, quantity: number) -> (),
    UpdateHealth: (health: number) -> (),
}

local Remotes = {}

function Remotes.fireServer<K>(name: K & string, ...)
    local remote = ReplicatedStorage.Remotes:FindFirstChild(name)
    assert(remote, `Remote {name} does not exist`)
    remote:FireServer(...)
end

return Remotes
```

For full type safety across the network boundary, libraries like ByteNet or
Blink are worth considering. They use buffers under the hood.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 13. Roblox API

### 13.1 The `task` library, not `spawn`/`delay`/`wait`

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `task.synchronize` and `task.desynchronize` only make sense inside an Actor.

```luau
task.spawn(fn, ...)        -- immediately, in a new thread
task.defer(fn, ...)        -- at the end of the current resumption cycle
task.delay(seconds, fn)    -- after X seconds
task.wait(seconds)         -- yield, returns the elapsed time
task.synchronize()         -- back to serial execution (parallel Luau)
task.desynchronize()       -- into parallel execution
task.cancel(thread)        -- cancel a thread
```

The old globals (`spawn`, `delay`, `wait`) have throttling and unreliable
timing. Do not use them any more.

`task.defer` is the answer to "I want this to happen after all current code is
done", without an arbitrary `task.wait()`.

### 13.2 Require by string

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `./`, `../` and `@self` work everywhere, `@game` since January 2026. Custom aliases from `.luaurc` are not in the Roblox engine yet.

Roblox has always had require by **instance**: you hand `require` an actual
ModuleScript object, and you get to that object by walking the DataModel with
property lookups. Luau also lets you hand `require` a **string path**, which the
engine resolves for you at require time.

```luau
const ServerScriptService = game:GetService("ServerScriptService")

-- Instance require: you build a reference to the object first
const Inventory = require(ServerScriptService.Modules.Inventory)

-- String require: you describe where it lives
const Inventory = require("@game/ServerScriptService/Modules/Inventory")
```

Both give you the same cached module. The difference is everything that happens
*before* the require: in the instance form each `.Name` in the chain is a real
property lookup on a real instance that has to exist at that exact moment.

#### The four prefixes

Every string path is relative to something. There is no implicit root, so the
path always starts with one of these.

| Prefix | Resolves from | Instance equivalent |
|---|---|---|
| `./` | the script's parent, so its sibling folder | `script.Parent` |
| `../` | one level above that, and it chains | `script.Parent.Parent` |
| `@self/` | the script's own children | `script` |
| `@game/` | the root of the DataModel | `game` |

```luau
-- ServerScriptService/Systems/Combat  (a Script, with a child Config)
require("./Ballistics")            -- ServerScriptService.Systems.Ballistics
require("../Modules/Inventory")    -- ServerScriptService.Modules.Inventory
require("@self/Config")            -- the Config inside Combat itself
require("@game/ReplicatedStorage/Shared/Signal")
```

#### Translating what you already have

| Instance require | String require |
|---|---|
| `require(ServerScriptService.Modules.Inventory)` | `require("@game/ServerScriptService/Modules/Inventory")` |
| `require(ReplicatedStorage.Shared.Signal)` | `require("@game/ReplicatedStorage/Shared/Signal")` |
| `require(script.Parent.Config)` | `require("./Config")` |
| `require(script.Parent.Parent.Shared.Types)` | `require("../Shared/Types")` |
| `require(script.Config)` | `require("@self/Config")` |
| `require(workspace.Map.Logic)` | `require("@game/Workspace/Map/Logic")` |

Note the last one: inside a `@game/` path you spell the service out as it is
named in the DataModel, so `Workspace`, not the `workspace` global.

#### The rules

- **Always a prefix.** `require("Modules/Inventory")` is an error. Absolute paths
  without `@game` do not exist
- **Slashes, not dots.** A `.` is part of a name, not a step down the tree
- **No file extension.** There are no files in the engine, so it is
  `require("./Signal")` and never `require("./Signal.luau")`
- **Case sensitive.** `@game/replicatedstorage/Shared` does not resolve
- **Literal strings only.** Write the path out. The moment you build it at
  runtime you lose autocomplete, go-to-definition, the return type of the module
  and every Rojo-aware tool that follows your requires

```luau
-- Fine
const Signal = require("@game/ReplicatedStorage/Shared/Signal")

-- Technically a string, practically useless: nothing can follow this
const name = "Signal"
const Signal = require(`@game/ReplicatedStorage/Shared/{name}`)
```

> [!WARNING]
> Require-by-string does not wait for a ModuleScript to replicate. On the client,
> use `game.Loaded:Wait()` or `WaitForChild` ([13.6](#136-waitforchild-with-a-timeout))
> first. The string is resolved at the
> moment the require runs, exactly like an instance path would be, so on a client
> script that runs early the target simply is not there yet.

#### What it does not change

- It does not create anything. If the path does not resolve, it errors
- It does not bypass the module cache. Requiring the same module by string and by
  instance gives you the same single cached result ([13.22](#1322-modules-lifetime-and-require-semantics))
- It does not fix recursive requires. A require cycle is still a cycle
- It has nothing to do with streaming. `@game/Workspace/...` on the client is
  subject to the same streaming rules as everything else ([13.21](#1321-instance-streaming))

#### Why bother

- **Your file tree and your requires line up.** With Rojo, `src/Shared/Signal.luau`
  is `require("../Shared/Signal")` from a sibling folder. The path in the editor
  and the path in the code are the same path
- **No service preamble.** Five `game:GetService` lines at the top of a file exist
  purely to make the requires below them readable. String paths do not need them
- **Moving a folder is a find and replace.** Moving one in instance form means
  rewriting chains that were spelled differently in every file
- **It survives outside Roblox.** Lune, Lute and luau-lsp all understand `./` and
  `../`. A module that requires only that way can be unit tested outside the
  engine ([14.6](#146-testing))

#### `.luaurc` and `.config.luau` aliases

Outside the engine you can define your own aliases, in a `.luaurc` or in the same
settings written as Luau in a `.config.luau` ([14.4](#144-tooling)):

```json
{
  "aliases": {
    "shared": "src/Shared",
    "net": "src/Net"
  }
}
```

```luau
const Signal = require("@shared/Signal")
```

This works in Lune, Lute and luau-lsp. The Roblox engine does not read
either file's aliases yet, so a module that uses them runs in your tooling and
breaks in the game. If you want one module to work in both places, stay on `./`
and `../`, which are the only forms every runtime agrees on.

### 13.3 Attributes with types

> ✅ **Live game** &nbsp;`server + client` &nbsp;— you set them on the server, the client reads along. Attributes set client side stay local.

```luau
part:SetAttribute("Damage", 25)
local damage = part:GetAttribute("Damage") :: number?

part:GetAttributeChangedSignal("Damage"):Connect(function()
    print("Damage changed to", part:GetAttribute("Damage"))
end)

-- All attributes
for name, value in part:GetAttributes() do
    print(name, value)
end
```

Attributes replicate automatically from server to client and are visible in
Studio's Properties panel. Usually better than hidden ValueObjects.

### 13.4 CollectionService tags

> ✅ **Live game** &nbsp;`server + client` &nbsp;— tags you set on the client do not replicate to the server.

```luau
local CollectionService = game:GetService("CollectionService")

CollectionService:AddTag(part, "Damageable")

for _, tagged in CollectionService:GetTagged("Damageable") do
    setup(tagged)
end

CollectionService:GetInstanceAddedSignal("Damageable"):Connect(setup)
CollectionService:GetInstanceRemovedSignal("Damageable"):Connect(cleanup)
```

Much cleaner than `WaitForChild` and hardcoded paths everywhere.

### 13.5 Tags on the instance itself

> ✅ **Live game** &nbsp;`server + client` &nbsp;— same replication rule as above.

Faster than CollectionService and much shorter.

```luau
part:AddTag("Damageable")
print(part:HasTag("Damageable"))   -- true
part:RemoveTag("Damageable")
print(part:GetTags())              -- {"Damageable"}
```

CollectionService is still needed for `GetTagged` and the signals.

### 13.6 `WaitForChild` with a timeout

> ✅ **Live game** &nbsp;`server + client` &nbsp;— on the client it is often mandatory, on the server almost never needed.

```luau
-- Waits forever. On a typo your script silently stalls with no error.
const remotes = ReplicatedStorage:WaitForChild("Remotes")

-- Waits at most 10 seconds, returns nil on timeout
const remotes = ReplicatedStorage:WaitForChild("Remotes", 10)
if not remotes then
    error("Remotes missing after 10 seconds")
end
```

> [!WARNING]
> Without a timeout you only get an *Infinite yield possible* warning in the output
> after 5 seconds, and then you keep waiting. That is one of the hardest bugs to
> find, because there is no error.

On the server you almost never need `WaitForChild`: everything already exists
when the script loads.

### 13.7 `:Changed` is inconsistent

> ✅ **Live game** &nbsp;`server + client`

```luau
-- On a regular Instance: gives you the property NAME
part.Changed:Connect(function(property: string)
    print(property)   -- "Position"
end)

-- On a ValueObject: gives you the new VALUE
intValue.Changed:Connect(function(value: number)
    print(value)      -- 42
end)
```

This is historical baggage. Use `GetPropertyChangedSignal` and you never deal
with it:

```luau
part:GetPropertyChangedSignal("Position"):Connect(function()
    print(part.Position)
end)
```

It is cheaper too: `.Changed` fires for every property, the specific signal only
for that one.

### 13.8 Instance lookups you are probably missing

> ✅ **Live game** &nbsp;`server + client`

```luau
workspace:FindFirstChild("Name", true)          -- true = recursive
workspace:FindFirstChildOfClass("Camera")
workspace:FindFirstChildWhichIsA("BasePart")    -- respects inheritance
part:FindFirstAncestorOfClass("Model")
part:FindFirstAncestorWhichIsA("PVInstance")
```

`FindFirstChildOfClass` matches ClassName exactly.
`FindFirstChildWhichIsA` also matches subclasses, so a `Part` matches
`BasePart`. That difference costs people a lot of time.

### 13.9 RunService events, picking the right one

> ⚠️ **Live game** &nbsp;`server + client` &nbsp;— but `PreRender`, `PreAnimation` and `BindToRenderStep` only exist on the client.

Order per frame:

| Event | When | Where |
|---|---|---|
| `PreRender` | just before rendering | client |
| `PreAnimation` | before animation evaluation | client |
| `PreSimulation` | before the physics step | both |
| `PostSimulation` | after the physics step | both |

The old names `RenderStepped`, `Stepped` and `Heartbeat` still work but the new
ones are clearer. Camera code belongs in `PreRender`, physics related logic in
`PreSimulation` or `PostSimulation`.

```luau
-- With explicit priority relative to the camera and input
const RunService = game:GetService("RunService")

RunService:BindToRenderStep("CustomCamera", Enum.RenderPriority.Camera.Value - 1, function(dt)
    updateCamera(dt)
end)

RunService:UnbindFromRenderStep("CustomCamera")
```

Priorities: `First` (0), `Input` (100), `Camera` (200), `Character` (300),
`Last` (2000).

### 13.10 Raycasts and shapecasts

> ✅ **Live game** &nbsp;`server + client` &nbsp;— on the client you only hit what is streamed in, see [13.21](#1321-instance-streaming).

```luau
const params = RaycastParams.new()
params.FilterType = Enum.RaycastFilterType.Exclude
params.FilterDescendantsInstances = { character }
params.IgnoreWater = true
params.RespectCanCollide = true
params.CollisionGroup = "Projectiles"

const result = workspace:Raycast(origin, direction * 500, params)
if result then
    print(result.Instance, result.Position, result.Normal, result.Distance, result.Material)
end

-- Volume casts
workspace:Blockcast(cframe, size, direction, params)
workspace:Spherecast(origin, radius, direction, params)
workspace:Shapecast(part, direction, params)
```

> [!TIP]
> Create `RaycastParams` **once** and reuse it. Calling `RaycastParams.new()`
> inside a loop is waste.

```luau
-- Update the filter without new params
params:AddToFilter(newPart)
```

### 13.11 Overlap queries

> ✅ **Live game** &nbsp;`server + client` &nbsp;— same streaming caveat as with raycasts.

```luau
const overlapParams = OverlapParams.new()
overlapParams.FilterType = Enum.RaycastFilterType.Include
overlapParams.FilterDescendantsInstances = { workspace.Enemies }
overlapParams.MaxParts = 20

const parts = workspace:GetPartBoundsInBox(cframe, size, overlapParams)
const inRadius = workspace:GetPartBoundsInRadius(position, 25, overlapParams)
const inPart = workspace:GetPartsInPart(hitbox, overlapParams)
```

Setting `MaxParts` prevents a single query from returning thousands of parts.

### 13.12 Allocation-free constants

> ✅ **Live game** &nbsp;`server + client`

Roblox has named constants for the values you reach for most:

```luau
Vector3.zero
Vector3.one
Vector3.xAxis
Vector3.yAxis
Vector3.zAxis

Vector2.zero
Vector2.one

CFrame.identity

Color3.new()  -- still an allocation, cache this yourself
```

`Vector3` is a native value type ([5.16](#516-the-vector-library)), so
`Vector3.new(0, 0, 0)` does not allocate and `Vector3.zero` is a readability win,
not a speed one. `CFrame`, `Vector2` and `Color3` are still userdata: every `.new`
is a real allocation, so `CFrame.identity` and a cached `Color3` do save work in a
hot loop.

### 13.13 Network ownership

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— the client cannot set ownership, only hold it.

```luau
-- Server keeps control (important for anti-cheat on things that matter)
part:SetNetworkOwner(nil)

-- Client gets control (smoother for their own character/vehicle)
part:SetNetworkOwner(player)

print(part:GetNetworkOwner())
print(part:CanSetNetworkOwnership())
```

For projectiles, objectives and pickups: `SetNetworkOwner(nil)` so the client
cannot mess with them. For the vehicle the player is driving: give the player
ownership, otherwise it feels rubber-bandy.

### 13.14 Parallel Luau with Actors

> ✅ **Live game** &nbsp;`server + client` &nbsp;— only inside an Actor.

```luau
-- In a script under an Actor instance
local function heavyWork()
    task.desynchronize()   -- go parallel

    local result = 0
    for i = 1, 1_000_000 do
        result += math.sqrt(i)
    end

    task.synchronize()     -- back to serial for property writes
    workspace.Result.Value = result
end
```

Rules: while parallel you may not write Instance properties, reading is fine.
For shared state between actors there is `SharedTable`.

### 13.15 Actor messaging for parallel Luau

> ✅ **Live game** &nbsp;`server + client` &nbsp;— only inside an Actor.

Beyond `task.desynchronize`, the messaging API is where the real work happens:

```luau
-- In the Actor script
const actor = script:GetActor()

actor:BindToMessageParallel("ProcessChunk", function(chunkId: number, data: buffer)
    -- runs parallel, no Instance writes
    const result = heavyComputation(data)

    task.synchronize()
    applyResult(chunkId, result)
end)

-- From the main thread
actor:SendMessage("ProcessChunk", 1, chunkBuffer)
```

`BindToMessage` runs serially, `BindToMessageParallel` runs parallel. For
volumetric smoke or terrain generation this is how you use multiple cores.

Shared state between actors:

```luau
const SharedTableRegistry = game:GetService("SharedTableRegistry")
const shared = SharedTableRegistry:GetSharedTable("VoxelData")

shared.chunkCount = 100
SharedTable.increment(shared, "processed", 1)   -- atomic
```

### 13.16 MemoryStoreService for cross-server

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— in Studio only after **Enable Studio Access to API Services**.

DataStores are slow and have tight limits. MemoryStore is for fast, temporary,
cross-server data: matchmaking queues, live leaderboards, global events.

```luau
const MemoryStoreService = game:GetService("MemoryStoreService")

-- Queue for matchmaking
const queue = MemoryStoreService:GetQueue("MatchmakingQueue", 30)  -- 30s invisibility
queue:AddAsync({ userId = player.UserId, mmr = 1500 }, 600)        -- 600s expiry

const items, id = queue:ReadAsync(4, false, 30)
if items then
    queue:RemoveAsync(id)
end

-- SortedMap for a live leaderboard
const sortedMap = MemoryStoreService:GetSortedMap("GlobalScores")
sortedMap:SetAsync(tostring(player.UserId), score, 3600)
const top = sortedMap:GetRangeAsync(Enum.SortDirection.Descending, 10)

-- HashMap for fast key-value
const hashMap = MemoryStoreService:GetHashMap("ActiveSessions")
hashMap:SetAsync(tostring(player.UserId), jobId, 300)
```

Everything here belongs in a pcall, these are network calls.

### 13.17 MessagingService

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— messages do not travel between Studio sessions and live servers.

```luau
const MessagingService = game:GetService("MessagingService")

MessagingService:SubscribeAsync("GlobalAnnouncement", function(message)
    print(message.Data, message.Sent)
end)

MessagingService:PublishAsync("GlobalAnnouncement", { text = "Event start!" })
```

The limit is roughly 1KB per message. Combine it with MemoryStore when you need
to share more data: publish a key and let servers pull the data from MemoryStore.

### 13.18 DataStore: UpdateAsync and session locking

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— in Studio only after **Enable Studio Access to API Services**.

```luau
-- SetAsync overwrites blindly, race conditions guaranteed
store:SetAsync(key, value)

-- UpdateAsync reads and writes atomically
store:UpdateAsync(key, function(old)
    old = old or { coins = 0 }
    old.coins += 100
    return old
end)
```

Session locking to prevent data loss on server hops:

```luau
const function acquireLock(key: string, jobId: string): boolean
    local acquired = false
    const ok = pcall(function()
        store:UpdateAsync(key, function(data)
            data = data or { lock = nil, payload = {} }

            const lock = data.lock
            if lock and lock.jobId ~= jobId and os.time() - lock.time < 60 then
                return nil   -- returning nil cancels the write
            end

            data.lock = { jobId = jobId, time = os.time() }
            acquired = true
            return data
        end)
    end)
    return ok and acquired
end
```

Check the budget before you hammer:

```luau
const budget = DataStoreService:GetRequestBudgetForRequestType(
    Enum.DataStoreRequestType.UpdateAsync
)
if budget < 5 then
    task.wait(5)
end
```

> [!TIP]
> For production an existing library such as ProfileStore is smarter than building
> this yourself. Getting session locking right is surprisingly hard.

### 13.19 Retry with exponential backoff

> ✅ **Live game** &nbsp;`server + client` &nbsp;— the calls you wrap with it are usually server only.

Every Roblox web call can fail. This belongs in your codebase by default:

```luau
local function retry<T...>(attempts: number, fn: () -> T...): (boolean, T...)
    local lastError
    for attempt = 1, attempts do
        const results = table.pack(pcall(fn))
        if results[1] then
            return true, table.unpack(results, 2, results.n)
        end
        lastError = results[2]

        if attempt < attempts then
            task.wait(2 ^ (attempt - 1) + math.random() * 0.5)
        end
    end
    warn(`All {attempts} attempts failed: {lastError}`)
    return false
end

const ok, data = retry(4, function()
    return store:GetAsync(key)
end)
```

The `math.random()` jitter stops 50 servers from retrying at the same moment.

### 13.20 Deferred events

> ⚠️ **Live game** &nbsp;`depends on SignalBehavior` &nbsp;— set per place, applies in Studio and live. Test both settings.

Event handlers do not necessarily fire immediately. `Workspace.SignalBehavior`
decides, and every place created from a template is set to `Deferred`. `Default`
is currently equivalent to `Immediate` but that will change, so write code that
is correct in both modes.

With `Immediate` the handler runs right away, in the middle of the engine call
that caused the event. With `Deferred` the handler goes into a queue and runs at
the next resumption point. The resumption points are:

- input processing, once per input to be processed
- `RunService.PreRender`
- legacy `wait`, `spawn` and `delay` resumption
- `RunService.PreAnimation`, `PreSimulation`, `PostSimulation`
- `task.wait`, `task.spawn`, `task.delay` resumption
- `RunService.Heartbeat`
- `game:BindToClose`

What breaks, pattern one: reading a value the handler still has to set.

```luau
-- Always returns false with deferred events
local success = false
event:Connect(function()
    success = true
end)
doSomethingThatFiresTheEvent()
return success
```

You have to yield until at least the moment the handler should have run:

```luau
const done = Instance.new("BindableEvent")
event:Once(function()
    done:Fire()
end)
doSomethingThatFiresTheEvent()
done.Event:Wait()
```

Pattern two: the world has already changed by the time the handler runs.

```luau
const part = Instance.new("Part")
part.Parent = workspace

part.Destroying:Connect(function()
    print(part:GetFullName(), #part:GetChildren())   -- already emptied and detached
end)

part:Destroy()
```

So never read the world inside a handler as if it were still in the state it was
at fire time. Use the event arguments, or copy what you need before making the
change.

Pattern three, disconnect behavior:

- `Disconnect()` throws away all pending invocations
- `Destroy()` on the instance disconnects immediately, but still runs pending
  handlers
- `connection:Disconnect()` inside the handler is not a reliable "first time
  only", because several invocations may already be queued. Use `Once`

There is a re-entrancy limit on events triggering each other. The current depth
is 10, after which you get `Maximum event re-entrancy depth exceeded`.

> [!IMPORTANT]
> Test your game once with `Immediate` and once with `Deferred`. Code that only
> works in one mode leans on an assumption you do not control.

### 13.21 Instance streaming

> ⚠️ **Live game** &nbsp;`server config, client effect` &nbsp;— in Studio play solo you barely notice it, in the live game it is everything.

With `workspace.StreamingEnabled` the server only sends the regions around the
player to the client. The server always has everything, the client almost never
does. Any client script that assumes a part exists is broken by this.

What you need to know:

- streaming only touches descendants of `Workspace`. ReplicatedStorage,
  ReplicatedFirst and the rest replicate normally
- `WaitForChild` without a timeout on a streamed-out part hangs until the player
  happens to walk over there
- raycasts and overlap queries on the client only hit what is loaded
- client-side physics only simulates in streamed regions
- a `Sound` under a part that streams out stops

`ModelStreamingMode` per model:

| Mode | Behavior |
|---|---|
| Nonatomic | default, parts stream in and out individually |
| Atomic | the model only appears once all its initial descendants are there |
| Persistent | sent complete shortly after joining and never streams out |
| PersistentPerPlayer | Persistent for players from `Model:AddPersistentPlayer()`, Atomic for the rest |

```luau
-- A script needs all parts of this model
npcModel.ModelStreamingMode = Enum.ModelStreamingMode.Atomic

-- Must always exist on every client
hubModel.ModelStreamingMode = Enum.ModelStreamingMode.Persistent
```

Persistent is for the handful of things that genuinely must always be there.
They cost permanent memory on every client. Wait for them on the client:

```luau
workspace.PersistentLoaded:Wait()
```

Prefetch before you move someone, otherwise you get a streaming pause:

```luau
const ok = pcall(function()
    player:RequestStreamAroundAsync(targetPosition)
end)
character:PivotTo(CFrame.new(targetPosition))
```

For areas the player moves between often you can set multiple replication foci
with `Player:AddReplicationFocus()`.

Rule: all authoritative logic server side, and client scripts that can deal with
parts coming and going. CollectionService with `GetInstanceAddedSignal` ([13.4](#134-collectionservice-tags)) is
nicer for that than `WaitForChild`, because it fires again by itself when
something streams back in.

### 13.22 Modules: lifetime and require semantics

> ✅ **Live game** &nbsp;`server + client` &nbsp;— server, client and every Actor each have their own module cache.

The body of a ModuleScript runs once per Luau VM, after which the result is
cached. Server and client are separate VMs, so the same module exists twice with
completely separate state. Every Actor also has its own copy, which is exactly
why parallel Luau works ([13.14](#1314-parallel-luau-with-actors)).

```luau
-- Counter.luau
const Counter = { value = 0 }
return Counter
```

Server and client each have their own `value`. You share state through remotes
or through `SharedTableRegistry` ([13.15](#1315-actor-messaging-for-parallel-luau)), never through a module.

Two errors you will hit sooner or later:

- `Requested module was required recursively`. A requires B, B requires A. Put
  the shared types or data in a third module, or make one of the two requires
  lazy by doing it inside the function instead of at the top
- `Requested module experienced an error while loading`. The body threw. That
  error is cached and every subsequent require gives it back. The real cause is
  in the first error in the console, not the tenth

That is why side effects in the module body are risky: you do not control who
requires first, so you do not control when they run. Split loading from starting:

```luau
const Service = {}

function Service.init()
    -- connections, timers, state
end

return Service
```

One bootstrap script then decides the order. That also makes your modules
testable ([14.6](#146-testing)), because a test can skip `init`.

### 13.23 Player and character lifecycle

> ✅ **Live game** &nbsp;`server + client` &nbsp;— `CharacterAutoLoads` and `LoadCharacter` are server only.

`PlayerAdded` misses everyone who was already in when your script started
running. In Studio that is almost always the case:

```luau
const function onPlayerAdded(player: Player)
    -- ...
end

Players.PlayerAdded:Connect(onPlayerAdded)
for _, player in Players:GetPlayers() do
    task.spawn(onPlayerAdded, player)
end
```

The same goes for characters:

```luau
const function onCharacter(character: Model)
    const humanoid = character:WaitForChild("Humanoid", 10) :: Humanoid?
    if not humanoid then
        return
    end
end

player.CharacterAdded:Connect(onCharacter)
if player.Character then
    onCharacter(player.Character)
end
```

`player.Character` can be nil, can be the old character that is just being
replaced, and the Humanoid is not guaranteed to be inside it yet. Use
`CharacterAppearanceLoaded` if you are waiting on accessories or body colors, and
`Players.CharacterAutoLoads = false` plus `player:LoadCharacter()` if you want to
control respawning yourself.

### 13.24 Shutting down with BindToClose

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— it also runs when you stop a Studio test, hence the `IsStudio` check.

`PlayerRemoving` covers players who leave. It does not cover a server shutting
down, and that is exactly when you lose data. You need both:

```luau
Players.PlayerRemoving:Connect(function(player)
    Data.save(player)
end)

game:BindToClose(function()
    if RunService:IsStudio() then
        return
    end

    const players = Players:GetPlayers()
    local remaining = #players
    if remaining == 0 then
        return
    end

    for _, player in players do
        task.spawn(function()
            pcall(Data.save, player)
            remaining -= 1
        end)
    end

    const deadline = os.clock() + 25
    while remaining > 0 and os.clock() < deadline do
        task.wait(0.1)
    end
end)
```

> [!IMPORTANT]
> You get roughly 30 seconds. So save in parallel and not in a sequential loop,
> because every DataStore call can take seconds and with 30 players you run out of
> time. Combine this with session locking ([13.18](#1318-datastore-updateasync-and-session-locking)),
> otherwise the old server is still writing while the player is already on a new one.

### 13.25 What replicates and what does not

> ✅ **Live game** &nbsp;`server + client` &nbsp;— engine behavior, identical in Studio and live.

> [!IMPORTANT]
> The rule is short and people still guess it wrong. Server to client replicates
> nearly everything. Client to server replicates nearly nothing.

From the client, only this replicates:

- the physics, meaning CFrame and velocity, of assemblies that client has network
  ownership of, including their own character ([13.13](#1313-network-ownership))
- the humanoid state and the animations of that own character

That is it. Concretely that means:

```luau
-- All local only, the server sees none of it
Instance.new("Part").Parent = workspace
part.Transparency = 0.5
part:SetAttribute("Owner", player.UserId)
CollectionService:AddTag(part, "Target")
part:Destroy()
```

The other way around, a server change does replicate to everyone, except for a
few properties that are local by definition, such as
`LocalTransparencyModifier` and everything on the Camera.

What this means for your game: everything the player does goes through a remote
([section 12](#12-buffers-and-network-code)), and the only thing the client gets to decide by itself is where
their own character is. That is exactly why teleport and speed hacks exist and
why position validation belongs on the server ([12.12](#1212-server-side-safety)).

### 13.26 HttpService and JSON

> 🖥️ **Live game** &nbsp;`server only` &nbsp;— and only with HTTP requests enabled in Game Settings. These methods do not exist on the client.

HttpService only works on the server and only if HTTP requests are enabled in the
game settings. The budget is roughly 500 requests per minute per server.

Use `RequestAsync` and not `GetAsync` or `PostAsync`, because then you get the
status and headers back instead of just a body:

```luau
const HttpService = game:GetService("HttpService")

const function post(url: string, payload: any): (any?, string?)
    const ok, result = pcall(function()
        return HttpService:RequestAsync({
            Url = url,
            Method = "POST",
            Headers = {
                ["Content-Type"] = "application/json",
                ["Authorization"] = `Bearer {TOKEN}`,
            },
            Body = HttpService:JSONEncode(payload),
        })
    end)

    if not ok then
        return nil, tostring(result)              -- network error or blocked
    end
    if not result.Success then
        return nil, `HTTP {result.StatusCode}`    -- backend returned an error code
    end

    const decoded = HttpService:JSONDecode(result.Body)
    return decoded
end
```

> [!WARNING]
> A 500 from your backend is not a Luau error. Without the `result.Success` check
> you happily continue with an error page as your payload.

JSONEncode pitfalls:

```luau
HttpService:JSONEncode({})                    -- "[]", not "{}"
HttpService:JSONEncode({1, nil, 3})           -- hole in the array, output is wrong
HttpService:JSONEncode({[1] = "a", x = 1})    -- mixed keys, unpredictable
HttpService:JSONEncode({ n = math.huge })     -- inf and NaN produce invalid JSON
```

So an empty table becomes an empty array. If your backend expects an object, send
a sentinel field with it or leave the field out entirely.

Large numbers go across the wire as doubles, so send UserIds and other long IDs
as strings ([2.14](#214-number-precision-important-for-userids)). And whatever comes back is unknown data: validate it at the
boundary before you do anything with it ([14.2](#142-validate-at-the-edge-trust-on-the-inside)).

```luau
const raw = HttpService:JSONDecode(body) :: any
if type(raw) ~= "table" or type(raw.userId) ~= "string" then
    return
end
```

Talking to your own backend: there is no built-in crypto library in Roblox. Sign
requests with a shared secret plus a timestamp and have your backend reject
replays, or have your backend issue short-lived tokens. If your backend needs to
talk the other way, use Open Cloud instead of having the client poll.

### 13.27 EditableImage and EditableMesh

> ⚠️ **Live game** &nbsp;`client beta since January 2025` &nbsp;— there is a memory budget, and for assets that are not yours you need permissions.

`EditableImage` is not an Instance but an Object without a Parent. You create it
through AssetService and attach it with `Content` to something that displays an
image.

```luau
const AssetService = game:GetService("AssetService")

const image = AssetService:CreateEditableImage({ Size = Vector2.new(256, 256) })
imageLabel.ImageContent = Content.fromObject(image)

-- Works the same for Decal.TextureContent and MeshPart.TextureContent
```

You read and write pixels as a buffer: 4 bytes per pixel, RGBA, row by row from
the top left.

```luau
const size = image.Size
const pixels = buffer.create(size.X * size.Y * 4)

for y = 0, size.Y - 1 do
    for x = 0, size.X - 1 do
        const offset = (y * size.X + x) * 4
        buffer.writeu8(pixels, offset, 255)       -- R
        buffer.writeu8(pixels, offset + 1, 128)   -- G
        buffer.writeu8(pixels, offset + 2, 0)     -- B
        buffer.writeu8(pixels, offset + 3, 255)   -- A
    end
end

image:WritePixelsBuffer(Vector2.zero, size, pixels)
```

Four `writeu8` calls per pixel is a waste. Pack the color into one `writeu32`.
Little endian means the first byte is R:

```luau
buffer.writeu32(pixels, offset, r + g * 256 + b * 65536 + a * 16777216)
```

Two limits that hit every realtime effect:

- at most one EditableImage is updated to the screen per frame. Updating three
  images therefore takes three frames. One large atlas beats four small ones
- there is a ceiling on the resolution and on your total editable asset memory.
  Allocate one buffer and reuse it, do not create a new one every frame

`CreateEditableImageAsync` starts from an existing asset. `DrawImage`,
`DrawRectangle` and `DrawImageTransformed` do compositing without you looping per
pixel, which is nearly always faster than a Luau loop. `EditableMesh` follows the
same principle but for vertices and faces.

> [!NOTE]
> These APIs are still moving. Check the Creator Docs for the current limits before
> you build a system around them.

### 13.28 Small things that add up

> ✅ **Live game** &nbsp;`server + client`

```luau
-- ALWAYS parent last, otherwise every property change replicates separately
const part = Instance.new("Part")
part.Size = Vector3.one
part.Anchored = true
part.Parent = workspace          -- last line

-- NOT: Instance.new("Part", workspace)

-- GetService, always
const Players = game:GetService("Players")   -- not game.Players

-- Property changed signals
part:GetPropertyChangedSignal("Position"):Connect(onMove)

-- Preload before you show something
const ContentProvider = game:GetService("ContentProvider")
ContentProvider:PreloadAsync({ decal, sound, image })

-- Unique IDs
const guid = game:GetService("HttpService"):GenerateGUID(false)

-- Determinism with your own RNG stream
const rng = Random.new(seed)
rng:NextInteger(1, 100)
rng:NextNumber()
rng:NextUnitVector()

-- Measuring ping
print(player:GetNetworkPing() * 1000, "ms")

-- Detecting the environment
const RunService = game:GetService("RunService")
if RunService:IsStudio() then end
if RunService:IsServer() then end
if RunService:IsRunMode() then end
```

### 13.29 Enums are objects, not numbers

> ✅ **Live game** &nbsp;`server + client`

`Enum.Material.Plastic` is an `EnumItem`, a real object with a name and a number
behind it. That matters the moment you store one or send one over the wire.

```luau
const material = Enum.Material.Plastic

print(material.Name)    -- "Plastic"
print(material.Value)   -- 256
print(typeof(material)) -- "EnumItem"

-- Comparison is by identity, so this just works
if part.Material == Enum.Material.Neon then end

-- Every item of one enum
for _, item in Enum.Material:GetEnumItems() do
    print(item.Name, item.Value)
end

-- Back from a number or a name
const fromValue = Enum.Material:FromValue(256)
const fromName = Enum.Material:FromName("Plastic")
```

`FromValue` and `FromName` return `nil` for anything that does not exist, so
they are the safe way back from stored data.

> [!WARNING]
> Do not store `EnumItem.Value` in a DataStore or a packet as if it were stable.
> Those numbers belong to Roblox, not to you. They have been renumbered before
> and enum items do get deprecated. Store your own number and map it yourself.

```luau
-- Your numbering, your problem, and it never moves under you
const MaterialId = table.freeze({
    [Enum.Material.Plastic] = 0,
    [Enum.Material.Neon] = 1,
    [Enum.Material.Metal] = 2,
})

const MaterialById = table.freeze({
    [0] = Enum.Material.Plastic,
    [1] = Enum.Material.Neon,
    [2] = Enum.Material.Metal,
})

writer:u8(MaterialId[part.Material])
part.Material = MaterialById[reader:u8()] or Enum.Material.Plastic
```

An `EnumItem` also works as a table key, which is what the map above relies on.
For the type checker, `Enum.Material` as a type annotation is the *enum*, and
`Enum.Material.Plastic` is one item of it:

```luau
local function paint(part: BasePart, material: Enum.Material)
    part.Material = material
end

paint(part, Enum.Material.Neon)   -- OK
paint(part, "Neon")               -- Type error
```

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 14. Working practice

### 14.1 Standard script header

> ✅ **Live game** &nbsp;`server + client` &nbsp;— these are the directives from [4.1](#41-script-directives).

Set the type check mode once for the whole place instead of repeating it in every
file:

- In Studio, `Workspace.LuauTypeCheckMode` is the default for every script. A
  `--!strict` or `--!nonstrict` at the top of a file only overrides it
- With Rojo and luau-lsp, `languageMode` in `.luaurc` or `.config.luau`
  ([14.4](#144-tooling)) does the same for your editor and CI

`--!optimize 2` is not needed for shipping either. Published games are already
compiled at level 2, the directive only changes Studio.

What is left for the top of a file is what is specific to that file:

```luau
--!native
-- only on scripts with real computational load, see 11.6

const Players = game:GetService("Players")
const ReplicatedStorage = game:GetService("ReplicatedStorage")
```

### 14.2 Validate at the edge, trust on the inside

> 📐 **Live game** &nbsp;`pattern, not a feature`

Validate once at the boundary (remote handler, module public API) and work with
types you trust after that. Do not make every function defensive, that makes code
slow and unreadable.

```luau
-- Boundary: check everything
function Networker.onServerPacket(player: Player, raw: unknown)
    if type(raw) ~= "buffer" then return end
    -- ... validation ...
    Combat.applyDamage(player, validatedPayload)   -- trusted from here on
end

-- Internal: the types do the work
function Combat.applyDamage(player: Player, payload: DamagePayload)
    -- no repeated type checks
end
```

### 14.3 No magic numbers

> 📐 **Live game** &nbsp;`pattern, not a feature`

```luau
-- Bad
if timeSinceLastShot < 0.15 then return end

-- Good
const FIRE_RATE_SECONDS = 0.15
if timeSinceLastShot < FIRE_RATE_SECONDS then return end
```

Put them in a frozen config module so they live in one place and nobody changes
them by accident:

```luau
return table.freeze({
    Combat = table.freeze({
        FIRE_RATE = 0.15,
        MAX_RANGE = 500,
        HEADSHOT_MULTIPLIER = 2,
    }),
    Network = table.freeze({
        TICK_RATE = 20,
        MAX_PACKET_SIZE = 256,
    }),
})
```

### 14.4 Tooling

> 🚫 **Live game** &nbsp;`not shipped` &nbsp;— tooling runs on your machine, not in the game.

- **Rojo** for filesystem-based development with real git history
- **StyLua** for consistent formatting, run it as a pre-commit hook
- **Selene** as a linter, catches bugs the Luau type checker misses
- **Luau LSP** in VS Code for autocomplete and type errors outside Studio
- **Wally** for package management

A `.luaurc` in the root for your type settings:

```json
{
  "languageMode": "strict",
  "lint": { "*": true },
  "lintErrors": true
}
```

Or the same settings written as Luau in a `.config.luau`, which can also use
variables and logic. Pick one per directory, having both is an error:

```luau
return {
    luau = {
        languagemode = "strict",
        lint = { ["*"] = true },
        linterrors = true,
    },
}
```

### 14.5 Measure, do not guess

> ✅ **Live game** &nbsp;`server + client` &nbsp;— the MicroProfiler is in the live client too.

```luau
debug.profilebegin("NetworkFlush")
flushQueues()
debug.profileend()

debug.setmemorycategory("Networking")
```

Open the MicroProfiler with Ctrl+F6 in Studio. The Developer Console has a Memory
tab that shows your categories. Only optimize after you have measured which step
is slow.

### 14.6 Testing

> 🚫 **Live game** &nbsp;`not shipped` &nbsp;— tests run in Studio or in CI, not in the published game.

- **Jest-Lua** (`jsdotlua/jest-lua`) is the best maintained runner. TestEZ is the
  old standard and barely gets updates any more
- test files next to the module itself, `Combat.spec.luau`, and included in the
  tree through Rojo
- run them in Studio with a runner script, or headless in CI with
  `run-in-roblox`

More important than the runner is whether your code is testable at all. A module
that touches `workspace` or `Players` can only run in a real session. A pure
module you test in milliseconds. That is the same boundary as in [14.2](#142-validate-at-the-edge-trust-on-the-inside): engine
stuff at the edge, logic in the middle.

```luau
-- Not testable, needs a Player and a character
function Combat.fire(player: Player)
    const origin = player.Character.Head.Position
    -- ...
end

-- Testable, pure function
function Combat.resolveShot(
    origin: Vector3,
    direction: Vector3,
    targets: { Target }
): Hit?
    -- ...
end
```

That same split makes your network code easier: you can run `resolveShot` on the
server for validation and on the client for prediction, without duplication.

### 14.7 CI

> 🚫 **Live game** &nbsp;`not shipped` &nbsp;— this runs on your build machine.

A minimal pipeline you can drop into GitHub Actions:

```bash
rojo sourcemap default.project.json --output sourcemap.json
luau-lsp analyze --sourcemap=sourcemap.json --defs=globalTypes.d.luau src/
selene src/
stylua --check src/
```

You generate `globalTypes.d.luau` from the Roblox API dump; luau-lsp ships a
script for it. Without those definitions the analyzer does not know `game`,
`Instance` and the rest, and the output is worthless.

For everything around it, think codegen, asset pipelines and deploys through Open
Cloud, run Luau outside Roblox with **Lune**. **Lute** is the newer runtime from
the Luau team itself, still young but worth watching. The advantage of both: your
tooling is in the same language as your game.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 15. Quick reference

| Feature | Syntax | What for |
|---|---|---|
| Const binding | `const x = 0` | Block reassignment |
| Compound assign | `x += 1` | Shorter, evaluates the left side once |
| Continue | `continue` | Skip a loop iteration |
| Interpolation | `` `hi {x}` `` | Building strings |
| If expression | `if a then b else c` | Inline conditional |
| Gen. iteration | `for k, v in t do` | Replaces pairs/ipairs, same speed |
| Type cast | `x :: T` | Convincing the compiler |
| Optional | `T?` | Can be nil |
| Union | `A \| B` | One of the two |
| Intersection | `A & B` | Both, or overloads |
| Singleton | `"a" \| "b"` | Enum-like types |
| Never | `x: never` | Exhaustiveness checking |
| Generic | `<T>` | Reusable functions |
| Type pack | `<T...>` | Variadic generics |
| Read-only | `read x: T` | Immutable to the type checker |
| Freeze | `table.freeze(t)` | Runtime immutable |
| Buffer | `buffer.create(n)` | Binary data, networking |
| Native | `--!native` | Machine code compilation |
| Error level | `error(msg, 2)` | Points at the caller |
| Swap-remove | `t[i] = t[#t]` | O(1) removal |
| Vector library | `vector.create(x, y, z)` | Allocation-free vector math |
| Deferred events | `workspace.SignalBehavior` | When handlers run |
| Model streaming | `ModelStreamingMode` | Whether the client has the parts |
| Shutdown save | `game:BindToClose(fn)` | Last chance to write data |
| String require | `require("@game/A/B")` | A path instead of an instance chain |
| Enum item | `Enum.Material:FromValue(n)` | Safe way back from a stored number |

### Common mistakes

| Mistake | Correct |
|---|---|
| `for _, v in ...` over varargs | `table.pack`, `select`, or `{...}` |
| `table.isFrozen` | `table.isfrozen`, lowercase |
| `: void` as a return type | `()` |
| `{ok: T} \| {err: E}` as a Result | Needs a tag field for narrowing |
| `#dictionary == 0` | `next(dictionary) == nil` |
| `#t` on tables with holes | Track your own count |
| `WaitForChild(name)` without a timeout | Pass the second argument |
| Deep recursion with a tail call | Luau has no tail calls, use a loop |
| Assuming a handler runs immediately | Deferred is the default, yield first |
| `WaitForChild` on the client with streaming | Atomic or Persistent model, or tags |
| Only `PlayerRemoving` to save | Also `game:BindToClose` |
| A client-side change the server "sees" | Only physics of your own assembly replicates |
| `JSONEncode({})` as an empty object | Returns `[]` |
| Sharing state through a module | Server and client are separate VMs |
| `require("Modules/X")` with no prefix | Start with `./`, `../`, `@self/` or `@game/` |
| Storing `EnumItem.Value` as if it is stable | Keep your own number and map it |
| Localizing `math.sqrt` and friends for speed | Imports and fastcalls already cover it |
| `typeof(t.Field)` as an enum type | `keyof<typeof(t)>`, the field is just `string` |
| Treating a type error as a compile error | The script still runs, fail CI on it |

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

## 16. Sources

**Luau itself**

- Syntax: https://luau.org/syntax
- Type system: https://luau.org/types
- Type functions: https://luau.org/types/type-functions
- Object-oriented programs: https://luau.org/types/object-oriented-programs/
- Compatibility with Lua 5.x: https://luau.org/compatibility
- Performance: https://luau.org/performance
- Library reference: https://luau.org/library
- RFCs for upcoming features: https://rfcs.luau.org

**Roblox**

- Creator Docs: https://create.roblox.com/docs
- Deferred events: https://create.roblox.com/docs/scripting/events/deferred
- Instance streaming: https://create.roblox.com/docs/workspace/streaming
- API reference source in markdown: https://github.com/Roblox/creator-docs

**Tooling**

- Rojo: https://rojo.space
- Wally: https://wally.run
- Luau LSP: https://github.com/JohnnyMorganz/luau-lsp
- Jest-Lua: https://github.com/jsdotlua/jest-lua
- Lune: https://github.com/lune-org/lune
- Lute: `luau-lang/lute` on GitHub

Luau and the Roblox engine both change regularly. The RFC repo shows what is
coming to the language, and the creator-docs repo lets you see the diffs of the
engine documentation before you run into them.

<p align="right"><a href="#roblox-luau-reference"><sub>Back to top</sub></a></p>

---

<div align="center">

**Roblox Luau Reference** · verified against Roblox Studio 733 with the new type solver

[![Bindings](https://img.shields.io/badge/01-Bindings-0B84FF?style=for-the-badge&labelColor=1F2328)](#1-bindings-const-and-local) [![Standard library](https://img.shields.io/badge/05-Standard_library-2DA44E?style=for-the-badge&labelColor=1F2328)](#5-standard-library) [![Networking](https://img.shields.io/badge/12-Networking-E2231A?style=for-the-badge&labelColor=1F2328)](#12-buffers-and-network-code) [![Roblox API](https://img.shields.io/badge/13-Roblox_API-E2231A?style=for-the-badge&labelColor=1F2328)](#13-roblox-api) [![Quick reference](https://img.shields.io/badge/15-Quick_reference-57606A?style=for-the-badge&labelColor=1F2328)](#15-quick-reference)

<sub>[Back to the full index](#contents)</sub>

</div>
