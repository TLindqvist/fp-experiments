# 1 Basics

## 1.1 Reduction

One of the most basic concepts hen programming in any higher-level language comes from mathematics and is one that we often take for granted. That is **reduction**. According to Cambridge dictionary it is "the act of making something, or of smoething bevoming, smaller in size, amount[...]". In mathematics it means to reqrite an expression in simpler form. For example:

`2 * (1 + 3)` can be reduced to `2 * 4` which in turn can be reduced `8`.

When programming with expressions reducing is often called **evaluating**. Some languages like F# is "eagerly evaluated". This means that when the code in a source file is run from top to bottom, all expressions are evaluated. This is in contrast to "lazily evaluated" languages where expressions are only evaluated when needed.

## 1.2 Monoids

Monoids is an interestig concept in mathematics. It sounds like somethng complex but is in fact quite simple. For most of us, it is something we use every day without thinking about it. When programming it is also very useful.

## 1.3 A new of the same kind

Addition is well-known to most of us. Let's startup our F# REPL and look at an example:

```fs
> 1 + 2;;
val it: int = 3
```

We add two integers together and the result is a new integer. Let's try the same with strings. That is called concatenation:

```fs
> "Hello" + "!!!";;
val it: string = "Hello!!!"
```

It is the same thing. We add two strings together and the result is a new string. What about lists? `+` as an operator in F# does ont work on lists. Instead we can use the `@` operator:

```fs
> [1; 2] @ [3; 4];;
val it: int list = [1; 2; 3; 4]
```

Nice! That worked as well. We added two lists together and the result was a new list. Let's take a final example, we will add two functions together. This is called composition in the functional programming world. Here we need another operator, `>>`:

```fs
> let add1 x = x + 1;;
val add1: x: int -> int

> let double x = x + x;;
val double: x: int -> int

> let add1ThenDouble = add1 >> double;;
val add1ThenDouble: (int -> int)

> add1ThenDouble 1;;
val it: int = 4
```

Seeing this commonality is knowledge that is good to know as a programmer. It will help us in many ways throughout our career. Mathematicians calls this the **closure** property.

## 1.4 The same value

There is another pattern we can find. If we do addition with `0` we get the same value back. This is true for all types we have looked at so far. Let's try it out:

```fs
> 3 + 0;;
val it: int = 3
```

When concatenating strings we get the same behavior as well. For string we use the empty string `""` instead of `0`;

```fs
> "Hello" + "";;
val it: string = "Hello"
```

Also for lists the empty list `[]` produces the same pattern:

```fs
> [1; 2] @ [];;
val it: int list = [1; 2]
```

Finally we will test that the same applies to composition of functions. Here we compose with a function that return the same value:

```fs
> let add1 x = x + 1;;
val add1: x: int -> int

> let identical x = x;;
val identical: x: 'a -> 'a

> let add1' = add1 >> identical;;
val add1': (int -> int)

> add1' 2;;
val it: int = 3
```

As a bonus I will throw in `1` that when used in multiplication also produces the same value:

```fs
> 5 * 1;;
val it: int = 5
```

There is a name for this type of "element" in mathematics. It is called the **identity** element. It is a value that when combined with another value produces the same value. In the case of addition we saw that the identity element is `0`. For multiplication it is `1`. For strings it is `""` and for lists it is `[]`. For functions it is a function that returns the same value as it gets as input.

## 1.5 Left and right identity

Let us take the examples in the previous section and see that it does not matter if we add `0` to `2` or `2` to `0`. The result is the same. This is called the **left identity** and **right identity**.

Addition:

```fs
> 0 + 2 = 2 + 0;;
val it: bool = true
```

Mulitplication:

```fs
> 1 * 2 = 2 * 1;;
val it: bool = true
```

String concatenation:

```fs
> "Hello"  + "" = "" + "Hello";;
val it: bool = true
```

List concatenation:

```fs
> [1; 2] @ [] = [] @ [1; 2];;
val it: bool = true
```

Function composition:

```fs
> let add1 x = x + 1;;
val add1: x: int -> int

> let identical x = x;;
val identical: x: 'a -> 'a

> let add1' = add1 >> identical;;
val add1': (int -> int)

> let add1'' = identical >> add1;;
val add1'': (int -> int)

> add1' 1 = add1'' 1;;
val it: bool = true
```

## 1.6 Associativity

The last property we will look at is **associativity**. This means that it does not matter in which order we combine the values. In other words, how we group the operations does not matter. Note that associativity does say that the order does not matter. Let's look at our examples but extended with one more operation.

Addition:

```fs
> (1 + 2) + 3 = 1 + (2 + 3);;
val it: bool = true
```

Mulitplication:

