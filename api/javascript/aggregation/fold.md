---
layout: api-command
language: JavaScript
permalink: api/javascript/fold/
command: fold
io:
    -   - sequence
        - value
    -   - sequence
        - sequence
related_commands:
    reduce: reduce/
    concatMap: concat_map/
---

# Command syntax #

{% apibody %}
sequence.fold(base, function) &rarr; value
sequence.fold(base, function, {emit: function[, finalEmit: function]}) &rarr; sequence
{% endapibody %}

# Description #

Apply a function to a sequence in order, maintaining state via an accumulator. The `fold` command returns either a single value or a new sequence.

In its first form, `fold` operates like [reduce][rd], returning a value by applying a combining function to each element in a sequence, passing the current element and the previous reduction result to the function. However, `fold` has the following differences from `reduce`:

* it is guaranteed to proceed through the sequence from first element to last.
* it passes an initial base value to the function with the first element in place of the previous reduction result.

In its second form, `fold` operates like [concatMap][cm], returning a new sequence rather than a single value. When an `emit` function is provided, `fold` will:

* proceed through the sequence in order and take an initial base value, as above.
* for each element in the sequence, call both the combining function and a separate emitting function with the current element and previous reduction result.
* optionally pass the result of the combining function to the emitting function.

[rd]: /api/javascript/reduce/
[cm]: /api/javascript/concat_map/

This command 