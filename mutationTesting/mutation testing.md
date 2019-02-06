# Mutation testing: first steps with Stryker-Mutator .Net

### TLDR;
I will explain why Mutation testing is an extraordinary tools that pushes that
leads to superior code quality.
I will also draft how Stryker-Mutator is implemented.

## Mutation testing: WTF ?!
_Mutation testing_ is **second order** testing, i.e. it tests your **tests**,
not your **code**. You therefore use it on top of your favorite automated
testing practices: TDD, BDD, ATDD...
Having a good mutation score means your tests are good, and you can trust
them to catch errors. On the other hands, a low score means your test base
won't catch errors! Something that should alarm you.

## Underlying Principle
In order to assess if your tests can spot bugs, mutation testing tools will
inject bugs in your code base, then run the tests. At least one test should
fail, confirming the bug has been found. Otherwise, the bug was undetected,
which is obviously not cool!

## Workflow
The tool will generate **Mutants**,i.e. new versions of your code in which the
tool has injected a single bug in each.
Then the **Mutants** are tested using your test base. If the tests succeed,
the mutant is a **survivor**, and this means your test base is imperfect.
Conversely, if at least one test fails, the mutant has been **killed**,
and everything is fine.
Actually, there is a third option: the mutant can screw the logic of the code
and create some infinite loop. To handle this situation, mutation testing
tools have a timeout features that kills long running tests. Timeouts are
considered as test failures, but reported specifically, as it is impossible
to distinguish an endless loop and a bit of code that takes a long time to run
(see halting problem).

The tool will generate several **Mutants**, tests them all and then give back
the survival rate. That is the percentage of **survivors**. It will also
provide details on each generated mutant and its final status.

**You want your survival percentage to be as low as possible.** 0 would be
great.


## Limitations
You have to bear in mind that those tools are no small undertakings and
come with limitations.
### 1. Speed
Mutation testing is **SLOW**. Remember that the tool has to:
1. Analyze your project
1. Generate Mutants
1. For each Mutant:
  1. Compile it
  1. Test it
1. Generate a resulting report

