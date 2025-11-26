# Exercise: Error Diagnosis Challenge

## Exercise Instructions

Select one of the following error scenarios (sample errors provided below):

1. Use the debugging prompts from the "Tracing Error Messages and Stack Traces" section to analyze the error
2. Document the following about solving the error (you don't actually have to create fixed runnable code for this exercise):
   - Error description and what it means
   - Root cause identification
   - Suggested solution
   - Learning points that could help prevent similar errors
3. Share your analysis with peers to compare different approaches

**Important:** Check out the starter code at the GitLab Repo. For reference the specific code is also listed on the course page.

---

## Sample Error Scenarios

### 1. Off by One Error (Python)

**Error Message:**
```
Traceback (most recent call last):
  File "/home/user/projects/inventory/stock_manager.py", line 25, in <module>
    main()
  File "/home/user/projects/inventory/stock_manager.py", line 17, in main
    print_inventory_report(items)
  File "/home/user/projects/inventory/stock_manager.py", line 10, in print_inventory_report
    for i in range(len(items)):
      print(f"Item {i+1}: {items[i]['name']} - Quantity: {items[i]['quantity']}")
IndexError: list index out of range
```

**Code Context:**
```python
def print_inventory_report(items):
    print("===== INVENTORY REPORT =====")
    # Error occurs in this loop - classic off-by-one error
    for i in range(len(items) + 1):  # Notice the + 1 here
        print(f"Item {i+1}: {items[i]['name']} - Quantity: {items[i]['quantity']}")
    print("============================")
```

### 2. Stack Overflow (Java)

**Error Message:**
```
Exception in thread "main" java.lang.StackOverflowError
	at com.example.recursion.FactorialCalculator.calculateFactorial(FactorialCalculator.java:15)
	at com.example.recursion.FactorialCalculator.calculateFactorial(FactorialCalculator.java:15)
	... [1000+ more lines of the same call]
```

**Code Context:**
```java
public static int calculateFactorial(int num) {
    // Missing base case or incorrect recursive call
    // This will cause infinite recursion
    return num * calculateFactorial(num - 1);
}
```

### 3. Null Pointer Reference (Java)

**Error Message:**
```
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "com.example.store.Product.getPrice()" because "product" is null
	at com.example.store.ShoppingCart.calculateTotal(ShoppingCart.java:22)
```

---

## Prompt 1: Error Message Translation

### Prompt Template

```
I need help understanding this error message from my [language/framework] application.

Here's the complete error message and stack trace:

[paste full error message and stack trace or log file]

My application context:
- This happened when I was trying to [action that triggered the error]
- The application is a [brief description of application type]
- I'm using [language version] and [framework version]

Could you:
1. Explain what this error means in simple, non-technical terms
2. Identify the most relevant lines in the stack trace (which ones actually point to my code)
3. List 2-3 of the most likely causes based on this type of error
4. Suggest what specific information I should look for in my code
5. Provide a step-by-step debugging approach I could follow

I'm particularly unfamiliar with [specific technology or concept] mentioned in the error, so extra explanation there would help.
```

---

## Prompt 2: Root Cause Analysis

### Prompt Template

```
I need help diagnosing the root cause of an error in my [language/framework] application.

Here's the error and stack trace:
[paste complete error and stack trace or log file]

Here are the relevant code snippets from files mentioned in the stack trace:

File: [filename 1]
[code snippet from file 1]

File: [filename 2]
[code snippet from file 2]

Additional context:
- This error occurs when [specific scenario or user action]
- It happens [consistently/intermittently]
- I've already tried [mention any debugging steps already taken]
- These files interact by [brief explanation of how these components connect]

Could you please:
1. Identify the likely root cause (not just the symptoms)
2. Explain the chain of events leading to this error
3. Suggest 2-3 specific code changes that might fix the issue
4. Recommend tests I could write to verify the fix
5. Explain any patterns or anti-patterns you notice that might cause similar problems elsewhere
6. Suggest debugging tools or techniques specific to this type of error

I'm most confused about [specific aspect of the error or code], so detailed explanation there would be helpful.
```

---

## Reflection Questions

1. **How did the AI's explanation compare to documentation you found online?**
2. **What aspects of the error would have been difficult to diagnose manually?**
3. **How would you modify your code to provide better error messages in the future?**
4. **Did the AI help you understand not just the fix, but the underlying concepts?**

---

## Example Exercise Solution Format

When analyzing your error, consider structuring your response like this:

### Error Analysis: [Error Type]

**Error Description:**
[Explain what the error message means in plain language]

**Root Cause:**
[Identify the actual problem causing the error]

**Solution:**
[Provide specific code changes to fix the issue]

**Learning Points:**
[Share insights to prevent similar errors in the future]

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

