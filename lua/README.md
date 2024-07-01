# Horrible Fizz Buzz: Lua

This is a horrible implementation of the Fizz Buzz problem in Lua.

## How I did it

### AI Assistance

First, I wanted to ask Copilot to generate "a bad fizzbuzz implementation in
Lua". I would then use whatever it spat out as a basis for my terrible fizzbuzz
implementation, no matter how bad it came out. The goal here is to make the code
as bad as possible, so I'd take what I get... Unless what I got was good.

I quickly learned that Copilot isn't "allowed" to generate bad code, and has a
lot of synonyms for "bad" that it recognizes. So instead I turned to the option
I should have went with in the first place if I wanted bad code: ChatGPT.

I asked it the same thing, and it returned this:

<details> <summary>ChatGPT's code</summary>

```lua
-- Bad FizzBuzz implementation in Lua
for i = 1, 100 do
    local fizz = i % 3 == 0
    local buzz = i % 5 == 0

    if fizz and buzz then
        print("FizzBuzz")
    elseif fizz then
        print("Fizz")
    elseif buzz then
        print("Buzz")
    else
        print(i)
    end

    -- Unnecessary and inefficient conditional checks
    if i % 3 == 0 and i % 5 ~= 0 then
        print("Fizz")
    elseif i % 5 == 0 and i % 3 ~= 0 then
        print("Buzz")
    elseif i % 3 == 0 and i % 5 == 0 then
        print("FizzBuzz")
    end
end
```

</details>

And I don't like it. It looks like there are enough "proper" fizzbuzz
implementations that ChatGPT must have scanned through, so even though I asked
it to make something bad, it still made "basic fizzbuzz" -- except it just
prints everything twice.

So I asked it to make it worse, and it returned this:

<details> <summary>ChatGPT's worse code</summary>

```lua
-- Even Worse FizzBuzz implementation in Lua
for i = 1, 100 do
    local output = ""

    -- Unnecessary nested if-else structures
    if i % 3 == 0 then
        if i % 5 == 0 then
            output = output .. "FizzBuzz"
        else
            output = output .. "Fizz"
        end
    else
        if i % 5 == 0 then
            output = output .. "Buzz"
        else
            output = output .. tostring(i)
        end
    end

    -- Redundant checks and variable reassignments
    if i % 3 == 0 then
        output = "Fizz"
    end
    if i % 5 == 0 then
        output = "Buzz"
    end
    if i % 3 == 0 and i % 5 == 0 then
        output = "FizzBuzz"
    end
    if i % 3 ~= 0 and i % 5 ~= 0 then
        output = tostring(i)
    end

    -- Unnecessary loop inside the loop
    for j = 1, 1 do
        if i % 3 == 0 then
            if i % 5 == 0 then
                output = "FizzBuzz"
            else
                output = "Fizz"
            end
        else
            if i % 5 == 0 then
                output = "Buzz"
            else
                output = tostring(i)
            end
        end
    end

    print(output)
end
```

</details>

And it was indeed worse. But... I could see it was just repeating the same thing
over again. Instead of writing it twice, it'd write it three times -- except not,
it just rewrote the variable `output` every time, this time. It'd only print
the output once though.

