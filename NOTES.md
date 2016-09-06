# Cucumber
Cucumber executes your .feature files, and those files contain executable specifications written in a language called Gherkin.

## Gherkin
It is a *Domain Specific Language* that lets you describe software’s behaviour without detailing how that behaviour is implemented.

- Gherkin is a line-oriented language that uses indentation to define structure. 
- Line endings terminate statements (eg, steps). 
- Either spaces or tabs may be used for indentation (but spaces are more portable). 
- Comment lines are allowed anywhere in the file. They begin with zero or more spaces, followed by a hash sign (#) and some amount of text.
- The parser divides the input into features, scenarios and steps. 

In Gherkin, each line that isn't blank has to start with a Gherkin keyword, followed by any text you like. The main keywords are:
- Feature
- Scenario
- Given, When, Then, And, But (Steps)
- Background
- Scenario Outline
- Examples

There are a few extra keywords as well:

- """ (Doc Strings)
- | (Data Tables)
- @ (Tags)
- \# (Comments)

### Feature
- A .feature file is supposed to describe a single feature of the system, or a particular aspect of a feature. 
- It's just a way to provide a high-level description of a software feature, and to group related scenarios.
- A feature has three basic elements---the Feature: keyword, a name (on the same line) and an optional (but highly recommended) description that can span multiple lines.
- Some parts of Gherkin documents do not have to start with a keyword. After the keywords Feature, Scenario, Scenario Outline or Examples you can write anything you like, as long as no line starts with a key a keyword.

### Scenario
- A scenario is a concrete example that illustrates a business rule. 
- It consists of a list of steps.
- You can have as many steps as you like, but we recommend you keep the number at 3-5 per scenario. 

Scenarios follow the same pattern (This is done with steps):
- Describe an initial context
- Describe an event
- Describe an expected outcome

### Steps
- A step typically starts with Given, When or Then. 
- If there are multiple Given or When steps underneath each other, you can use And or But. Cucumber does not differentiate between the keywords, but choosing the right one is important for the readability of the scenario as a whole.

#### Given
- Given steps are used to describe the initial context of the system---the scene of the scenario. 
- It is typically something that happened in the past.
- It's ok to have several Given steps (just use And or But for number 2 and upwards to make it more readable).

#### When
- When steps are used to describe an event, or an action. 
- This can be a person interacting with the system, or it can be an event triggered by another system.
- **It's strongly recommended you only have a single When step per scenario.**
- If you feel compelled to add more it's usually a sign that you should split the scenario up in multiple scenarios.

#### Then
- Then steps are used to describe an expected outcome, or result.
- The step definition of a Then step **should use an assertion** to compare the actual outcome (what the system actually does) to the expected outcome (what the step says the system is supposed to do).


### Background
Occasionally you'll find yourself repeating the same Given steps in all of the scenarios in a feature file.
You can literally move such Given steps to the background by grouping them under a Background section before the first scenario:
```gherkin
Background:
  Given a $100 microwave was sold on 2015-11-03
  And today is 2015-11-18
```

### Scenario Outline
When you have a complex business rule with variable inputs or outputs we can simplify it with a Scenario Outline.

```gherkin
Scenario Outline: feeding a suckler cow
  Given the cow weighs <weight> kg
  When we calculate the feeding requirements
  Then the energy should be <energy> MJ
  And the protein should be <protein> kg

  Examples:
    | weight | energy | protein |
    |    450 |  26500 |     215 |
    |    500 |  29500 |     245 |
    |    575 |  31500 |     255 |
    |    600 |  37000 |     305 |
```

#### Examples

- A Scenario Outline section is always followed by one or more Examples sections, which are a container for a table.
- The table must have a header row corresponding to the variables in the Scenario Outline steps.
- Each of the rows below will create a new Scenario, filling in the variable values.

### Step Arguments
In some cases you might want to pass a larger chunk of text or a table of data to a step.

- For this purpose Gherkin has Doc Strings and Data Tables.
    - Doc Strings are handy for passing a larger piece of text to a step definition.
    - Data Tables are handy for passing a list of values to a step definition:

\* Just like Doc Strings, Data Tables will be passed to the Step Definition as the last argument.


### Tags
Tags are a way to group Scenarios. They are @-prefixed strings.\*
- You can tell Cucumber to only run scenarios with certain tags, or to exclude scenarios with certain tags.
- You can place as many tags as you like above Feature, Scenario, Scenario Outline or Examples keywords. 
- Tags are inherited from parent elements.

\* Space character are invalid in tags and may separate them.


## Step Definitions
Cucumber doesn't know how to execute your scenarios out-of-the-box. It needs Step Definitions to translate plain text Gherkin steps into actions that will interact with the system.

When Cucumber executes a Step in a Scenario it will look for a matching Step Definition to execute.




Cucumber doesn't know how to execute your scenarios out-of-the-box. It needs Step Definitions to translate plain text Gherkin steps into actions that will interact with the system.

When Cucumber executes a Step in a Scenario it will look for a matching Step Definition to execute.

A Step Definition is a small piece of code with a pattern attached to it. The pattern is used to link the step definition to all the matching Steps, and the code is what Cucumber will execute when it sees a Gherkin Step.

To understand how Step Definitions work, consider the following Scenario:
```gherkin
Scenario: Some cukes
  Given I have 48 cukes in my belly
```

The I have 48 cukes in my belly part of the step (the text following the Given keyword) will match the Step Definition below:

```JavaScript
Given(/^I have (\d+) cukes in my belly$/, function (cukes) {
  console.log("Cukes: " + cukes);
});
```

1. When Cucumber matches a Step against a pattern in a Step Definition, it passes the value of all the capture groups to the Step Definition's arguments.
2. Capture groups are strings (even when they match digits like \d+). For statically typed languages, Cucumber will automatically transform those strings into the appropriate type. For dynamically typed languages, no transformation happens by default, as there is no type information.
3. Cucumber does not differentiate between the five step keywords Given, When, Then, And and But.