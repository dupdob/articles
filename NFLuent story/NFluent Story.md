# NFluent, the seven year itch
## Abstract
In this serie of posts, I will present how NFluent has evolved throughout its history. I will mostly focus on the successive redesigns it went through, explaining why and how this redesigns happened.

While the idea of writing those posts is not new, the interest of doing it appeared to me while I was thinking about what NFluent V3 will look like.
It made me realized the major transformations the code base went through and how it was a testimony of the value of test driven approaches.
I hope it will serve as an illustration for those how still doubts of the benefits of TDD, but this serve may be of interest for those who wonder what is the decision process for open source libraries as well as those who are simply interested in knowing more about **NFluent**.

## Introduction
**As of today, NFluent latest version is V2.6**, *it has been downloaded* more than 450,000 time and is used by many projects, open source and proprietary.

The oldest version of NFluent you can download is **V0.4**, dating back to April 21, 2013, and it comes with a stark warning stating that it is obsolete and you should stay away from it. Which is incidently the first release I contributed code to. But my involvement actually started earlier.

This project is the brainchild of Thomas Pierrain and it exists thanks to his hard work and to some early contributors' efforts. It started as a set of extensions methods providing helpers for writing Asserts on enumerable/arrays; one of the founding motivations of NFluent was to explore OSS. And the rest is history, as goes the saying.



## Prior to V1: the shaping stage
### Early Stages
Here is an [early commit](https://github.com/tpierrain/NFluent/tree/30ac1863006f2ef00e2cb5d478b8a985bd8e5d6f). As said earlier, this is a set of extension methods providing helpers for asserting enumeration.
But there is explicit value in the lbrary (a method helping assert enumeration on per property basis), documentation and tests.

From a code perspective, while still quite simple, it is already documented. Implementation may appear naive, but this is to be understood as pragmatism, or KISS as well as the result of **TDD** approach.


### The core philosophy
[Two weeks after the first commit](https://github.com/tpierrain/NFluent/tree/6d15d3b27111a15404c76bcf85a69f7dc4cd1805), Thomas expressed the defining principals of the library:
> NFluent will make your tests:
> - **fluent to write**: with auto completion support
> - **fluent to read**: they read very close to plain English, making it easier for non-technical people to read test code.
> - **fluent to troubleshoot**: every failing extension method of the NFluent library throws Exception with clear message status to ease your TDD experience.
> - **less error-prone**: indeed, no more confusion about the order of the "expected" and "actual" values.

The fourth was dropped later, during a more deliberate effort to refine the proposal, mostly becase it felt redundant.
What is important is NFluent remains true to this values almost 7 years down the road.


### A hint of things to come
Things were moving fast in those early times! The day after Thomas layed down the principles, it introduced a new entry point that would ultimately lead to NFluent's signature:

```    
public static class Assert
{
  /// <summary>
  /// Asserts that a condition is true. If the condition is false the method throw a <see cref="T:FluentAssertionException"/>.
  /// </summary>
  /// <param name="condition">The evaluated condition.</param>
  
  public static void That(bool condition)
  {
     // TODO: improve the default message of failing Assert.That
     That(condition, string.Empty);
  }
  ...
}
```
In this version, you use it with a boolean condition, like this:

```
Assert.That(x == null); // fails if x is null
```



_Code available [here](https://github.com/tpierrain/NFluent/tree/e845db0a0d9784a8c9298456c0ce2240238329d0)_.

### OSS step one
I hope you had the curiosity to check the various links I provided so far. If you did, you may have noticed that something important was missing.

...

**The licence!**

**Every open source project needs a license**. And you need to choose one early on in the project. NFluent relies on the Apache 2.0 open source licence. There is a lot of material describing the differences between the various licences, picking one is a matter of controlling:

- What you may held liable of. Generally speaking, you want to reject any liability.
- How the project may be modified/forked, especially regarding commercial usage.

Bear in mind that whatever constraint you put in the license applies to you as well, unless explicitely specified.


_Code available [here](https://github.com/tpierrain/NFluent/tree/a5ab913c57fc21508b905bb63a8467148ed1ba2b)_.

### OSS Step Two
**Another key enabler for an OSS project is having a discussion space**, where _authors_, _contributors_ and _users_ can discuss issues, design choices, new features,...

This is important, as one of the most vital ressource for OSS projects is the voice of your users. You need to solve real issues for real projects.

Thomas opened some Google groups to have a discussion channels with users end of February 2013 (see [this commit](https://github.com/tpierrain/NFluent/tree/120d6cf958f6a4eea31149a9eb33c11d9a72242a)). While those has been superseeded by github's issues, you can still browse them here:
[Google Groups](https://groups.google.com/forum/#!forum/nfluent-discuss).


### Strenghtening the product vision
The project evolves rapidly. 4 weeks after the first commit, Thomas adds [UseCases.md](https://github.com/tpierrain/NFluent/blob/bfc7844f75dbe7c50c14a9c4fa5bef22c2e83b6d/UseCases.md) which lists the main features of NFLuent.