```fs
> (2 * 3) * 4 = 2 * (3 * 4);;
val it: bool = true
```

String concatenation:

```fs
> ("Hello " + "world") + "!" = "Hello " + ("world" + "!");;
val it: bool = true
```

List concatenation:

```fs
> ([1; 2] @ [3; 4]) @ [5; 6] = [1; 2] @ ([3; 4] @ [5; 6]);;
val it: bool = true
```

Function composition:

```fs
> let toUpper (x: string) = x.ToUpper();;
val toUpper: x: string -> string

> let first (x: string) = x.Substring(0,1);;
val first: x: string -> string

> let toLower (x: string) = x.ToLower();;
val toLower: x: string -> string

> let version1 = (toUpper >> first) >> toLower;;
val version1: (string -> string)

> let version2 = toUpper >> (first >> toLower);;
val version2: (string -> string)

> version1 "test" = version2 "test";;
val it: bool = true
```

This is called associativity.

## 1.7 Commutativity

Commutativity is reminiscent of associativy at first glance but is not the same. Commutativity is a property that means that the order of the values does not matter. This is true for addition and multiplication but not for string concatenation, list concatenation and function composition. Note that commutativity does not say that how we group the operations does not matter.

Addition:

```fs
> 3 + 2 + 1 = 1 + 2 + 3;;
val it: bool = true
```

Multiplication:

```fs
> 3 * 2 * 1 = 1 * 2 * 3;;
val it: bool = true
```

But this is not a property that holds for string concatenation:

```fs
> "Hello" + "!" = "!" + "Hello";;
val it: bool = false
```

`Hello!` is not the same as `!Hello`. The same goes for list concatenation:

```fs
> [1; 2] @ [3; 4] = [3; 4] @ [1; 2];;
val it: bool = false
```

`[1; 2; 3; 4]` is not the same as `[3; 4; 1; 2]`. For function composition this property does not hold for every type of functions. In the following example it does not:

```fs
> let add1 a = x + 1;;
val add1: (int -> int)

> let double x  = x * 2;;
val double: (int -> int)

> let add1ThenDouble = add1 >> double;;
val add1ThenDouble: (int -> int)

let doubleThenAdd1 = double >> add1;;
val doubleThenAdd1: (int -> int)

> add1ThenDouble 1 = doubleThenAdd 1;;
val it: bool = false
```

Looks like the functions in this example works just like math.

# 1.8 Definition of a monoid

It turns out that in math things that has these properties are called monoids.

**A monoid is a set of values and an operation that:**

- **Combines two values and returns a new value.**
- **The set must also have an identity element.**
- **The operation must be associative.**

A monoid that is commutative is called a symmetric monoid.

# 1.9 The benefit of the closure property

So, what is the point of this?

The closure property gives us the ability to reduce a list of elements with a given operator.

For addition of integers this becomes:

```fs
> List.reduce (+) [1; 2; 3; 4];;
val it: int = 10
```

Strings are equally easy:

```fs
> List.reduce (+) ["Hello"; " "; "world"; "!"];;
val it: string = "Hello world!"
```

We can also reduce functions since they have the closure property:

```fs
> let add1 x = x + 1;;
val add1: x: int -> int

> let double x = x * 2;;
val double: x: int -> int

> let add1ThenDouble = List.reduce (>>) [add1; double];;
val add1ThenDouble: (int -> int)

> add1ThenDouble 3;;
val it: int = 8
```

We have seen that the benefits of the closure property is the we can take operations that work on to elements and make them work on any number of elements.

# 1.10 Identity element

The identity element gives us the possibility to reduce lists with zero elements. In F# for example, using `List.reduce` on an empty list will throw an exception. We can instead use `List.fold`. It works almost exactly as `List.reduce` but takes an extra argument, the "starting" value. Here we can use the identity element:

```fs
> List.reduce (+) [];;
System.ArgumentException: The input list was empty. (Parameter 'list')
   at Microsoft.FSharp.Collections.ListModule.Reduce[T](FSharpFunc`2 reduction, FSharpList`1 list) in D:\a\_work\1\s\src\FSharp.Core\list.fs:line 306
   at <StartupCode$FSI_0023>.$FSI_0023.main@() in C:\Users\TobiasLindqvist\src\pulstavlan\Pulstavlan.Client\stdin:line 26
   at System.RuntimeMethodHandle.InvokeMethod(Object target, Void** arguments, Signature sig, Boolean isConstructor)
   at System.Reflection.MethodBaseInvoker.InvokeWithNoArgs(Object obj, BindingFlags invokeAttr)
Stopped due to error

> List.fold (+) 0 [];;
val it: int = 0
```

# 1.11 The benefits of associativity

