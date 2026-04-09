
#### Files 

##### Names

1.  File names should be **machine readable**: avoid spaces, symbols, and special characters. Prefer file names that are all lower case, and never
have names that differ only in their capitalization. Delimit words with `-` or `_`. Use `.R` as the extension of R files.

    ```
    # Good
    fit_models.R
    utility_functions.R
    exploratory-data-analysis.R

    # Bad
    fit models.R
    foo.r
    ExploratoryDataAnalysis.r
    ```

2.  File names should be **human readable**: use file names to describe what's in the file.

    ```
    # good
    report-draft-notes.txt

    # bad
    temp.r
    ```

    Use the same structure for closely related files:

    ```
    # good
    fig-eda.png
    fig-model-3.png

    # bad
    figure eda.PNG
    fig model three.png
    ```

3.  File names should play well with default ordering. If your file names contain dates, use yyyy-mm-dd (ISO8601) format so they sort in chronological order. If your file names include numbers, make sure to pad them with the appropriate number of zeros so that (e.g.) 11 doesn't get sorted before 2. If files should be used in a specific order, put the number at the start, not the end.

    ```
    # good
    01-load-data.R
    02-exploratory-analysis.R
    03-model-approach-1.R
    04-model-approach-2.R
    2025-01-01-report.Rmd
    2025-02-01.report.Rmd

    # bad
    alternative model.R
    code for exploratory analysis.r
    feb 01 report.Rmd
    jan 01 report.Rmd
    model_first_try.R
    run-first.r
    ```

##### Internal structure

Use commented lines of `-` and `=` to break up your file into easily readable
chunks.

```
# Load data ---------------------------

# Plot data ---------------------------
```

If your script uses add-on packages, load them all at once at the very
beginning of the file. This is more transparent than sprinkling `library()`
calls throughout your code or having hidden dependencies that are loaded in a
startup file, such as `.Rprofile`.


#### Syntax

##### Object names 

Variable and function names should use only lowercase letters, numbers, and `_`.
Use underscores (`_`) (so called snake case) to separate words within a name.

```{r}
# Good
day_one
day_1

# Bad
DayOne
dayone
```


Generally, variable names should be nouns and function names should be verbs.
Strive for names that are concise and meaningful.

```{r}
# Good
day_one

# Bad
first_day_of_the_month
djm1
```

##### Spacing

- Always put a space after a comma, never before, just like in regular English.

- Do not put spaces inside or outside parentheses for regular function calls.

- Place a space before and after `()` when used with `if`, `for`, or `while`.

- Place a space after `()` used for function arguments.

- The embracing operator, `{{ }}`, should always have inner spaces to help emphasise its special behaviour.

- Most infix operators (`==`, `+`, `-`, `<-`, etc.) should always be surrounded by spaces.

- There are a few exceptions, which should never be surrounded by spaces:

	- The operators with [high precedence][syntax]: `::`, `:::`, `$`, `@`, `[`,
    `[[`, `^`, unary `-`, unary `+`, and `:`.
	- Single-sided formulas when the right-hand side is a single identifier.
	- When used in tidy evaluation `!!` (bang-bang) and `!!!` (bang-bang-bang)
    (because they have precedence equivalent to unary `-`/`+`).
    - The help operator.

- Adding extra spaces is ok if it improves alignment of `=` or `<-`.
- Do not add extra spaces to places where space is not usually allowed.

##### Vertical space

- Use vertical whitespace sparingly, and primarily to separate your "thoughts" in code, much like paragraph breaks in prose.

- Avoid empty lines at the start or end of functions.
- Only use a single empty line when needed to separate functions or pipes.
- It often makes sense to put an empty line before a comment block, to help visually connect the explanation with the code that it applies to.

##### Function calls

###### Named arguments {#argument-names}

A function's arguments typically fall into two broad categories: one supplies
the __data__ to compute on; the other controls the __details__ of computation.
When you call a function, you typically omit the names of data arguments,
because they are used so commonly. If you override the default value of an
argument, use the full name:

```{r}
# Good
mean(1:10, na.rm = TRUE)

# Bad
mean(x = 1:10, , FALSE)
mean(, TRUE, x = c(1:10, NA))
```

- Avoid partial matching, where you supply a unique prefix of a function argument.

```{r}
# Good
rep(1:2, times = 3)
cut(1:10, breaks = c(0, 4, 11))

# Bad
rep(1:2, t = 3)
cut(1:10, br = c(0, 4, 11))
```

###### Assignment

Avoid assignment in function calls:

