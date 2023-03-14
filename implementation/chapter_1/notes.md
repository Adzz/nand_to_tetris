# Boolean Logic.

## Truth Tables

This is a truth table for 2 variables:

X  | Y |  CHIP |
-------|-------|
1  | 0 |   1
0  | 1 |   1
1  | 1 |   0
0  | 0 |   0

This enumerates all possible combinations of true / false for two variables. We can also imagine a function that operates on both of those variables and produces a value. This function could produce either a 1 or a 0 for each row in the table, and if you knew the result for each row you would be able to figure out what that function actually is.

Let's take an example, let's imagine that for each row our imaginary function produces these values:

X  | Y | f(x, y) |
-------|---------|
1  | 0 |  1      |
0  | 1 |  0      |
1  | 1 |  0      |
0  | 0 |  0      |

What is the boolean function that produces this answer for each row? We can figure this out by discovering the "canonical representation".

## The Canonical Representation

The canonical representation is just the simplest function that describes the boolean operations needed to get the correct output for the given row. There are two ways to get it, Sum Of Minterm or Product Of Maxterm.

### Sum Of Minterm

We get it like this:

1. Select all rows for which f(x, y) is `1`
2. Let `0` mean `!variable` and `1` mean `variable`
3. AND together all literals created from the row.
4. OR together all of the statements created from steps 1-3.

5. Simplify??

So for row 1 we would use `x AND not(y)`, or in the usual notation `x · !y`*** This is the only row that returns a 1 for `f(x, y)` so we only use this row.

That means our canonical representation for the function shown above is:

`f(x, y) = x * !y`

If we use this formula for each row we get the correct answer.

Row 1: 1 * !0 = 1 * 1 = 1
Row 2: 0 * !1 = 0 * 0 = 0
Row 3: 1 * !1 = 1 * 0 = 0
Row 4: 0 * !0 = 0 * 1 = 0

If we go back to the `f(x, y)` what we did is make up an output for each row. That output can essentially be a 1 or 0 and it's the combination of _all_ rows that tells us what the function actually is. That means we could figure out every possible boolean function for two variables by enumerating all possibilities. Let's imagine every possible combination of output for all rows for X and Y.

### Sum of Maxterm

https://www.geeksforgeeks.org/canonical-and-standard-form/?ref=lbp

It would look something like this:

X  | Y | f1 | f2| f3| f4| f5| f6| f7| f8| f9|f10|f11|f12|f13|f14|f15|f16|
-------|----|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
0  | 0 |  0 | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 1 | 1 | 1 | 1 | 0 | 0 |
0  | 1 |  0 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 |
1  | 0 |  1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
1  | 1 |  0 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | 1 | 0 |

x * !y + !x + y

Each function is one possible combination of row outputs.

Because we have 2 possible values (`1` or `0`) the number of possible rows is 2^N where N is the number of variables. In our case we have x and y so that's 2^2 == 4 rows.

Each function can output a `1` or `0` per row, so the row combinations are also 2^N, but here N is the number of possible rows. In our case 4. So we have 2^4=16 possible functions.

In general it is 2^2^n combinations. That grows fast as we add variables! Eg just 3 variables has 256 possible functions!

## All Canonical Functions

If we found the canonical function for each possible function in the table above it would look like this. This is a good exercise to do yourself. You can do it by hand or you can even try to write a function to do it for you. My attempt is below

(This doesn't simplify the canonical function at all).
```elixir
rows = for x <- [0, 1], y <- [0, 1], do:  [x, y]
columns = for a <- [1, 0], b <- [1, 0], c <- [1, 0], d <- [1, 0] do
  [a,b,c,d]
end

Enum.map(columns, fn column ->
  Enum.with_index(column, fn output_for_row, index ->
    {Enum.at(rows, index), output_for_row}
  end)
  |> Enum.filter(fn
    {_row, 0} -> false
    {_row, 1} -> true
  end)
  |> Enum.reverse()
  |> Enum.map_join(" + ", fn
    {[1, 1]=c, 1} -> "x * y"
    {[1, 0]=c, 1} -> "x * not(y)"
    {[0, 0]=c, 1} -> "not(x) * not(y)"
    {[0, 1]=c, 1} -> "not(x) * y"
  end)
end)
```

| function_name | canonical_function      |
------------------------------------------|
f1              | x + not(y)            |
f2              | x * not(y) + not(x) * y |
f3              | x + y                   |
f4              |
f5              |
f6              |
f7              |
f8              |
f9              |
f10             |
f11             |
f12             |
f13             |
f14             |
f15             |
f16             |





***(!y can be y with a hat on but I can't find that symbol.I think this works too: ¬ ).
