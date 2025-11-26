# Exercise: Design Pattern Implementation Challenge

## Exercise Instructions

Select a piece of code (see Sample Pattern Opportunities) that exhibits one of these pattern opportunities:

1. Conditional logic that could use the Strategy pattern
2. Object creation complexity that could use the Factory pattern
3. Complex object relationships that could use the Observer pattern
4. Incompatible interfaces that could use the Adapter pattern

Use the provided AI prompts to:

1. Analyze the code for pattern opportunities
2. Get implementation guidance for the chosen pattern
3. Refactor the code to implement the pattern
4. Write tests to verify the refactored code preserves the original behavior
5. Document the benefits gained from the pattern implementation

**Important:** Check out the starter code at the GitLab Repo. For reference the specific code is also listed on the course page.

---

## Sample Pattern Opportunities

### Strategy Pattern Opportunity: Complex pricing rules with many conditionals

[See course materials for full code example]

### Factory Pattern Opportunity: Complex object creation with many dependencies

[See course materials for full code example]

### Observer Pattern Opportunity: A system where changes to one object require updates to many others

[See course materials for full code example]

### Adapter Pattern Opportunity: Code that needs to work with an external API with an incompatible interface

[See course materials for full code example]

---

## Prompt 1: Pattern Opportunity Identification

### Prompt Template

```
I'd like to identify potential design patterns that could improve this code:

[Paste code here]

Please:
1. Identify code structures or problems that could benefit from design patterns
2. Suggest which specific patterns would be appropriate and why
3. Explain the benefits each pattern would bring to this code
4. Highlight any potential drawbacks or implementation challenges
5. Prioritize suggestions based on impact vs. implementation effort
```

---

## Prompt 2: Pattern Implementation Guidance

### Prompt Template

```
I'd like to refactor this code to implement the [PATTERN_NAME] design pattern:

[Paste code to refactor]

I think this pattern is applicable because [YOUR REASON FOR CHOOSING THIS PATTERN FOR THIS CODE].

I am thinking of implementing it by making this changes:
[SUMMARY OF HOW YOU ARE THINKING ABOUT IMPLEMENTING THE PATTERN]

Please:
1. Verify if my chosen design pattern and implementation ideas makes sense, and point out errors in my thinking.
2. If the pattern is applicable, provide a step-by-step refactoring plan to help guide me.
3. Show the refactored implementation with the pattern applied
4. Highlight key changes and how they align with the pattern principles
5. Explain how to verify the refactoring preserves the original behavior
6. If the pattern is not applicable, please refer me to alternative patterns I can look at that are more applicable to this code.
```

---

## Reflection Questions

1. **How did implementing the pattern improve the code's maintainability?**
2. **What future changes will be easier because of this pattern?**
3. **Were there any unexpected challenges in implementing the pattern?**
4. **How would you explain this pattern to another developer?**

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

