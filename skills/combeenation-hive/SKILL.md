---
name: combeenation-hive
description: Hive, the rule language of Combeenation configurators. Use when reading, writing, or fixing Hive rules, components, or queries.
---

# Hive

Hive is the language in which a Combeenation configurator's business logic is written. Read this file before writing any Hive; it covers what you cannot guess from other languages.

## Mental model: a spreadsheet

A configurator is a **spreadsheet**. A **Value component** is a named cell; its rule, stored in its `Value` field, is the cell's formula. When anything a rule references changes, the platform re-evaluates that rule and everything downstream, in dependency order. Rules never decide when they run.

Where the spreadsheet ends:

- **Pure expressions.** A rule is one expression that produces a value. There is no re-assignment, no mutation, no loop, and no lambda; `var` names a value once. To transform a collection, write a query.
- **`input` is the only state.** Each component rule has an `input`: a value set from outside the rule (UI controls, Custom Code). It is persisted automatically with the configuration and is the only way to store a value. A rule typically reads it with a fallback: `input or 10`.
- **Static types.** A type mismatch is a compile error, not a runtime value.
- **Names, not addresses.** Components are referenced by name, case-insensitively.

## Types

Every value has a static type, and a component's type carries the type of its value: `ValueComponent<Number>`, or `ValueComponent<{id=Text,price=Number,thumb=Image}>` for a record. A Value component's type follows its compiled rule: change the rule's result type and the component's type changes, and every rule that uses the component is re-checked. When you create a component, give it the type its rule produces. When an edit changes a component's type, check the errors it caused in other components.

Write types as `Number`, `Text`, `Logic`, `List<T>`, `Map<K, V>`, and records as `{name=Type, ...}`.

## Syntax

```hive
// line comment          /* block comment */
var net = Price * Amount;           // name a value once; `;` separates expressions
var tax: Number = net * 0.2;        // optional explicit type
net + tax                           // the last expression is the rule's result

if Amount > 9 then Price * 0.8      // every if needs an else, and ends with end
elif Amount >= 5 then Price * 0.9
else Price end

input or 1                          // fallback when the left side is null
Selected?.name or ""                // ?. yields null instead of failing on null
x = null                            // null and empty are the same keyword; errors say Empty

[1, 2, 3]                           // list
["red": 1, "blue": 2]               // map (Number or Text keys)
{ id: "a1", price: 9.5 }            // record value; its type is {id=Text,price=Number}
new Mail { mail: "a@b.c" }          // object of a named type
OnFinish.Quote(new { ... })         // type name omitted: only where the type is inferred
var m: Mail = new { mail: "a@b.c" } // ...such as from an explicit type
m with { mail: "x@y.z" }            // copy of an object with changed fields (objects only)
"""Total: {=Price * Amount} EUR"""  // formatted text; {=expr} embeds a value, {{ is a literal {
input or json`{ "a": [true, null] }` // opaque JSON value, for Custom Code input; no member access or interpolation

value is Number    value is not Text    value as Number
```

Operators: `+ - * / mod ^`, comparison `= <> < > <= >=`, `~=` (case-insensitive text equality), `and or not`, and `+` to concatenate text. Call functions with parentheses (`name.toLower()`, `list.length()`); read properties without them (`configurator.isDraft`).

Global variables: `configurator` (properties, `configurator.components`, and any component by name), `presets`, `configuration`, `user`. Write `@top.user` when a component shadows a global, and `Hive.Math.Pi` when one shadows a built-in type.

Queries (`from ... select ...`) load, filter, sort, and reshape data sources, lists, and maps. Before writing or editing one, read [references/queries.md](references/queries.md).

## Traps

- `=` compares and `<>` means not-equal. There is no `==`, `!=`, `&&`, `||`, or `!`.
- Records (`{ id: "a1" }`) and objects (`new ...`) are different: records hold data, objects are what built-in APIs take. `with` works on objects only.
- Record fields match by position, not name: write fields in the order of the target type. Same-typed fields in the wrong order compile and silently swap values. Object fields match by name.
- `new { ... }` without a type name compiles only where the type is inferred: a typed `var`, or a function parameter. `var x = new { ... }` fails.
- Text comparison with `=` is case-sensitive, and so are text map keys; use `~=` for case-insensitive equality.
- Indexes start at 1. Index 0 also returns the first item, a negative index counts from the end, and an out-of-range index is clamped to the last item rather than failing. Check `length()` when an empty collection matters.
- `get(key)` on a map returns `null` for a missing key.
- An empty `[]` only compiles where its type can be inferred: `input or []`, or `var items: List<Text> = []`.
- Avoid rules that can return `null`; give every path a real value.
- Write `null` in new code. When editing a rule that already uses `empty`, keep using `empty` in that rule.
- A text literal uses double quotes. Inside it, escape `\"`, `\\`, `\n`, and `\t`.

## Editor version and legacy features

`cfgr_draft_list` reports each configurator's editor version.

- **v2**: create and edit Value components only. Other component types do not exist in v2.
- **v1**: may contain Table, Record, Resource, Graphic, or Checkout components. These are legacy: keep them working when you edit them, and build anything new with Value components. Before touching them, read [references/legacy.md](references/legacy.md).

## Looking up functions

This skill carries no function catalog; look functions up for each task:

1. The MCP tool that describes a type's members, once it is available.
2. The per-type pages listed at https://docs.combeenation.com/llms.txt (Number, Text, List, Map, and others). They can be incomplete.

Use a function only when one of these sources confirms it exists. If neither does, say so and stop rather than guess.

## Feedback

Every component edit through the MCP returns the errors the edit introduced and resolved, across all components. A valid edit can break other components. Read both lists after every edit, fix every new error, and only then move on.