```{r}
# Good
x <- complicated_function()
if (nzchar(x) < 1) {
  # do something
}

# Bad
if (nzchar(x <- complicated_function()) < 1) {
  # do something
}
```

The only exception is in functions that capture side-effects:

```{r}
output <- capture.output(x <- f())
```

###### Long function calls

- Strive to limit your code to 80 characters per line. 
- If a function call is too long to fit on a single line, use one line each for
the function name, each argument, and the closing `)`.
This makes the code easier to read and to change later.

```{r}
# Good
do_something_very_complicated(
  something = "that",
  requires = many,
  arguments = "some of which may be long"
)

# Bad
do_something_very_complicated("that", requires, many, arguments,
                              "some of which may be long"
                              )
```


##### Braced expressions 

Braced expressions, `{}`, define the most important hierarchy of R code, allowing you to group multiple R expressions together into a single expression. 

To make this hierarchy easy to see:

- `{` should be the last character on the line.
  Related code (e.g., an `if` clause, a function declaration, a trailing comma, ...) must be on the same line as the opening brace.

- The contents should be indented by two spaces.

- `}` should be the first character on the line.

```{r}
# Good
if (y < 0 && debug) {
  message("y is negative")
}

if (y == 0) {
  if (x > 0) {
    log(x)
  } else {
    message("x is negative or zero")
  }
} else {
  y^x
}

test_that("call1 returns an ordered factor", {
  expect_s3_class(call1(x, y), c("factor", "ordered"))
})

tryCatch(
  {
    x <- scan()
    cat("Total: ", sum(x), "\n", sep = "")
  },
  interrupt = function(e) {
    message("Aborted by user")
  }
)

# Bad
if (y < 0 && debug) {
message("Y is negative")
}

if (y == 0)
{
    if (x > 0) {
      log(x)
    } else {
  message("x is negative or zero")
    }
} else { y ^ x }
```

It is occasionally useful to have empty braced expressions, in which case it should be written `{}`, with no intervening space.

```{r}
# Good
function(...) {}

# Bad
function(...) { }
function(...) {

}
```

##### Control flow

###### Loops

R defines three types of looping constructs: `for`, `while`, and `repeat` loops.

- The body of a loop must be a braced expression.

- It is occasionally useful to use a `while` loop with an empty braced expression body to wait. There should be no space within the `{}`.

###### If statements

- A single line if statement must never contain braced expressions. You can use
  single line if statements for very simple statements that don't have
  side-effects and don't modify the control flow.

- A multiline if statement must contain braced expressions.

- When present, `else` should be on the same line as `}`.

- Avoid implicit type coercion (e.g. from numeric to logical) in the condition of an if statement:

- `&` and `|` should never be used inside of an `if` clause because they can return vectors. Always use `&&` and `||` instead.

- `ifelse(x, a, b)` is not a drop-in replacement for `if (x) a else b`. `ifelse()` is vectorised (i.e. if `length(x) > 1`, then `a` and `b`  will be recycled to match) and it is eager (i.e. both `a` and `b` will always be evaluated).

###### Control flow modifiers {#control-flow-modifiers}

Syntax that affects control flow (like `return()`, `stop()`, `break`, or `next`) should always go in their own `{}` block:

###### Switch statements

- Avoid position-based `switch()` statements (i.e. prefer names).
- Each element should go on its own line unless all element can fit on one line.
- Elements that fall through to the following element should have a space after `=`.
- Provide a fall-through error unless you have previously validated the input.

##### Semicolons

- Semicolons are never recommended. In particular, don't put `;` at the end of a line, and don't use `;` to put multiple commands on one line.

##### Assignment

Use `<-`, not `=`, for assignment.

##### Data

###### Character vectors

- Use `"`, not `'`, for quoting text. The only exception is when the text already contains double quotes and no single quotes.


###### Logical vectors

- Prefer `TRUE` and `FALSE` over `T` and `F`.

##### Comments

- Each line of a comment should begin with the comment symbol and a single space: `# `

#### Functions

##### Naming

- Strive to use verbs for function names:

```{r}
# Good
add_row()
permute()

# Bad
row_adder()
permutation()
```

##### Anonymous functions

- Use the new lambda syntax: `\(x) x + 1` when writing short anonymous functions (i.e. when you define a function in an argument without giving it an explicit name).


- Don't use `\()` for multi-line functions or when creating named functions:

- Avoid using `\()` in a pipe, and remember to use informative argument names.

##### Multi-line function definitions

