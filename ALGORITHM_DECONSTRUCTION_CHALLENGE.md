# Algorithm Deconstruction Challenge

## Exercise Overview

This exercise focuses on using AI prompts to break down and understand complex algorithms. You'll work with the Task Priority Sorting Algorithm from the TaskManager application and use AI to gain deep understanding of how it works.

## Selected Algorithm: Task Priority Sorting Algorithm

The algorithm we'll analyze is the `TaskPriorityManager` class, specifically the `calculateTaskScore` method which implements a weighted scoring system for task prioritization.

## Algorithm Analysis Using AI Prompts

### Prompt 1: Understanding How a Specific Algorithm Works

**Prompt Used:**
```
I need to understand how this task priority scoring algorithm works:

[Paste calculateTaskScore method]

Please explain:
1. What is the overall purpose of this algorithm?
2. How does the scoring system work step by step?
3. What are the different factors that contribute to the score?
4. How are the weights determined for each factor?
5. What edge cases does this algorithm handle?
```

**AI Explanation Summary:**

The algorithm calculates a composite importance score for tasks using multiple weighted factors:

1. **Base Priority Score**: Each priority level (LOW=1, MEDIUM=2, HIGH=3, URGENT=4) is multiplied by 10 to create a base score
2. **Due Date Proximity**: Adds points based on how soon the task is due:
   - Overdue: +30 points
   - Due today: +20 points
   - Due in 2 days: +15 points
   - Due in a week: +10 points
3. **Status Penalties**: Reduces score for completed or in-review tasks:
   - DONE: -50 points
   - REVIEW: -15 points
4. **Tag Boosts**: Adds +8 points for tasks with "blocker", "critical", or "urgent" tags
5. **Recency Boost**: Adds +5 points for tasks updated within the last day

**Key Insights:**
- The algorithm uses additive scoring (factors are added/subtracted)
- Negative scores are possible (completed tasks can have negative scores)
- The scoring is designed to surface urgent, incomplete tasks that are due soon

### Prompt 2: Deepening Understanding Through Guided Questions

**Prompt Used:**
```
I understand this algorithm calculates a score, but I want to deepen my understanding. Here's what I think I know:

- The algorithm combines multiple factors into a single score
- Higher scores mean higher priority
- The scoring seems to favor tasks that are urgent and due soon

Please ask me questions to help me understand:
1. Why certain weights were chosen (like why overdue gets +30 vs +20 for today)
2. How the algorithm handles edge cases (null due dates, empty tag lists)
3. What happens when a task has conflicting signals (high priority but already done)
4. How this scoring system compares to other prioritization approaches
```

**Key Questions and Answers:**

**Q: Why does the algorithm subtract 50 points for completed tasks?**
A: This ensures completed tasks are deprioritized significantly, but the subtraction is large enough that even high-priority completed tasks won't appear at the top of sorted lists.

**Q: What happens if a task has no due date?**
A: The due date factor is skipped entirely (the if statement checks for null), so the task only gets points from priority, status, tags, and recency.

**Q: Can a task have a negative score?**
A: Yes! A LOW priority (10 points) task that's DONE (-50 points) would have a score of -40, ensuring it appears at the bottom of sorted lists.

**Q: Why multiply base priority by 10?**
A: This creates a scale where priority differences are significant (10, 20, 30, 40) compared to other factors, ensuring priority level remains the dominant factor.

### Prompt 3: Mapping Data Flow and State Management

**Prompt Used:**
```
I want to understand the complete data flow when this algorithm is used:

[Paste calculateTaskScore, sortTasksByImportance, and getTopPriorityTasks methods]

Please map out:
1. How data flows from input (Task object) through the scoring calculation
2. How the score is used in the sorting process
3. What state changes occur during sorting
4. How the final sorted list is produced
```

**Data Flow Diagram:**

```
Input: List<Task>
    │
    ▼
┌─────────────────────────────────────┐
│ For each Task in the list:         │
│   └─> calculateTaskScore(task)      │
│       ├─> Extract priority         │
│       ├─> Calculate base score      │
│       ├─> Add due date factor       │
│       ├─> Apply status penalty      │
│       ├─> Check for tag boost       │
│       └─> Add recency boost         │
│       └─> Return: int score         │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ sortTasksByImportance(tasks)        │
│   └─> Stream operations:            │
│       ├─> Map each task to score    │
│       ├─> Sort by score (descending)│
│       └─> Collect to List           │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│ getTopPriorityTasks(tasks, limit)   │
│   └─> Take first N tasks            │
└─────────────────────────────────────┘
    │
    ▼
Output: List<Task> (sorted by priority)
```

**State Changes:**
- **No mutation**: The original task list is not modified
- **New list creation**: A new sorted list is created
- **Score calculation**: Scores are calculated on-demand (not stored)
- **Immutable operations**: All operations use functional programming patterns

## Visual Diagram of Algorithm Logic

