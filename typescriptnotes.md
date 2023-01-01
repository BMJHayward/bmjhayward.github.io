###effective typescript

#### by Dan Vanderkam

Illustrates 62 specific ways to use/improve typescript.
Part of the `Effective X` series of programming books.

Discusses diff bt JS and TS. Structural typing and TS options, symbols can be in "type" space or "value" space. c.f. scala which has similar system

#### The Zen of TypeScript

Pythonistas may have heard of the "Zen of Python".
It looks like this:

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!

It's good programming advice generally. Unless your tools or languages give their own specific guidance, these aphorisms are a great fall back.

Well, the chapter titles of this book are just as instructional as the actual book, so I thought they could be shortlisted into their on TS-philosophy!

    Think of types as sets of values
    Prefer type declarations to type assertions
    Avoid cluttering your code with inferable types
    Understand type widening
    Understand type narrowing
    Create objects all at once
    Be consistent with your aliases
    Use async functions for async code, not callbacks
    Liberal in what you accept, strict in what you produce
    Don't repeat type information in documentation
    Prefer unions of interfaces to interfaces of unions
    Favour more precise alternative to string types
    Incomplete types are better than inaccurate types
    Generate types from APIs or specs, not data
    Name types using language of your problem domain
    Always use the narrowest possible scope
    Hide unsafe type assertions in well-typed functions
    Export all types that appear in public APIs
    Prefer ECMAscript features to JS features
    don't rely on private to hide information
    write modern JS
    Migration isn't complete until you enable `noImplicitAny`

There. Not a bad list. A bit more practical than the python version.

Type system in TS is not "sound". A "sound" type system is one that guarantees the accuracy of its static types. E.g. reason or elm.

## option monads
programming typescript book

```typescript
function flatten<T>(array: T[][]): T[] {
    return Array.prototype.concat.apply([], array)
}

interface Option<T> {
    flatMap<U>(f: (value: T) => None): None
    flatMap<U>(f: (value: T) => Option<U>): Option<U>
    getOrElse(value: T): T
}

class Some<T> implements Option<T> {
    constructor(private value :T){}
    flatMap<U>(f: (value: T) => None): None
    flatMap<U>(f: (value: T) => Some<U>): Some<U>
    flatMap<U>(f: (value: T) => Option<U>): Option<U> {
        return f(this.value)
    }
    getOrElse(value: T): T {
        return this.value
    }
}

class None implements Option<never> {
    flatMap(): None {
        return this
    }
    getOrElse(value: T): T {
        return value
    }
}

function Option<T>(value: null | undefined): None
function Option<T>(value: T): Some<T>
function Option<T>(value: T): Option<T> {
  if (value == null) {
    return new None
  }
  return new Some(value)
}

// how to use it
let result = Option(6)        // Some<number>
  .flatMap(n => Option(n * 3) // Some<number>
  .flatMap(n = > new None)    // None
  .getOrElse(7)               // 7
```


option monad video
channel: studying with alex
vid: best intro to monads
```typescript
interface Option<T> {}
class Some<T> implements Option<T> {}
class None<T> implements Option<T> {}

function some<T>(x: T): Option<T> { return new Some(x) };

function run<T>(
  input: Option<T>,
  transform: (_: T) => Option<T>
  ): Option<T> {
    if (input == none) {
      return none
    }
  return transform(input.value)
}

```