- There are two options if the function name and definition can't fit on a single line. In both cases, each argument goes on its own line; the difference is how deep you indent it and where you put `)` and `{`:

	- **Single-indent**: indent the argument name with a single indent (i.e. two spaces).
    The trailing `)` and leading `{` go on a new line.

    ```{r}
    # Good
    long_function_name <- function(
      a = "a long argument",
      b = "another argument",
      c = "another long argument"
    ) {
      # As usual code is indented by two spaces.
    }
    ```

	- **Hanging-indent**: indent the argument name to match the opening `(` of `function`.
    The trailing `)` and leading `{` go on the same line as the last argument.

    ```{r}
    # Good
    long_function_name <- function(a = "a long argument",
                                   b = "another argument",
                                   c = "another long argument") {
      # As usual code is indented by two spaces.
    }
    ```

These styles are designed to clearly separate the function definition from its body.

```{r}
# Bad
long_function_name <- function(a = "a long argument",
  b = "another argument",
  c = "another long argument") {
  # Here it's hard to spot where the definition ends and the
  # code begins, and to see all three function arguments
}
```

If a function argument can't fit on a single line, this is a sign you should rework the argument to keep it [short and sweet](https://design.tidyverse.org/defaults-short-and-sweet.html).

##### S7

In S7, the method definition can be long because the function name is replaced by a method call that specifies the generic and dispatch classes. In this case we recommend the single-indent style.

```{r}
method(from_provider, list(openai_provider, class_any)) <- function(
  provider,
  x,
  ...,
  error_call = caller_env()
) {
  ...
}
```

If the method definition is too long to fit on one line, use the usual rules to
spread the method arguments across multiple lines:

```{r}
method(
  from_provider,
  list(openai_provider, class_any, a_very_long_class_name)
) <- function(
  provider,
  x,
  ...,
  error_call = caller_env()
) {
  ...
}
```

##### `return()`

Only use `return()` for early returns. Otherwise, rely on R to return the result
of the last evaluated expression.

```{r}
# Good
find_abs <- function(x) {
  if (x > 0) {
    return(x)
  }
  x * -1
}
add_two <- function(x, y) {
  x + y
}

# Bad
add_two <- function(x, y) {
  return(x + y)
}
```

