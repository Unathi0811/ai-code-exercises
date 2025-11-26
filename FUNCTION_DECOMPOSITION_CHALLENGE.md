# Exercise: Function Decomposition Challenge

## Exercise Instructions

Select a complex function from your codebase or one of the provided sample functions:

1. Data Validation Function with Nested Conditionals (JavaScript)
2. Report Generation Function with Multiple Data Transformations (Python)
3. Data Processing Pipeline with Error Handling (Java)

Use the provided AI prompts to:

1. Identify the distinct responsibilities in the function
2. Create a decomposition plan
3. Extract helper functions with clear names and purposes
4. Refactor the main function to call the helper functions
5. Run tests to verify the refactoring preserves behavior
6. Document your refactoring approach and the benefits gained

**Important:** Check out the starter code at the GitLab Repo. For reference the specific code is also listed on the course page.

---

## Sample Complex Functions

### 1. Data Validation Function with Nested Conditionals (JavaScript)

[See course materials for full code example]

### 2. Report Generation Function with Multiple Data Transformations (Python)

[See course materials for full code example]

### 3. Data Processing Pipeline with Error Handling (Java)

[See course materials for full code example]

---

## Prompt 1: Function Responsibility Analysis

### Prompt Template

```
I have a complex function that I'd like to refactor by breaking it down into smaller functions:

[Paste complex function here]

Please:
1. Identify the distinct responsibilities/concerns in this function
2. Suggest a decomposition strategy with smaller functions
3. Show which parts of the original code would move to each new function
4. Provide a refactored version with the main function delegating to the smaller functions
```

---

## Prompt 2: Single-Responsibility Extraction

### Prompt Template

```
I'd like to extract a single responsibility from this complex function:

[Paste complex function here]

Specifically, I want to extract the logic that handles:
[Describe the specific responsibility to extract, e.g., "validation", "data transformation", "error handling"]

Please:
1. Identify all code related to this responsibility
2. Create a new function that encapsulates just this responsibility
3. Show how to modify the original function to call the new function
4. Suggest an appropriate name and parameters for the extracted function
```

---

## Prompt 3: Conditional Logic Simplification

### Prompt Template

```
This function contains complex conditional logic that I want to simplify:

[Paste function with complex conditionals]

Please:
1. Identify the different logical paths through the conditionals
2. Extract each conditional branch into a separate named function
3. Suggest a pattern (Strategy, Command, etc.) that could replace the conditionals
4. Provide a refactored version with simplified logic in the main function
```

---

## Reflection Questions

1. **How did breaking down the function improve its readability and maintainability?**
2. **What was the most challenging part of decomposing the function?**
3. **Which extracted function would be most reusable in other contexts?**
4. **What patterns did you discover that could be applied to other functions?**

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

