---
sidebar_position: 3
title: Expression editor
description: Author Ballerina expressions with assistance, suggestions, and inline validation.
---

# Expression editor

The Expression editor is the inline value-input surface used throughout the WSO2 Integrator IDE wherever a field accepts an expression. It provides syntax highlighting, inline validation, and a UI that adapts to the type expected at the cursor — so you can supply values without remembering the full Ballerina syntax for each context.

You can recognize the editor by the `fx` icon next to a field. Selecting it opens the inline editor; the expand control opens the same editor in a larger view for longer expressions.

## Where the expression editor appears

The Expression editor appears in every side panel and form that takes a value, including:

- Variable initializers in the **Declare Variable** panel.
- Arguments to function calls and connector remote functions.
- Conditions in **If** and **While** nodes.
- Default values for configurable variables.
- Any field marked with the `fx` icon.

{/* ![Expression editor in the Declare Variable panel](TODO-expression-editor-overview.png) */}

## Helper Pane

Helperpane will automatically appear once you start typing in the expression editor otherwise you can click the side expand icon.

### Input



### Configure

Controls next to the input let you expand the editor into a larger view and toggle the helper panels for variables and functions.

### Variables

Insert any variable that is in scope at the cursor. The picker lists reachable variables together with their types, so you can choose the right one without leaving the editor.

### Functions

Insert standard library functions and user-defined functions from the current package. Selecting a function inserts a call template with placeholders for each parameter.

## User experience

### Completions

Context-aware autocomplete suggests variables, functions, record fields, and keywords that are valid at the cursor. Suggestions are filtered by the expected type, so only compatible options appear first.

### Chips

Variables and function calls render as compact chips inside the expression. Chips keep long expressions readable and let you click a reference to inspect or replace it without editing the surrounding text.

### Function parameter type suggestions

While filling a function call, the editor surfaces the expected type for the current parameter and offers values that match it. This makes it easy to chain calls without checking the function signature manually.

## Variations

The Expression editor adapts its UI to the type expected at the cursor. The following variations cover the common cases.

### Text mode

Appears when the expected type is `string`. Provides string-literal entry with support for template strings and variable interpolation.

### Number

Appears for `int`, `float`, and `decimal` fields. Accepts numeric literals or numeric expressions and validates the value against the expected numeric type.

### Boolean

Appears for `boolean` fields. Offers a quick `true` / `false` toggle in addition to free-form expression entry.

### Array mode

Appears when the expected type is an array. You can add, reorder, and remove elements as discrete rows instead of editing one long literal, and each row uses the variation that matches the array's element type.

### Record mode

Appears when the expected type is a record. The editor renders each field of the record as its own input, so you can fill the record without writing braces or field names by hand. Optional fields can be added or removed from the form, and each field's input itself uses the variation that matches its type — a `string` field shows Text mode, an `int` field shows Number, an array field shows Array mode, and so on.

The Record variation also supports nested values inline. When a field is a union, an array, or a boolean, you can supply that value directly inside the record form:

- **Union** — choose the member type, then enter the value using that member's variation.
- **Array** — open the field as an inline array editor and add elements one by one.
- **Boolean** — toggle `true` / `false` on the field itself.

{/* ![Record variation UI](TODO-expression-editor-record.png) */}

### Union mode

Appears when the expected type is a union. Prompts you to pick which member type to supply, then switches to that member's variation for the actual value.

### Nested variations

When types compose — for example, a union of arrays — the editor nests the matching variations. You first pick the union member, and the editor then opens the corresponding inner variation (such as Array mode) for the selected type.

### SELECT

Appears for fields that accept a fixed set of values, such as enumerated configuration options. The field renders as a dropdown instead of a free-form expression.

### SQL

Appears when you author SQL queries — for example, in database connector parameters. The editor provides SQL-aware highlighting and lets you bind integration variables as query parameters.

### Prompt

Appears when you author natural-language prompts for AI nodes. The editor is optimized for multi-line text and supports inserting variables as chips so prompt templates remain readable.