The costly part is the inner loop of course, where the tool needs to build
and test each mutant.
For example, for **[NFluent](http://n-fluent.net/)** Stryker-net generates
around 1600 mutants, and a test run takes around 10 seconds.
This give a grand total of roughly **4 (four) hours**.
Run time can be significantly improve by using test coverage detail so the
engine only run tests that may be impacted by the mutation. But it implies
a tight collaboration between the test runner, the coverage tool and the
mutation testing tool.

### 2. Mutations
The tool has to generate mutants, but this raises two conflicting goals:

- On one
hand, you want to inject a lot of mutants to really exert your tests.
- But on
the other hand, you need to have an acceptable running time, hence a
reasonable number of test runs (= mutants).

The tool must
also try and generate interesting mutants: the objective is to inject non
trivial bugs. The usual approach is to rely on simple patterns, that can
be mutated quite simply. Such as replacing comparisons (less by greater,
of less by less or equal, and vice-versa), increment by decrement,
inverting ifs condition...

### 3. Implementation
Creating a mutation tool is a daunting task: in order to generate realistic,
interesting mutants, **the tool must perform syntactic analysis of the source
code to identify patterns it can modify**.

For language having an intermediate
form, such a Java or C#/F#, the tool can mutate the bytecode/IL directly
(I think [PITest](http://pitest.org/) does this). It
has the great advantage of being simpler to implement (IL/Bytecode is a pretty
limited language, close to assembler). But with a significant drawback as it
may be difficult or even impossible to show the mutation at the source code
level.

As a user, being able to read the mutated code is important, as it helps you to
reproduce the mutants if need arises.

On the .Net front, the implementation complexity has long been a major
obstacle; The most advanced project,
[Nina-Turtle](http://www.mutation-testing.net/), uses IL modification.
## Prerequisites
There is an important prerequisite: having a decent test coverage. Keep in mind
that any uncovered significant block of code/method will be a mutants' nest
and will drag your score down.

# Discovering Stryker-Mutator.Net
We have a clear and simple test coverage strategy for NFluent: **100% of line
and branch coverage**. Quality is paramount for an assertion library, as the
quality of other projects depends on it, and I made a personal commitment to
keep the bar at 100%. It sometimes accidentally drops a bit, but top priority
is to restore it when the slip is discovered. You can check it by yourself on
[codecov.io](https://codecov.io/gh/NFluent/NFluent).

For the past 3 years, some people
(well @alexandre_victoor, mostly) said we need to look into mutation testing
to assess the quality of our tests.
But, when I tried to a couple of years ago, I discovered a bleak reality: there
was no actively supported **mutation testing tool for .Net**.

That is, until September 2018, where the first alpha versions of
[Stryker Mutator](https://stryker-mutator.io/) were released for Net Core.

## First steps
I immediately decided to try it on NFluent; so on mid October 2018, I installed
Stryker-Mutator 0.3 and made my first runs. Which meant: adding a dependency
to the NFluent test project for Net.Core 2.1 and using `dotnet stryker` command
to initiate the test.
Sadly, I kept having a bleak result: no mutants were generated, so no score
was computed.

I suspected it was related to NFluent heavy reliance on Visual Studio shared
projects. Having a glance at the Stryker's source code seemed to confirm my
intuition as I discovered that Stryker read the __csp__ file in order to
discover the source files of the project.
Next step was to fork the project on Github and debug it to confirm my
suspicion. I saw this issue as a perfect opportunity for me. Indeed, it allowed
me to fulfill several important projects I had in my backlogs for a couple of
years:
1. Contribute to an OSS (besides NFluent)
2. Secure the reliability of NFluent
3. Increase attractiveness of Net Core platform
4. Satisfy my curiosity and understand the underlying design of a mutation
testing tool.

In the following weeks I opened 5 pull requests to Stryker for features I
though were important for the project success:
1. Support for shared projects (a feature often used by OSS projects for
supporting multiple Net framework versions)
2. Improve performance (of the running tests)
3. Improve experience when used on a laptop (see footnote 1 for details)
4. Improve experience when Stryker failed to generate proper mutants
5. Improve experience by providing estimated remaining running time

I must say that Stryker's project team was helpful and receptive to my
suggestions, which is good, considering they are still in the early stage of
the project and very busy adding features.

## Getting the first results
I got the first successful run mid November, and the results did surprise me:
**224 mutants survived out of 1557 generated, roughly 15% of survival rate**.
Definitely more than I anticipated, having in mind that the project as a 100%
test coverage rate.

I assumed I had a lot of false positive, i.e. mutations that were supposed to
survive.

**I was wrong!**

Once I started reviewing those survivors, I quickly realized that almost
all survivors were relevant, but also that **they were strong indications
of actual code weaknesses**.

I have been improving the code and test since, and on my latest run, the
survival rate is down to 10.5% (174 out of 1644).

## Post mortem of failed kills
The surviving mutants can be classified in categories. Please note that I have
not
established any objective statistics regarding those category, I only share
my impression regarding the size of those various groups.

### 1. No test coverage
That is, mutants that survived simply because the code was not part of any test
whatsoever.
It should not have happened, since I have 100% test coverage.
Yes but NFluent relies on several test assemblies to reach 100% coverage, and
current Stryker versions can only be applied on a single testing assembly.
We use several assemblies for good reasons, as we have one per supported
Net framework version (2.0, 3.0, 3.5, 4.0, 4.5, 4.7, net standard 1.3 and 2.0)
as well as one per testing framework we explicitly support
(NUnit, xUnit, MSTest).
But also for **less valid reasons, such as testing edge cases for low level
utility code**.

For me, those survivors are signs of slight ugliness that should be fixed but
may not be due to external constraints, **in the context of NFluent**.
As I said earlier, I suspect this is the largest group, 25-30% of the overall
population (in NFluent case).

### 2. Insufficient assertions
That is, mutants that survived due to some lacking assertions.
That was the category I was predicting I will have a lot of.
NFluent puts
a strong emphasis on error messages and as such, tests much checks the generated
error messages. It turns out that **we do not test all error messages**, so any
mutation of the associated text strings or code may survive. Sometimes, it was
simple oversight. So fixing this meant simply adding the appropriate assertion.

Sometimes it was a bit trickier; for example, NFluent has an assertion for
the execution time of a lambda.
Here is (part of) the failing check that is part of the test code base.
```csharp
// this always fails as the execution time is never 0
Check.ThatCode(() => Thread.Sleep(0)).LastsLessThan(0, TimeUnit.Milliseconds);

```
The problem is that since the actual execution time will vary, the error
message contains a variable part (the actual execution time).

**Here is the original test in full**
```csharp
[Test]
public void FailDurationTest()
{
    Check.ThatCode(() =>
        {
            Check.ThatCode(() => Thread.Sleep(0)).LastsLessThan(0, TimeUnit.Milliseconds);
        })
        .ThrowsAny()
        .AndWhichMessage().StartsWith(Environment.NewLine +
            "The checked code took too much time to execute." + Environment.NewLine +
            "The checked execution time:");
}
```
As you can see, the assertion only checks for the beginning of the message.
But the actual message looks like this
```
The checked code's execution time was too high.
The checked code's execution time:
	[0.7692 Milliseconds]
The expected code's execution time: less than
	[0 Milliseconds]
```
So any mutant that would corrupt the second part of the message would not be
caught by the test.
So to improve the efficiency of the test, I added support for regular
expression.

```csharp
[Test]
public void FailDurationTest()
{
    Check.ThatCode(() =>
        {
            Check.ThatCode(() => Thread.Sleep(0)).LastsLessThan(0, TimeUnit.Milliseconds);
        })
        .IsAFaillingCheckWithMessage("",
            "The checked code's execution time was too high.",
            "The checked code's execution time:",
            "#\\[.+ Milliseconds\\]",
            "The expected code's execution time: less than",
            "\t[0 Milliseconds]");
}
```
_Yes, the regular expression is still a bit permissive_.
**But all related mutants are killed.**

And you know the best part of this: in the actual NFluent's code **there was a
regression
that garbled the error message**. It turned out it was introduced a year before
after a refactoring. And the insufficient assertions let it pass undetected.
**So I was able to fix an issue thanks to Stryker-Mutator!**

### 3. Limit cases
That is mutants that survived because they relate to how limits are handled in
the code and the tests. Mutation of limits handling strategy may survive if you
do not explicitly have tests for them.
The typical case is this one:
```csharp
public static ICheckLink<ICheck<TimeSpan>> IsGreaterThan(this ICheck<TimeSpan> check, Duration providedDuration)
{
    ExtensibilityHelper.BeginCheck(check)
        .CheckSutAttributes( sut => new Duration(sut, providedDuration.Unit), "")
        // important line is here
        .FailWhen(sut => sut <= providedDuration, "The {0} is not more than the limit.")
        .OnNegate("The {0} is more than the limit.")
        .ComparingTo(providedDuration, "more than", "less than or equal to")
        .EndCheck();
    return ExtensibilityHelper.BuildCheckLink(check);
}
```
As you can see, **IsGreaterThan** implements a strict comparison, hence if the
duration is equal to the provided limit, the check will fail.
Here are the tests for this check:
```csharp
[Test]
public void IsGreaterThanWorks()
{
    var testValue = TimeSpan.FromMilliseconds(500);
    Check.That(testValue).IsGreaterThan(TimeSpan.FromMilliseconds(100));

    Check.ThatCode(() =>
        {
            Check.That(TimeSpan.FromMilliseconds(50)).IsGreaterThan(100, TimeUnit.Milliseconds);
        })
        .IsAFailingCheckWithMessage("",
            "The checked duration is not more than the limit.",
            "The checked duration:",
            "\t[50 Milliseconds]",
            "The expected duration: more than",
            "\t[100 Milliseconds]");
}
```
Stryker-Mutator.Net will mutate the comparison replacing **<=** by **<**
```csharp
,FailWhen(sut => sut < providedDuration, "The {0} is not more than the limit.")
```
And the tests will keep on working. My initial reaction was to regard those as
false positive. On second thought, I realized that **not having a test
to deal with the limit case, was equivalent to consider the limit case as
undefined behavior**. Indeed, any change of behavior would introduce a slient
breaking change. Definitely not what I am ok with....

Of course, the required change is trivial, **adding the following test**:
```csharp
[Test]
public void IsGreaterThanFailsOnLimitValue()
{
    Check.ThatCode(() =>
        {
            Check.That(TimeSpan.FromMilliseconds(50)).IsGreaterThan(50, TimeUnit.Milliseconds);
        })
        .IsAFailingCheckWithMessage("",
            "The checked duration is not more than the limit.",
            "The checked duration:",
            "\t[50 Milliseconds]",
            "The expected duration: more than",
            "\t[50 Milliseconds]");
}
```

Notes:
1. I had a lot of timeout results on my laptop. I realized it was related to
my MBA going into sleep. I revised Striker's timeout strategy to handle this situation
and voilà, no more random timeouts.
