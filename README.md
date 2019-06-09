# Ayte :: Utility

[![CircleCI](https://img.shields.io/circleci/project/github/ayte-io/java-utility.svg?style=flat-square)](https://circleci.com/gh/ayte-io/java-utility)
[![Maven Central](https://img.shields.io/maven-central/v/io.ayte.utility/parent.svg?style=flat-square)](https://mvnrepository.com/artifact/io.ayte.utility)

[![MIT License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE-MIT)
[![UPL-1.0 License](https://img.shields.io/badge/license-UPL&dash;1.0-brightgreen.svg?style=flat-square)](LICENSE-UPL-1.0)

## Where am I?

![](resources/kermit.jpg)

This is central repository that aggregates various Ayte utility 
libraries for Java. Because of NIH syndrome, some OCD regarding naming
and, finally, some missing functionality in common libraries it has been
decided to create our own ones.

## What do we try to solve here?

- Common boilerplate (`optional -> optional.orElse(null)`, 
`list -> list.stream().anyMatch(predicate)`)
- Debug unfriendliness (`MinTest$lambda@2640` - what the hell does this 
thing do?)
- Hard to guess and/or long utility functionality names (`materialize` 
instead of `emptyIfNull`)
- Porting some common functionality (e.g. `anyMatch()` for iterable)

## Components

- [value][] - some data interfaces/classes like Pair, Trio and so on
- [supplier][] - common implementations for 
`java.util.function.Supplier` interface
- [fabricator][] - interface identical to `Supplier`, but which may 
throw (for example, InputStream providers may be exposed as such 
Fabricators that throw IOExceptions), as well as common implementations
- [task][] - Interface that sits between `Runnable` and `Callable` - it
doesn't return value, but can throw typed exception
- [action][] - Interface similar to function with throw support
- [predicate][] - Collection of common predicate implementations, as
well as TernaryPredicate support
- [function][] - Collection of common function implementations
- [collection][] - Classic NIH collection-related static helper methods
- [string][] - Same, but for strings

## How to use this stuff

### Coordinates

Every component is split into `api` and `kit` packages. `api` package 
contains all the interfaces, while `kit` contains implementations - this 
is done so anyone who needs just interfaces wouldn't have to pull bunch 
of unnecessary classes.

Every library is exposed as two artifacts, 
`io.ayte.utility.<library>:api` and `io.ayte.utility.<library>:kit`, 
which contain `io.ayte.utility.<library>.api` and
`io.ayte.utility.<library>.kit` packages, as well as same-named modules,
and kit module requires transitive api module.

Some libraries may miss one of two artifacts, but that's OK.

There are also aggregate artifacts `io.ayte.utility:api` and 
`io.ayte.utility:kit` that simply aggregate all other packages. They 
also expose `io.ayte.utility.api` and `io.ayte.utility.kit` modules that
transitively require all other modules.

### Static classes

Internal functionality is exposed through classes with static methods.
They usually provide static factories for interfaces maintained by
component and thus most often are called `Xs`, where `X` is interface
name `Xs` provides factories for - `Pairs`, `Fabricators` and so on. 

### Interfaces

As stated above, interfaces reside in `api` components. They can be used 
separately (quite often you need just `TernaryFunction` interface 
without any additional weight).

#### AugmentedX

`AugmentedX` names are used to extend `X` interface with default 
methods overriden so they would use named classes instead of lambdas.
They reside in `kit` packages, since they depend on actual 
implementations.

### AmplifiedX

`AmplifiedX` names are used to resolve possible name collision, for 
example `AmplifiedCollections` may exist because otherwise it would 
collide with `java.util.Collections`.

### Backward compatibility

Blah-blah-blah we will put all the effort to preserve it. Deprecated
things will stay for one major release, and then they'll be gone. Please
note that according to semantic versioning every 0.x release is 
treated as major release.

## Conventions

### Priorities

- Developer friendliness
- Java 8 compatibility, but Java 9+ readiness (modularization)
- Proper naming
- Proper splitting to reduce unnecessary dependencies and possibility of 
dependency hell
- Eradication of long and unpleasant but common lambdas
- Backward compatibility
- Code simplicity

### Named classes

Every function, predicate and so on should be exposed as named class.
This is somewhat contradictory to what lambdas were intended to bring 
in, but when it comes to lambda world, it becomes hard to debug: all you
see in debugger is `<ClassName>$lambda@<hashcode>` and captured 
variables, so it is hard to tell what lambda does, even if it's just
`x -> x * 2`. Because of that, all that simple stuff - `Xor`, `Not`,
`GreaterThan`, `Min` and so on should be exposed as named classes, just
to ease the pain of lambda debugging where small effort makes great 
profit. It is much easier to read complex predicate constructions when
they are named, `Not(And(GreaterThan(3), Even, ElementOf(6, 9, 12)))` is
way way way easier to understand than lambda composition.

### Static constructors exposed through helper classes

Whole functionality should be exposed through static utility classes, so
real constructors and required computations are called in shadows. This
allows some simplicity hacks, for example operation *verify that all 
predicates in collection respond with true* can be substituted just with 
first predicate of such collection if it's size is 1, and can be reduced
to constant true if collection is empty. Everything but such static 
classes or explicitly exposed functionality is considered internal even
if you are writing non-modularized code.

### Backward compatibility

Just don't break it.

### Contract tests

Lots of tests look pretty much redundant and only test contracts of
classes. This is done just to prevent any modifications that would 
break those contracts, even if this is unlikely.

### Performance friendliness

Last & least, wherever possible, try to write performant code, but 
remember that compiler is smarter than you.

## Licensing

MIT / UPL-1.0

Ayte Labs, 2019

Have fun & do whatever you want, for free.

  [value]: https://github.com/ayte-io/java-utility-value
  [supplier]: https://github.com/ayte-io/java-utility-supplier
  [fabricator]: https://github.com/ayte-io/java-utility-fabricator
  [task]: https://github.com/ayte-io/java-utility-task
  [action]: https://github.com/ayte-io/java-utility-action
  [predicate]: https://github.com/ayte-io/java-utility-predicate
  [function]: https://github.com/ayte-io/java-utility-function
  [collection]: https://github.com/ayte-io/java-utility-collection
  [string]: https://github.com/ayte-io/java-utility-string
