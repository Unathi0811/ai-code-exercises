# Exercise: Using AI to help with testing

## Exercise Overview

This exercise will guide you through the process of using AI to help improve your testing skills using the task prioritization algorithm provided in the course materials. You'll apply the strategies discussed in the chapter to develop better testing practices.

## Instructions

Choose one implementation of the task prioritization algorithm (Python, JavaScript, or Java Task Manager) to work with throughout this exercise.

**Important:** Check out the starter code at the GitLab Repo.

---

## Part 1: Understanding What to Test

### Exercise 1.1: Behavior Analysis

Examine the `calculateTaskScore` function from your chosen language implementation.

Use the following prompt with an AI assistant:

```
I'm learning how to test this function, and I want to understand what behaviors I should test:

[Paste the calculateTaskScore function]

Rather than generating tests for me, please:
1. Ask me questions about what I think this function does
2. After I answer, help identify any behaviors I missed
3. Ask me what edge cases I think should be tested
4. Help me identify additional edge cases I didn't think of
5. Ask me which test I should write first and why
```

**Task:** Based on the conversation, create a list of at least 5 test cases you should write, including any edge cases identified.

### Exercise 1.2: Test Planning

Now focus on the entire set of functions: `calculateTaskScore`, `sortTasksByImportance`, and `getTopPriorityTasks`.

Use this prompt:

```
I'm learning to write tests for these related functions:

[Paste all three functions from your chosen implementation]

Instead of writing tests for me, please:
1. Help me create a testing plan by asking me questions
2. For each behavior I identify, ask me how I would test it
3. If I miss something important, give me hints rather than answers
4. For each edge case we identify, ask me what I expect to happen
5. Help me create a checklist of tests I should write, organized by priority
```

**Task:** Create a structured test plan document including:
- Priority of test cases
- Types of tests needed (unit, integration)
- Test dependencies
- Expected outcomes for each test

---

## Part 2: Improving a Single Test

### Exercise 2.1: Writing Your First Test

Write a basic test for the `calculateTaskScore` function. Intentionally make it simple (just test the basic functionality).

Use this prompt to improve your test:

```
I wrote this test for the following function:

Function:
[Paste calculateTaskScore function]

My test:
[Paste your simple test]

Instead of rewriting it for me, please:
1. Ask me questions about what my test is trying to verify
2. Help me identify if my test is checking behavior or implementation details
3. Suggest how I could make the test's purpose clearer
4. Ask me what edge cases my test might be missing
5. Guide me in improving my assertions to be more precise
```

**Task:** Based on the feedback, rewrite your test to make it more robust and clearer.

### Exercise 2.2: Learning From Examples

Choose another aspect of the `calculateTaskScore` function to test (e.g., due date calculations).

Use this prompt:

```
I'm trying to understand how to better test the due date calculation portion of this function:

[Paste calculateTaskScore function]

I'm thinking of writing a test like this (pseudocode):
[Write a rough outline/pseudocode of your test idea]

Please:
1. Explain the principles of a good test for this specific functionality
2. Show me ONE example of a better test with comments explaining why it's better
3. Ask me questions about how I would improve my approach based on this example
4. Challenge me to identify what edge cases I should add
5. Guide me in writing more precise assertions
```

**Task:** Write a comprehensive test for the due date calculation functionality based on your conversation.

---

## Part 3: Test-Driven Development Practice

### Exercise 3.1: TDD for a New Feature

Imagine you need to add a new feature: tasks assigned to the current user should get a score boost of +12.

Use this prompt before writing any code:

```
I want to practice Test-Driven Development to add a new feature:

Tasks assigned to the current user should get a score boost of +12.

Here's the current function:
[Paste calculateTaskScore function]

Instead of writing code and tests for me, please:
1. Ask me what I think the first test should be and why
2. Give me feedback on my proposed test
3. After I write the test, ask me what minimal code would make it pass
4. Once I've implemented the code, guide me on what test to add next
5. Help me understand when it's time to refactor vs. add new functionality
```

**Task:** Follow the TDD process:
1. Write a failing test for the new feature
2. Implement the minimal code to make it pass
3. Refactor if necessary
4. Write the next test if needed

### Exercise 3.2: TDD for Bug Fix

Imagine there's a bug: the calculation for "days since update" is incorrect.

Use this prompt:

```
I want to practice TDD to fix a bug:

The calculation for "days since update" isn't working correctly.

Here's the current function:
[Paste calculateTaskScore function]

Please:
1. Ask me what test I would write to reproduce the bug
2. Help me understand if my test actually demonstrates the bug
3. Guide me in implementing a minimal fix
4. Ask me if there are any other tests I should add to prevent regression
```

**Task:** Write a test that demonstrates the bug, then fix the code to make the test pass.

---

## Part 4: Integration Testing

### Exercise 4.1: Testing the Full Workflow

Think about how to test the three functions working together: `calculateTaskScore`, `sortTasksByImportance`, and `getTopPriorityTasks`.

Use this prompt:

```
I want to create an integration test for the task priority workflow:

[Paste all three functions]

Rather than writing the test for me, please:
1. Ask me what scenarios an integration test should verify
2. Guide me in designing test data that would exercise the entire workflow
3. Help me understand what assertions would verify the correct behavior
4. Ask me how I would structure the test to make it readable and maintainable
```

**Task:** Write an integration test that verifies the three functions work correctly together.

---

## Discussion

After completing all parts of this exercise, prepare a short document that includes:

1. Your test plan document from Part 1
2. Your improved unit tests from Part 2
3. Your TDD implementation and tests from Part 3
4. Your integration test from Part 4
5. A reflection on what you learned about testing through this exercise

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