```
┌─────────────────────────────────────────────────────────────┐
│                    calculateTaskScore(task)                  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │  Base Score = Priority × 10      │
        │  LOW=10, MEDIUM=20, HIGH=30,      │
        │  URGENT=40                         │
        └───────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │  Due Date Factor (if dueDate != null)│
        │  Overdue: +30                      │
        │  Today: +20                        │
        │  ≤2 days: +15                      │
        │  ≤7 days: +10                      │
        └───────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │  Status Penalty                    │
        │  DONE: -50                        │
        │  REVIEW: -15                      │
        └───────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │  Tag Boost (if has blocker/       │
        │  critical/urgent tag): +8         │
        └───────────────────────────────────┘
                            │
                            ▼
        ┌───────────────────────────────────┐
        │  Recency Boost (if updated <1 day)│
        │  +5                                │
        └───────────────────────────────────┘
                            │
                            ▼
                    Return Final Score
```

## Insights and Learning Points

### Algorithm Design Patterns

1. **Composite Scoring Pattern**: Multiple independent factors are combined additively
2. **Weighted Factor System**: Different factors have different weights based on business importance
3. **Functional Programming**: Uses streams and immutable operations
4. **Null Safety**: Checks for null before accessing due date

### Key Design Decisions

1. **Why additive scoring?**
   - Simple to understand and maintain
   - Easy to adjust weights
   - Factors are independent

2. **Why large penalties for completed tasks?**
   - Ensures completed tasks don't clutter priority lists
   - Even urgent completed tasks are deprioritized

3. **Why recency boost?**
   - Recently updated tasks are likely active
   - Helps surface tasks being worked on

### Potential Improvements Identified

1. **Configurable Weights**: Make weights configurable rather than hardcoded
2. **Score Caching**: Cache scores if tasks don't change frequently
3. **Normalization**: Consider normalizing scores to 0-100 range for easier interpretation
4. **Time-based Decay**: Make recency boost decay gradually rather than binary

## Reflection Questions

### How did the AI's explanation change your understanding?

The AI helped me understand that this isn't just a simple priority system, but a sophisticated multi-factor scoring algorithm. I initially thought it was just priority + due date, but the AI revealed the nuanced interactions between factors, especially how status penalties can override other factors.

### What aspects were still difficult to understand after AI explanation?

The temporal calculations using `ChronoUnit.DAYS.between()` were initially confusing. The AI helped clarify that this calculates the difference in days, which can be negative (for overdue tasks).

### How would you explain this algorithm to another junior developer?

"This algorithm calculates a priority score by starting with a base score from the task's priority level (multiplied by 10). Then it adds points if the task is due soon (more points for more urgent), subtracts points if it's completed or in review, adds points for special tags, and adds points if it was recently updated. Higher scores mean higher priority."

### Did you test this understanding against AI?

Yes, I asked the AI to verify my understanding by explaining edge cases:
- What if a task has no due date? (Skipped entirely)
- What if a task is URGENT but DONE? (Score: 40 - 50 = -10, so low priority)
- What if a task has multiple special tags? (Only +8 total, not per tag)

### How might you improve the algorithm based on your understanding?

1. **Add configuration**: Make weights configurable via a properties file
2. **Add logging**: Log score calculations for debugging
3. **Add validation**: Ensure scores are within expected ranges
4. **Add documentation**: Document the scoring formula more explicitly
5. **Consider normalization**: Normalize to 0-100 scale for easier interpretation

## Code Snippets for Reference

### Original Algorithm

```java
public static int calculateTaskScore(Task task) {
    // Base priority weights
    Map<TaskPriority, Integer> priorityWeights = Map.of(
        TaskPriority.LOW, 1,
        TaskPriority.MEDIUM, 2,
        TaskPriority.HIGH, 3,
        TaskPriority.URGENT, 4
    );

    // Calculate base score from priority
    int score = priorityWeights.getOrDefault(task.getPriority(), 0) * 10;

    // Add due date factor (higher score for tasks due sooner)
    if (task.getDueDate() != null) {
        long daysUntilDue = ChronoUnit.DAYS.between(
            LocalDateTime.now(),
            task.getDueDate()
        );

        if (daysUntilDue < 0) { // Overdue tasks
            score += 30;
        } else if (daysUntilDue == 0) { // Due today
            score += 20;
        } else if (daysUntilDue <= 2) { // Due in next 2 days
            score += 15;
        } else if (daysUntilDue <= 7) { // Due in next week
            score += 10;
        }
    }

    // Reduce score for tasks that are completed or in review
    if (task.getStatus() == TaskStatus.DONE) {
        score -= 50;
    } else if (task.getStatus() == TaskStatus.REVIEW) {
        score -= 15;
    }

    // Boost score for tasks with certain tags
    if (task.getTags().stream().anyMatch(tag ->
        List.of("blocker", "critical", "urgent").contains(tag))) {
        score += 8;
    }

    // Boost score for recently updated tasks
    long daysSinceUpdate = ChronoUnit.DAYS.between(
        task.getUpdatedAt(),
        LocalDateTime.now()
    );
    if (daysSinceUpdate < 1) {
        score += 5;
    }

    return score;
}
```

## Conclusion

Through systematic AI-assisted analysis, I gained a comprehensive understanding of the task priority scoring algorithm. The three-prompt approach (understanding, deepening, and mapping) provided complementary perspectives that together revealed the algorithm's design, data flow, and potential improvements. This exercise demonstrated how AI can be a powerful learning partner for understanding complex algorithms.

