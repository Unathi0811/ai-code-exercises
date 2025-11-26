# Exercise: AI Solution Verification Challenge

## Exercise Instructions

Select one of the following scenarios:

1. A sorting function with a subtle bug
2. [Other scenario from course materials]

Ask an AI tool to solve the problem, then apply all three verification strategies:

1. Collaborative Solution Verification
2. Learning Through Alternative Approaches
3. Developing a Critical Eye

Document the verification process and what you learned, then implement the final, verified solution.

**Important:** Check out the starter code at the GitLab Repo. For reference the specific code is also listed on the course page.

---

## Sample Problem: Buggy Sorting Function

### Buggy Code

```javascript
// Buggy sorting function
function mergeSort(arr) {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
}

function merge(left, right) {
  let result = [];
  let i = 0;
  let j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] < right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }

  // Bug: Only one of these loops will execute
  while (i < left.length) {
    result.push(left[i]);
    j++; // Bug: incrementing j instead of i
  }

  while (j < right.length) {
    result.push(right[j]);
    j++;
  }

  return result;
}
```

---

## Verification Strategy 1: Collaborative Solution Verification

### Prompt Template

```
I'd like to verify a solution you've suggested before implementing it. Let me share both the solution and my current understanding of how to test it.

The solution you provided:
[Paste AI's suggested code or explanation]

My understanding of this solution:
- What I think it does: [Your explanation of how the solution works]
- How I think it fixes the original problem: [Your understanding of why it addresses the issue]

My ideas for testing this solution:
- Test cases I'm considering: [List 2-3 test cases you've thought of]
- Edge cases I'm concerned about: [Any edge cases you can think of]
- Parts I'm unsure how to test: [Mention any aspects you're not sure how to verify]

Could you please:
1. Confirm if my understanding is accurate or correct any misconceptions
2. Evaluate my proposed test cases and suggest improvements
3. Recommend additional edge cases I might have missed
4. Provide a simple verification plan that would help ensure this solution works properly
5. Highlight any assumptions in the solution that might not be obvious to me

My environment/constraints:
[Describe relevant details about your system, versions, etc.]

I want to learn how to properly verify solutions, not just implement them blindly.
```

---

## Verification Strategy 2: Learning Through Alternative Approaches

### Prompt Template

```
You suggested this approach to solve my problem:

[Paste AI's suggested solution]

I'd like to explore alternative approaches to deepen my understanding. Here are my thoughts:

My understanding of the current solution:
- How it works: [Your explanation of the suggested approach]
- Pros: [What you think are advantages of this solution]
- Cons: [Any disadvantages or concerns you see]

Alternative ideas I'm considering:
- Idea 1: [Your own alternative approach if you have one]
- Idea 2: [Another approach if you have one, or just write "I don't have other ideas yet"]

Could you please:
1. Let me know if my understanding of the original solution is accurate
2. Provide 2-3 different approaches to solve the same problem
3. Compare all approaches (including mine if applicable) in terms of:
   - Performance considerations
   - Code readability and maintainability
   - Scalability for future extensions
   - Typical situations where each approach shines
4. Explain the principles or patterns behind each approach so I can learn when to apply them in the future

I'm not just looking for the "best" solution, but to understand the trade-offs and reasoning behind different approaches.
```

---

## Verification Strategy 3: Developing a Critical Eye

### Prompt Template

```
I want to develop my critical thinking skills by analyzing a solution together. Let's examine this solution you provided:

[Paste AI's suggested solution]

My initial assessment:
- Strengths I see: [What you think is good about the solution]
- Concerns I have: [Any aspects you're unsure about]
- Questions that came to mind: [1-2 specific questions you have]

Could we collaborate on a critical review where:
1. You confirm or correct my initial assessment
2. We identify potential weaknesses in this solution that I might have missed
3. We consider what assumptions this code makes about:
   - The environment it runs in
   - The input data
   - Error handling expectations
   - Performance requirements
4. We discuss how maintainable this solution would be if:
   - Requirements change slightly in the future
   - The codebase scales significantly
   - Another developer needs to modify it without context
5. You suggest improvements that would address the biggest concerns

I'm trying to develop the skill of critically evaluating solutions before implementing them. Explaining your thought process would help me learn this skill better.
```

---

## Reflection Questions

1. **How did your confidence in the solution change after verification?**
2. **What aspects of the AI solution required the most scrutiny?**
3. **Which verification technique was most valuable for your specific problem?**
4. **What would you do differently in future verification processes?**

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