[ChatGPT Thread](https://chatgpt.com/share/70c01f82-4bd1-41e1-94e7-15e068919efa)

### Nevermind, No More AI Assistance

Alright, "AI" isn't going to work. Let's move on, and take matters into my own
hands. I am going to make my own terrible fizzbuzz implementation from scratch.

#### Starting With Some Ground Rules

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.

With that in mind, let's get started. I'm going to start by breaking down the
basic components of every programming language: variables, functions, and
control structures. Then, I'll make a mess of them. Finally, I can put them all
together to make a horrible fizzbuzz implementation.

#### All Rules

Collected from all the sections below, these are *all* of the rules compiled
together.

<details> <summary>(spoiler) All Rules</summary>

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.
4. Variables must be limited according to the [variable rules](#variable-rules).
5. No usage of basic `if` statements allowed at all. See the
  [if statement examples](#if-statement-examples) for alternatives.
6. No usage of basic loop constructs allowed. Recursion must be used instead.
7. We must use `goto` 3 times, exactly. We can have any number of labels however.
8. We must use labels instead of comments.

</details>

##### Variables

Lua has a few different types of variables. Below are a list of some of the most
important ones:

- `nil`: The type of `nil` is `nil`. It is the absence of a value.
- `boolean`: The type of `true` and `false` is `boolean`. They are the two
  possible values of a boolean variable.
- `number`: The type of `1`, `2.5`, and `math.pi` is `number`. They are all
  numbers. Stored internally as doubles.
- `string`: The type of `"Hello, World!"` is `string`. It is a sequence of
  characters.
- `table`: The type of `{}` is `table`. It is a combination of both a hash map
  and an array.
- `function`: The type of `function() end` is `function`. It is a block of code
  that can be called.

So lets limit each of these types.

###### Variable Rules

- `nil`: Directly referencing `nil` is not allowed. We must use a non-existent
  variable instead. Prefer using a variable name that is close to some other
  variable name to cause confusion.
- `boolean`: We are not allowed to use `true` or `false` anywhere.
- `number`: All numeric operations must be performed with the inclusion of
  floating-point numbers in some way. This opens the path for some fun in the
  way of floating-point arithmetic errors.
- `string`: Built-in manipulation methods are not allowed. Concatenation and such
  must be done manually by building a table of characters and using `table.concat`.
  `table.concat` will be the *only* built-in function we are allowed to use for
  strings.
- `table`: We can only use tables as arrays. However, we cannot index using
  numbers. We must index using the *name* of the number. For example, `t[1]` is
  not allowed, but `t["one"]` is.
- `function`: Functions are not allowed to return any values, nor can they take
  any arguments.

So, new rule:

- Variables must be limited according to the variable rules.

<details> <summary>Current Rules</summary>

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.
4. ***Variables must be limited according to the [variable rules](#variable-rules).***

</details>

##### If Statements

Lua has really simple if statements, which is great for readability. We don't
want that though, so we're going to mess them up as much as possible.

I briefly considered using the "terrible if statements" library I made, called
[LuaIf](https://github.com/fatboychummy/LuaIf), but decided against it. When
using the library, you are still writing semi-readable code, it's just wrapped
in a lot of `function() ... end`s. This is not ideal.

So let's start by breaking down what an if statement *is*.

```lua
if condition then
  code()
elseif another_condition then
  more_code()
else
  even_more_code()
end
```

As we can see, there are three main parts of an if statement, and a few optional
extras:

1. The `if` keyword.
2. The condition.
3. The code block.
4. (Optional) The `elseif` keyword, another condition, and another code block.
5. (Optional) The `else` keyword and another code block.

But do we need to keep everything there? Lua has some syntactic sugar that we
can abuse to make our code look more complicated than it is. For example, we can
use the the short-circuit evaluation of `and` and `or` to make our own `if`
statements.

```lua
_= condition and code()
```

This is a valid Lua statement. If `condition` is truthy, then `code()` will be
called. If `condition` is falsy, then `code()` will not be called. This is
because Lua's `and` operator will only evaluate the right-hand side if the
left-hand side is truthy. If the left-hand side is falsy, then the right-hand
side is never evaluated.

We can do the same thing with `or` to add an `else` block.

```lua
_= condition and code() or else_code()
```

Same as above. If `condition` is truthy, `code()` will run. Otherwise,
short-circuit evaluation states that `code()` will not run, then the right-hand
side of the `or` will run, which is `else_code()`.

On the same note, due to short-circuit evaluation, `else_code()` runs *only* if
`condition` is falsy. If the left-hand side of the `or` is truthy, then the
right-hand side is never evaluated.

Appending else-*if*s instead is simple. Instead of `else_code()`, copy the left
and right of the `and` over.

```lua
_= condition and code() or another_condition and elseif_code() or else_code()
```

However, these have a caveat. If `code()` returns a falsy value, then
`else_code()` will run.

Thus, we can wrap this in a function that alkways returns a truthy value.

```lua
_= condition and (function() code() return true end)() or else_code()
```

This also applies to elseifs.

I use the above ideas in my LuaIf library, and they do indeed look quite
unreadable. Take a look at *this* example from within the library:

```lua
Else = function(func)
  local tmp = not condition and not kill and func and func()
  return not func and not condition and {End = If.End, If = function(c) return If.ThenC(c, kill) end}
   or not func and condition and {End = If.End, If = function() return If.ThenC(false, true) end}
   or {End = If.End}
end
```

Isn't that just beautifully chaotic?

But we can make them even worse. Like. What if, for example. We used something
that was never meant to be used as an if statement, as an if statemnet. Like a
loop?

```lua
for _ = 1, condition and 1 or 0 do
  code()
end
```

Interestingly, it may not look like we can have an else or elseif statement here,
but we can still do the same as we have done above with the `and` and `or`, so
long as we wrap the functions in something that returns `0`.

```lua
for _ = 1, condition and 1 or elseif_condition and (function() elseif_code() return 0 end)() or (function() else_code() return 0 end)() do
  code()
end
```

###### If Statement Examples

I decided to write up a bunch of different ways to do if statements...
```lua
-- if statements

-- "base" if statement
if condition then code() end

-- Basic short-circuit evaluation if statement
_ = condition and code()

-- if statement using numeric for loop and short-circuit evaluation
for i = 1, condition and 1 or 0 do code() end

-- if statement using generic for loop
for _ in pairs({condition or nil}) do code() end

-- if statement using generic for loop and coroutines
for _ in coroutine.wrap(function() coroutine.yield(condition or nil) end) do code() end

-- if statement using repeat until and short-circuit evaluation
repeat until condition and code() or true

-- if statement using while and short-circuit evaluation
while condition and code() and false do end
```

I am particularly a fan of the coroutine one. It's so... Unnecessary.

So, new rule:

- No usage of basic `if` statements allowed *at all*. See the
  [if statement examples](#if-statement-examples) for alternatives.

<details> <summary>Current Rules</summary>

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.
4. Variables must be limited according to the [variable rules](#variable-rules).
5. ***No usage of basic `if` statements allowed at all. See the***
  ***[if statement examples](#if-statement-examples) for alternatives.***

</details>

##### Loops

Recursion. Recursion is the answer. The *only* answer. Normal loop constructs
are not allowed (except when not being used as loops). We must use recursion
instead.

Do remember the function limitations as well, we cannot pass arguments to
functions, nor can we return values from them. So I will have fun with that.

So, new rule:

- No usage of basic loop constructs allowed. Recursion must be used instead.

<details> <summary>Current Rules</summary>

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.
4. Variables must be limited according to the [variable rules](#variable-rules).
5. No usage of basic `if` statements allowed at all. See the
  [if statement examples](#if-statement-examples) for alternatives.
6. ***No usage of basic loop constructs allowed. Recursion must be used instead.***

</details>

##### Miscellaneous Rules

There are a few more things we can do to make the code unreadable.

- We must use `goto` 3 times, exactly. We can have any number of labels however.
  In fact, we must use labels instead of comments.
- ?

<details> <summary>Current Rules</summary>

1. The code must be able to run.
2. The code must be as unreadable as possible.
3. The code must output according to the Fizz Buzz rules.
4. Variables must be limited according to the [variable rules](#variable-rules).
5. No usage of basic `if` statements allowed at all. See the
  [if statement examples](#if-statement-examples) for alternatives.
6. No usage of basic loop constructs allowed. Recursion must be used instead.
7. ***We must use `goto` 3 times, exactly. We can have any number of labels however.***
8. ***We must use labels instead of comments.***

</details>


#### Putting It All Together

See [fizzbuzz.lua](fizzbuzz.lua) for the final code.