The associativity property gives us the ability to parallelize things. If the order does not matter, parallelization is possible. This can be huge, especially if combined with the benefits of the closure property. The work can be split between several cores or even computers.

If an operation also is commutative it will give us a lot of freedom when parallelizing. We can split the work in any way we want.

# 1.12 Pure functions

In functional programming we often prefer functions that are **pure**. By pure we mean a function that does not have any side effects. That is code that returns the same result every time it is called with the same arguments.

Examples of pure functions are:

```fs
let add x y = x + y

let greet name = "Hello " + name
```

It does not matter how many times or when you call the above functions they will always return the same result. This is often called **referential transparency**. There are however functions that does not always return the same result. Examples are:

```fs
let random = new System.Random();;
val random: System.Random

> let randomInt () = random.Next(1,10);;
val randomInt: unit -> int

> randomInt();;
val it: int = 8

> randomInt();;
val it: int = 10
```

`randomInt` does not return the same value every time it is called. In the same, way functions that loads from a database or get the current time are not pure.

Pure functions are preferred since they are easier in many ways. Easier to test and easier to parallelize. Since they always return the same value for the same input they can also be memoized. That is the first time the function is called, the calculation is done and for all future calls, the result is returned directly.

Another way to put it is that if a function can be replaced with a table lookup it is a pure function. Let's take logical AND as an example. It could be replaced with a dictionary like in the example below. This implies that passing a pure function as an argument to another function and reducing it is the same as passing data.

```fs
> let and' = Map [(false, false), false; (false, true), false; (true, false), false; (true, true), true];;
val and': Map<(bool * bool),bool> =
  map
    [((false, false), false); ((false, true), false); ((true, false), false);
     ((true, true), true)]

> and'[(false, true)] = false && true;;
val it: bool = true
```

## 1.13 A somewhat bigger example

Here is an example of how we can use the theoretical stuff above in a fictitious example. It is a complete example in the MapReduce pattern.

Let's say I want my sons to be better at programming. In order to do that I create a small exercise at home. I want them to calculate how many of apples we buy on average during a year. Average is interesting since it is an example of an operation that is commutative but not associative. In plain English it does not matter in what order we calculate how many apples we have bought but it matters how we group the operations.

In order to do it properly my sons have to sum up the total number of apples bought and the number of times we bought groceries at the store. The total number of apples bought is then divided by the number of times we bought groceries.

The data of our groceries are stored in a dictionary of lists. The dictionary is keyed by month and values are a list of trips to the store and the inner lists contains the bought fruits.

```fs
type Month = January | February | March | April | May | June | July | August | September | October | November | December

type Fruits = Apple | Orange | Banana

let groceries = Map [
      January,  [[Apple; Orange; Banana]; [Apple; Apple; Banana]; [Banana]; [Orange; Banana]]
      February, [[Banana; Apple; Apple; Banana]; []; [Banana; Orange; Banana; Orange]]
      March,    [[Apple; Banana]; [Apple; Banana; Banana; Orange]; []; [Banana]]
      April,    [[Orange]; [Banana]; [Apple; Apple]; [Banana; Banana; Orange; Banana]; []]
      // May and so on...
]
```

My sons who loves functional programming, wants to solve this with MapReduce. They choose to do it with the following four steps:

1. Create a function to map(transform) the data into something math works on (a monoid)
2. Create a binary operator that works on the new data (the monoid)
3. Create a list of the data
4. Reduce the list with the new operator
5. Complete the calculation

```cs
type FruitStatistics = { Apples: int; ShoppingOccurences: int }

// Step 1 - Function for converting an entry in the dictionary to a monoid
let monthStatistics data =
    { Apples = data
               |> List.collect id // Flatten the list of lists. id is the identity function.
               |> List.filter (fun x -> x = Apple) // Just keep apples
               |> List.length // Only apples left, count them
      ShoppingOccurences = data.Length }

// Step 2 - Function for combining two monoids
let combineStatistics left right =
    { Apples = left.Apples + right.Apples
      ShoppingOccurences = left.ShoppingOccurences + right.ShoppingOccurences }

// Step 3 - Reduce the groceries to the statistics we need
let statistics =
   groceries
   |> Map.values
   |> Seq.map monthStatistics
   |> Seq.reduce combineStatistics

// Step 4 - Calculate the average
var average = (float statistics.Apples) / (float statistics.ShoppingOccurences) // 0.5625
```

`data` innehåller nu en monoid som vi kan återanvända för nya beräkningar eller om vi vid ett senare tillfälle får två nya användare att ta med i beräkningen. Dock skall man komma ihåg att vi inte kan återanvända värdet om vi muterar de existerande användarnas register över lästa böcker.
