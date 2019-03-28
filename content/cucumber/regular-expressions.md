---
title: Regular Expressions
subtitle: Linking step definitions to Gherkin steps
polyglot:
 - java
 - javascript
 - ruby

weight: 4
markup: mmark
---

Cucumber uses *expressions* to link a [Gherkin Step](/gherkin/reference#steps)
to a [Step Definition](/cucumber/step-definitions). You can use Regular Expressions
or [Cucumber Expressions](/cucumber/regular-expressions).

[Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression) offer similar functionality to 
Cucumber Expressions, with a syntax that is more powerful but harder to read and write. 

{{% block "javascript,ruby" %}}
Regular Expressions are represented by a regular expression delimited by `/` while Cucumber Expression are strings 
delimited by `"`. So `/I have 42 cucumbers in my belly/` is a Regular Expression while 
`"I have 42 cucumbers in my belly"` is a Cucumber expression.
{{% /block %}}

{{% block "java" %}}
Java does not have a a dedicated syntax for regular expressions. To distinguish between Regular and Cucumber 
Expressions [a heuristic](https://github.com/cucumber/cucumber/blob/master/cucumber-expressions/java/heuristics.adoc) 
is used. In short `^I have 42 cucumbers in my belly$` is a Regular Expression while `I have 42 cucumbers in my belly` 
is a Cucumber expression.
{{% /block %}}


# Introduction

Let's write a Regular Expression that matches the following Gherkin step (the `Given`
keyword has been removed here, as it's not part of the match).

    I have 42 cucumbers in my belly

The simplest Regular Expression that matches that text would be the text itself,
but we can also write a more generic expression, with an `int` *output parameter*:

    ^I have (\d+) cucumbers in my belly$

When the text is matched against that expression, the number `42` is extracted
from the `(\d+)` capture group and passed as an argument to the [step definition](/cucumber/step-definitions).


# Parameter types

If you prefer to use Regular Expressions, each 
*capture group* from the match will be passed as arguments to the step definition's {{% stepdef-body %}}.

{{% block "java" %}}
```java
@Given("I have (\\d+) cukes in my belly")
public void i_have_n_cukes_in_my_belly(int cukes) {
}
```
{{% /block %}}

{{% block "ruby" %}}
```ruby
Given(/I have (\d+) cukes in my belly/) do |cukes|
end
```
{{% /block %}}

{{% block "javascript" %}}
```javascript
Given(/I have (\d+) cukes in my belly/, function (cukes) {
});
```
{{% /block %}}

If the capture group expression is identical to one of the registered 
[parameter types](/cucumber/cucumber-expressions#parameter-types)'s `regexp`,
the captured string will be transformed before it is passed to the 
step definition's {{% stepdef-body %}}. In the example above, the `cukes`
argument will be an integer, because the built-in `int` parameter type's
`regexp` is `\d+` .


By default the results captured by a capture group are passed as `String` to the step definition. When the 
expression in the capture group match a predefined or custom parameter type defined in 
[Cucumber Expressions - Parameter types](/cucumber/regular-expressions#parameter-types) it will be transformed.