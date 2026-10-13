# Hive queries

A query loads, filters, sorts, and reshapes a collection: a data source, a list, or a map. It can appear in any rule.

## Before querying a data source

Know its columns and their types before you write the query. Column types decide which operators and functions apply, and how sorting behaves: text sorts case-sensitively and alphabetically, numbers sort numerically. Column names are case-insensitive.

## Shape

```hive
from Parts select all                                         // whole source, same kind of collection
from p in Parts select [ p.name ]                             // name the query item; `all` is then unavailable
from Parts select { .nr = .partNumber, .name, .price }        // table; every column name starts with .
from Parts select { ...all, .label = .name.toUpper() }        // spread all columns, add or override some
from Parts select [ .partNumber : .name ]                     // map: key : value
from Parts select distinct [ .category ]                      // distinct works with any projection
from Parts select all
  filter .price > MinPrice and (Category = "" or .category ~= Category)
  sort by .category, .netWeight desc
  skip 1 take 100
```

- The query item is named `all` by default. `.column` is shorthand for `all.column`, or for `p.column` when the item is named `p`.
- Clauses appear in this order: `select`, then `filter`, `sort by`, `skip`, `take`. All except `select` are optional.
- `select` decides the result: `all` keeps the source's kind of collection, `{ }` builds a table, `[ x ]` a list, `[ k : v ]` a map.
- In a table projection, `.col` keeps a column, `.col as alias` (legacy) or `.alias = .col` renames it, `.col = expr` computes it, and `...expr` spreads fields. An explicit column overrides a spread field of the same name.
- A computed column is not visible in the same query's `filter`, `sort by`, or other computed columns: they still see the source column.
- `filter` takes any expression that results in `Logic`. To make a filter optional, write `if cond then ... else true end`.
- Every `sort by` value must be `Number`, `Text`, or `Logic`. A column or function call works as is; wrap any other expression in parentheses. The same applies to `skip` and `take`, which cannot use columns.

## Limits on column-dependent code

The query engine evaluates only the parts of `filter` and computed columns that depend on a column. Parts that don't depend on a column are evaluated beforehand and have no limits, so move component lookups out of column-dependent expressions. Column-dependent code may use only:

- Number: `+ - * / mod ^`, comparisons, `toText`, `round`, `floor`, `ceiling`, `truncate`, `atLeast`, `atMost`
- Text: `+ = <> ~=`, `length`, `contains`, `skip`, `take`, `replace`, `toUpper`, `toLower`, `toNumber`
- Logic: `not and or = <>`
- List: `length`, `get`, `contains`
- `if`/`elif`/`else`, `var`, and `?.`

```hive
filter Prices.Get(.partNumber).price < MaxPrice      // fails: the component lookup depends on a column
filter .price > Prices.Get(SelectedPart).price       // works: the right side is evaluated beforehand
```
