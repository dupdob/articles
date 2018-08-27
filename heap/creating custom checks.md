# Writing your own checks

## Disclaimer
This documentation describes part of NFluent that are still in beta. Naming and API details may change before official release.
Documentation will of course be updated to reflect the final version.

## Introduction
With NFluent, you can extend exiting assertions and write your own checks, either to add some to already supported classes or to add support for your own classes.
Your checks will integrate with the existing syntax, including support for **And** and **Not** keywords, if you respect a set of simple rules.

As an example, let's say we want a check verifying that a string contains a given number. Let's call it **IsThisNumber**.
Here is an example of what we want to achieve.
```csharp
// this one is ok
Check.That("12").IsThisNumber(12);
// this one will fail
Check.That("12").IsThisNumber(14);
```

**Prerequisite: you must be familiar with extension methods**

## General Signature
### Parameters
The signature of our check must be:
```csharp
// static class to hosts extension methods
public static class MyChekcs
{
  ...
  public static ICheckLink<ICheck<string>> IsThisNumber(this ICheck<string> context, int expected)
  {
    // implementation left out for clarity
    ...
    return ExtensibilityHelper.BuildChainLink(context);
  }
  ...
}
```
The first parameter _context_ is an instance of **ICheck< string >**. It stores context information for checks, such as the _system under test_ (a.ka.sut). It will be used by helper functions in the implementation.
The second parameter, _expected_, is the value we want to be represented by the string.

### Return value
The role of the return value is to support chaining of checks using _And_.
There is an function that will help you build the proper instance: _ExtensibilityHelper.BuildChainLink(...)_.
It needs a single argument, which is the ICheck<T> instance received as the first argument.


```csharp
{
  ...
 return ExtensibilityHelper.BuildChainLink(context);
}
```
Not much to discuss there, this is just boilerplate code.

## The check logic
Now let's look at the full code.
```csharp
public static ICheckLink<ICheck<string>> IsThisNumber(this ICheck<string> context, int expected)
{

  ExtensibilityHelper.BeginCheck(context).
    FailsIfNull().
    FailsWhen( sut => sut != expected.ToString(),
      "The {checked} does not contain the {expected}.").
    Expecting(expected).
    EndCheck();

  return ExtensibilityHelper.BuildChainLink(context);
}
```
As you can see, the check logic is expressed as a single fluent expression, in the form of method.method.method....

Let me explain the role of each method used here.

### BeginCheck
```csharp
ExtensibilityHelper.BeginCheck(context).
```
As the name implies, _BeginCheck_ is used to begin to describe the check logic. Under the hood, it extracts required informations from _context_ and create and _ICheckLogic< string >_ that provides the main building blocks.

_Note: for those who are familiar with the original extensibility helper methods, **BeginCheck** replaces both **ExtractChecker** and **ExecuteCheck**._
### FailsIfNull()
```csharp
FailsIfNull().
```
Signals that the check must fail if the _checked string_ is null. A default error message is provided, but you can provide one if you think this is useful.
### FailsWhen(_lambda_, error)
```csharp
 FailsWhen( sut => sut != expected.ToString(),
            "The {checked} does not contain the {expected}.").
```

Signals that the check must fail when the provided _lambda_ returns true. The failure message will be generated using _error_ as a string template, where **{checked}** will be replaced by an expression describing the _sut_ and **{expected}** by an expression describing the expected value.

### Expecting(_expected_)
```csharp
Expecting(expected).
```
Specify what was the expected value for the _sut_. This is used for error message generation.

### EndCheck()
```csharp
EndCheck();
```
Request the evaluation of failure conditions and report errors when relevant.
Error are reported through raised exceptions. Only the first failure is reported.

## Handling negation
Actually, the code is still missing some bits.
You need to remember that NFluent offers a _Not_ keyword that can be used to invert the checking logic.
So let's see what happens when we try to use it.

So let's see how it works with a negated failing test:
```csharp
 Check.That("12").Not.IsThisNumber(14);
```

No exception raised, the check is successful, as we expected.

What happens if we negate a successful check?

```csharp
 Check.That("14").Not.IsThisNumber(14);
```
Oups, we get an _InvalidOperationException_
```csharp
{System.InvalidOperationException}: 'Error message was not specified.'
```
Of course, we need to provide an exception message for the negated case. Let me fix the code for you:
```csharp
public static ICheckLink<ICheck<string>> IsThisNumber(this ICheck<string> context, int expected)
{
  ExtensibilityHelper.BeginCheck(context)
   .FailsIfNull()
   .FailsWhen( sut => sut != expected.ToString(),
     "The {checked} does not contain the {expected}.")
   .Expecting(expected)
   .Negates("The {checked} contains the {expected} whereas it should not.")
   .EndCheck();

  return ExtensibilityHelper.BuildChainLink(context);
}
```
### Negates(_message_)
```csharp
.Negates("The {checked} contains the {expected} whereas it should not.")
```
Specify the message template when the check fails in negated mode , i.e. after the _Not_ keyword.

## And we're done
This page covers the methods used to build most of the checks you may need. **NFluent** supports more complex failure condition and elaborate checks. You will find them describe in the [Advanced checks](advanced_checks) page.
