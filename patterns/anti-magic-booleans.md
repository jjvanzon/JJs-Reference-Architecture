---
title: "🪄 Magic Booleans"
image: "/images/magic-boolean.png"
description: "Software programming C# syntax, passing a boolean parameter by name with no value"
keywords:
  - syntax
  - boolean
  - bool
  - design patterns
  - code samples
  - c#
  - .net
  - coding
  - programming
  - software engineering
  - software development
  - software design
  - software architecture
  - software
  - computers
---

`[ Draft ]`

🪄 Anti-Magic Booleans
=================

[back](other.md)

C# syntax, passing a boolean parameter by name with no value.

<img src="../images/magic-boolean.png" width="400" />


<h2>Contents</h2>

- [Intro](#intro)
    - [Magic Booleans](#magic-booleans)
        - [Outtakes](#outtakes)


Intro
-----

Allows for a syntax that looks like a boolean parameter that you do not have to assign a value to:

```cs
Contains(matchCase)
Contains(matchCase: true)
Contains(matchCase: false)
```

Instead of a `bool` flag, you can use a single-valued enum e.g.,

```cs
enum SpaceMatters { spaceMatters = 1 }
```

This lets callers write:

```cs
Has(text, spaceMatters)
```

without `: true` for cleaner syntax, instead of:

```cs
Has(text, spaceMatters: true)
```

The explicit boolean value option is still available. That's what makes it look like a magic boolean, of which you can leave out the value.

A side-effect of this pattern, is that the overload that takes the magic boolean, does not have to swich on `true`/`false`. Instead, it can assume the flag is set, creating opportunities for optimization as a bonus.

Combinations of flags might be comma `,` separated:

```cs
"a".In("a", "b", "c", spaceMatters, caseMatters);
```

This was preferred over the a flags enum notation `spaceMatters | caseMatters` for its simplicity and accessibility as well as speed, since `ifs` on flags can be omitted in the method implementations that offer these magic booleans. It does, in turn, require again more overloads.

### Magic Booleans

Today I want to talk about a software programming pattern that fell out of my sleeve, that I will call: ✨ Magic Booleans 🪄

This pattern allows you to use this syntax:

Contains(matchCase, trimSpace)

As shorthand along with:

Contains(matchCase: true, trimSpace: true)
Contains(true, true)

This looks like you are passing booleans without having to state their value. I wanted something analogus to attributes from HTML that do not have to specify a value like <input type="text" readonly /> (You could also use readonly="true" but it is not necessary.)

-----

This all has to do with the kind of ugginess of boolean flags:

Contains(matchCase: true) is verbose.
Contains(true) is vague.

Using Booleans makes sense, but the syntax is either too vague or feels obtrusive for a flag. I guess we sometimes settle for the vague shorter notation Contains(true), but you can't read from that if true means to match case or to actually ignore case instead!

-----

An alternative you might see, is to use enums:

[Flags] enum ContainsFlags { None, MatchCase, TrimSpace }

Now you might say:

Contains(ContainsFlags.MatchCase | ContainsFlags.TrimSpace)

Actually these days this is also possible:

Contains(MatchCase | TrimSpace)

(If you add this somewhere in your code files: global using static ContainsFlags;)

Actually this looks much neater, but some people still hate using static after its was introduced, and will insist on using the (very) long version "for clarity" or whatever.

You cannot separate by comma in this case, so this wouldn't work:

Contains(MatchCase, TrimSpace)

-----

Why couldn't you just use:

Contains(matchCase, trimSpace)

Well you can, but you have to break a few "style rules".
We'll still (ab)use enums...

-----

Here are the enums:

public enum MatchCase { matchCase }
public enum TrimSpace { trimSpace }

You can make a few Contains overloads:

public bool Contains() { ... }
public bool Contains(MatchCase matchCase) { ... }
public bool Contains(TrimSpace trimSpace) { ... }
public bool Contains(MatchCase matchCase, TrimSpace trimSpace) { ... }
public bool Contains(bool matchCase, bool trimSpace) { ... }

Oof, I guess I do like my overloads.

-----

Now we can call any of these syntaxes:

Contains()
Contains(matchCase)
Contains(trimSpace)
Contains(matchCase, trimSpace)

-----

It does require some (global) static usings though:

GlobalUsings.cs:

global using static TheNameSpace.MatchCase;
global using static TheNameSpace.TrimSpace;

-----

You can still use the explicit booleans:

Contains(matchCase: true, trimSpace: true)
Contains(true, true)

This depending on the syntax brevity and hard-coded/variable style that works best for your code.

-----

There is one added benefit to this that you wouldn't get otherwise (BONUS). They allow for a performance boost, over any other use of flags. This is because the magic bool has its own overload:

public bool Contains(MatchCase matchCase) { ... }

The implementation doesn't actually need an if statement for the flag. It doesn't even use this parameter. The C# compiler only use it to lead the call to this overload, that fully specializes for having the flag ON.

-----

So this is one of my little adventures trying to offer simpler syntaxes in my APIs. I'd like to thank C# for its new features, while also showing it one of my fingers at the same time. (We roll like that.)

Happy coding everybody and see you next time! Bye! 👋

#dotnet #csharp #softwarearchitecture #coding #designpatterns

#### Outtakes

`[ NOTE: These may be moved back in: used to be outtakes for a social post. ]`

`public bool Contains(TrimSpace trimSpace, MatchCase matchCase);`
`public bool Contains(bool trimSpace, bool matchCase);`

('Fun fact': Using the parameter name `Contains(matchCase: true)` wasn't even an option in earlier versions of  C#.)

A comma might look a little more intuitive, and you can indeed accomplish this by changing Contains' method definition, and this is all part of the myriad of non-standardized choices for using flags.


`using static MatchCase;`
`using static TrimSpace;`

You could declare these globally for ease, so you only need to do it once in your project:

 none of which is preferred over the other

(You might need to add your namespace in front of MatchCase or TrimSpace.)

So now we have our magic boolean, that appears to not need a value assigned:

`Contains(matchCase)`
`Contains(matchCase: true)`
`Contains(matchCase: false)`

Is this easier? Well using it seems easier:

`Contains(matchCase)`

(If you are ok also adding `global using TheNameSpace.MatchCase` to your project.)

It requires quite trickery in the implementation of our `Contains` method, but if that's what you want your API to do, then you CAN actually do it. Plus: Extra performance! Whoohoo!

[back](other.md)