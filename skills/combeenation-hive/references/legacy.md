# Legacy components

Editor v1 configurators may contain component types that editor v2 no longer supports. Both editors run the same Hive, so a v1 configurator can do everything a v2 configurator can.

When you edit a v1 configurator:

- Keep existing legacy components working, and change them only as far as the task requires.
- Build anything new with Value components, using the replacements below.
- Migrating a legacy component is its own task. Do it only when the user asks for it.

| Legacy component   | Use instead                                                                                     |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| Table component    | A query in a Value component's rule                                                             |
| Record component   | A Value component holding a record or a list of records, or a data source asset for larger data |
| Resource component | An asset (image, file, ...)                                                                     |
| Checkout component | The configurator's `FinishActions` rule                                                         |
| Graphic component  | No replacement: produce 2D graphics in Custom Code                                              |

Unlike a Value component, legacy components have several fields that hold rules, so one legacy component does not equal one rule.