Return statements should always be on their own line because they have important effects on the control flow. See also [control flow modifiers](#control-flow-modifiers).

```{r}
# Good
find_abs <- function(x) {
  if (x > 0) {
    return(x)
  }
  x * -1
}

# Bad
find_abs <- function(x) {
  if (x > 0) return(x)
  x * -1
}
```

If your function is called primarily for its side-effects (like printing,
plotting, or saving to disk), it should return the first argument invisibly.
This makes it possible to use the function as part of a pipe. `print` methods
should usually do this, like this example from [httr](http://httr.r-lib.org/):

```{r}
print.url <- function(x, ...) {
  cat("Url: ", build_url(x), "\n", sep = "")
  invisible(x)
}
```

##### Comments

In code, use comments to explain the "why" not the "what" or "how". Each line
of a comment should begin with the comment symbol and a single space: `# `.

```{r}
# Good

# Objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)


# Bad

# Recurse only with bare lists
x <- map_if(x, is_bare_list, recurse)
```

Comments should be in sentence case, and only end with a full stop if they
contain at least two sentences:

```{r}
# Good

# Objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)

# Do not use `is.list()`. Objects like data frames must be treated
# as leaves.
x <- map_if(x, is_bare_list, recurse)


# Bad

# objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)

# Objects like data frames are treated as leaves.
x <- map_if(x, is_bare_list, recurse)
```


#### Pipes

##### Introduction

Use `|>` to emphasise a sequence of actions, rather than the object that the actions are being performed on.

The tidyverse has been designed to work particularly well with the pipe, but you can use it with any code, particularly in conjunction with the `_` placeholder.

```{r}
strings |>
  str_replace("a", "b") |>
  str_replace("x", "y")

strings |>
  gsub("a", "b", x = _) |>
  gsub("x", "y", x = _)
```

Avoid using the pipe when:

* You need to manipulate more than one object at a time. Reserve pipes for a
  sequence of steps applied to one primary object.

* There are meaningful intermediate objects that could be given
  informative names.

##### Whitespace

`|>` should always have a space before it, and should usually be followed by a new line. After the first step, each line should be indented by two spaces. This structure makes it easier to add new steps (or rearrange existing steps) and harder to overlook a step.

```{r}
# Good
iris |>
  summarize(across(where(is.numeric), mean), .by = Species) |>
  pivot_longer(!Species, names_to = "measure", values_to = "value") |>
  arrange(value)

# Bad
iris |> summarize(across(where(is.numeric), mean), .by = Species) |>
pivot_longer(!Species, names_to = "measure", values_to = "value") |>
arrange(value)
```

##### Long lines

If the arguments to a function don't all fit on one line, put each argument on
its own line and indent:

```{r}
# Good
iris |>
  summarise(
    Sepal.Length = mean(Sepal.Length),
    Sepal.Width = mean(Sepal.Width),
    .by = Species
  )

# Bad
iris |>
  summarise(Sepal.Length = mean(Sepal.Length), Sepal.Width = mean(Sepal.Width), .by = Species)
```

For data analysis, we recommend using the pipe whenever a function needs to span multiple lines, even if it's only a single step.

```{r}
# Bad
summarise(
  iris,
  Sepal.Length = mean(Sepal.Length),
  Sepal.Width = mean(Sepal.Width),
  .by = Species
)
```

##### Short pipes

It's ok to write a short pipe on a single line:

```{r}
# Ok
iris |> subset(Species == "virginica") |> _$Sepal.Length
iris |> summarise(width = Sepal.Width, .by = Species) |> arrange(width)
```

But because short pipes often become longer pipes, we recommend that you generally stick to one function per line:

```{r}
# Better
iris |>
  subset(Species == "virginica") |>
  _$Sepal.Length

iris |>
  summarise(width = Sepal.Width, .by = Species) |>
  arrange(width)
```

Sometimes it's useful to include a short pipe as an argument to a function in a
longer pipe. Carefully consider whether the code is more readable with a short
inline pipe (which doesn't require a lookup elsewhere) or if it's better to move
the code outside the pipe and give it an evocative name.

```{r}
# Good
x |>
  semi_join(y |> filter(is_valid))

# Ok
x |>
  select(a, b, w) |>
  left_join(y |> select(a, b, v), join_by(a, b))

# Better
x_join <- x |> select(a, b, w)
y_join <- y |> select(a, b, v)
left_join(x_join, y_join, join_by(a, b))
```

##### Assignment

There are three acceptable forms of assignment:

*   Variable name and assignment on separate lines:

    ```{r}
    iris_long <-
      iris |>
      gather(measure, value, -Species) |>
      arrange(-value)
    ```

*   Variable name and assignment on the same line:

    ```{r}
    iris_long <- iris |>
      gather(measure, value, -Species) |>
      arrange(-value)
    ```

*   Assignment at the end of the pipe with `->`:

    ```{r}
    iris |>
      gather(measure, value, -Species) |>
      arrange(-value) ->
      iris_long
    ```

I think that the third is the most natural to write, but makes reading a little
harder: when the name comes first, it can act as a heading to remind
you of the purpose of the pipe.

##### magrittr

We recommend you use the base `|>` pipe instead of magrittr's `%>%`.

```{r}
# Good
iris |>
  summarise(width = Sepal.Width, .by = Species) |>
  arrange(width)

# Bad
iris %>%
  summarise(width = Sepal.Width, .by = Species) %>%
  arrange(width)
```

As of R 4.3.0, the base pipe provides all the features from magrittr that we recommend using.

#### ggplot2


##### Introduction

Styling suggestions for `+` used to separate ggplot2 layers are very similar to those for `|>` in pipelines.


##### Whitespace

`+` should always have a space before it, and should be followed by a new line. This is true even if your plot has only two layers. After the first step, each line should be indented by two spaces.

If you are creating a ggplot off of a dplyr pipeline, there should only be one level of indentation.

```{r}
# Good
iris |>
  filter(Species == "setosa") |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()

# Bad
iris |>
  filter(Species == "setosa") |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
    geom_point()

# Bad
iris |>
  filter(Species == "setosa") |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) + geom_point()
```


##### Long lines

If the arguments to a ggplot2 layer don't all fit on one line, put each argument on its own line and indent:

```{r}
# Good
iris |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length, color = Species)) +
  geom_point() +
  labs(
    x = "Sepal width, in cm",
    y = "Sepal length, in cm",
    title = "Sepal length vs. width of irises"
  )

# Bad
iris |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length, color = Species)) +
  geom_point() +
  labs(x = "Sepal width, in cm", y = "Sepal length, in cm", title = "Sepal length vs. width of irises")
```

ggplot2 allows you to do data manipulation, such as filtering or slicing, within the `data` argument. Avoid this, and instead do the data manipulation in a pipeline before starting plotting.

```{r}
# Good
iris |>
  filter(Species == "setosa") |>
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()

# Bad
ggplot(filter(iris, Species == "setosa"), aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()
```
