# Code Documentation Exercise

## Exercise Overview

This exercise practices using AI to generate comprehensive documentation for undocumented code. You'll apply prompt strategies to create function/method documentation that helps other developers understand and use the code correctly.

## Selected Code: TaskPriorityManager.calculateTaskScore

We'll document the `calculateTaskScore` method from the TaskPriorityManager class.

## Original Code (Undocumented)

```java
public static int calculateTaskScore(Task task) {
    Map<TaskPriority, Integer> priorityWeights = Map.of(
        TaskPriority.LOW, 1,
        TaskPriority.MEDIUM, 2,
        TaskPriority.HIGH, 3,
        TaskPriority.URGENT, 4
    );

    int score = priorityWeights.getOrDefault(task.getPriority(), 0) * 10;

    if (task.getDueDate() != null) {
        long daysUntilDue = ChronoUnit.DAYS.between(
            LocalDateTime.now(),
            task.getDueDate()
        );

        if (daysUntilDue < 0) {
            score += 30;
        } else if (daysUntilDue == 0) {
            score += 20;
        } else if (daysUntilDue <= 2) {
            score += 15;
        } else if (daysUntilDue <= 7) {
            score += 10;
        }
    }

    if (task.getStatus() == TaskStatus.DONE) {
        score -= 50;
    } else if (task.getStatus() == TaskStatus.REVIEW) {
        score -= 15;
    }

    if (task.getTags().stream().anyMatch(tag ->
        List.of("blocker", "critical", "urgent").contains(tag))) {
        score += 8;
    }

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

## Prompt 1: Comprehensive Function Documentation

**Prompt Used:**
```
Please create comprehensive documentation for this function following JavaDoc conventions:

```java
[Paste calculateTaskScore method]
```

The documentation should include:
1. A clear description of what the function does
2. All parameters with types and descriptions
3. Return value with type and description
4. Any exceptions or errors that might be thrown
5. Example usage
6. Any important notes or edge cases developers should be aware of
```

**Generated Documentation (Prompt 1):**

```java
/**
 * Calculates a composite priority score for a task based on multiple weighted factors.
 * 
 * <p>The scoring system combines several factors to determine task importance:
 * <ul>
 *   <li>Base priority level (LOW=10, MEDIUM=20, HIGH=30, URGENT=40)</li>
 *   <li>Due date proximity (overdue tasks get highest boost)</li>
 *   <li>Status penalties (completed tasks are heavily penalized)</li>
 *   <li>Special tag boosts (blocker, critical, urgent tags add points)</li>
 *   <li>Recency boost (recently updated tasks get a small boost)</li>
 * </ul>
 * 
 * <p>The final score can be positive or negative. Higher scores indicate higher priority.
 * Completed tasks typically have negative scores to ensure they appear at the bottom
 * of sorted lists.
 * 
 * @param task The task for which to calculate the priority score. Must not be null.
 * @return An integer score representing the task's priority. Higher scores indicate
 *         higher priority. The score can be negative for completed tasks.
 * 
 * @throws NullPointerException if the task parameter is null
 * 
 * @example
 * <pre>{@code
 * Task urgentTask = new Task("Fix critical bug");
 * urgentTask.setPriority(TaskPriority.URGENT);
 * urgentTask.setDueDate(LocalDateTime.now().plusDays(1));
 * urgentTask.setStatus(TaskStatus.TODO);
 * 
 * int score = TaskPriorityManager.calculateTaskScore(urgentTask);
 * // Score will be: 40 (URGENT) + 15 (due in 1 day) = 55
 * }</pre>
 * 
 * @example
 * <pre>{@code
 * Task completedTask = new Task("Completed task");
 * completedTask.setPriority(TaskPriority.HIGH);
 * completedTask.setStatus(TaskStatus.DONE);
 * 
 * int score = TaskPriorityManager.calculateTaskScore(completedTask);
 * // Score will be: 30 (HIGH) - 50 (DONE) = -20
 * }</pre>
 * 
 * @note The scoring weights are hardcoded. To change weights, modify the method directly.
 * @note Tasks without due dates skip the due date factor entirely.
 * @note The recency boost only applies to tasks updated within the last 24 hours.
 * @note Multiple special tags don't stack - having multiple special tags still only adds +8.
 */
public static int calculateTaskScore(Task task) {
    // ... implementation
}
```

## Prompt 2: Intent and Logic Explanation

**Prompt Used:**
```
I need help documenting the intent and logic behind this code. Please:

```java
[Paste calculateTaskScore method]
```

1. Explain what this code is trying to accomplish at a high level
2. Break down the logic step-by-step
3. Identify any assumptions or edge cases in the implementation
4. Suggest inline comments for complex parts
5. Note any potential improvements while maintaining the original functionality
```

**AI Analysis and Insights:**

### High-Level Intent

The code implements a multi-factor scoring algorithm to rank tasks by importance. It's designed to surface urgent, incomplete tasks that are due soon while deprioritizing completed or inactive tasks.

### Step-by-Step Logic Breakdown

1. **Base Priority Calculation**: Maps priority enum to numeric weights (1-4) and multiplies by 10
2. **Due Date Factor**: Calculates days until due date and adds points based on urgency
3. **Status Penalties**: Reduces score for completed or in-review tasks
4. **Tag Boost**: Checks for special tags and adds bonus points
5. **Recency Boost**: Adds points for recently updated tasks

### Assumptions and Edge Cases

**Assumptions:**
- Task object is fully initialized with all required fields
- `updatedAt` is always set (never null)
- Priority enum values match the hardcoded weights

**Edge Cases:**
- Null due date: Due date factor is skipped
- Negative days (overdue): Handled with `daysUntilDue < 0` check
- Empty tag list: Tag boost is skipped
- Completed high-priority task: Can result in negative score

### Suggested Inline Comments

```java
public static int calculateTaskScore(Task task) {
    // Define priority weight mapping: LOW=1, MEDIUM=2, HIGH=3, URGENT=4
    Map<TaskPriority, Integer> priorityWeights = Map.of(
        TaskPriority.LOW, 1,
        TaskPriority.MEDIUM, 2,
        TaskPriority.HIGH, 3,
        TaskPriority.URGENT, 4
    );

    // Calculate base score: priority weight × 10
    // This ensures priority level is the dominant factor (10, 20, 30, 40)
    int score = priorityWeights.getOrDefault(task.getPriority(), 0) * 10;

    // Due date proximity factor: tasks due sooner get higher scores
    if (task.getDueDate() != null) {
        // Calculate days between now and due date
        // Negative values indicate overdue tasks
        long daysUntilDue = ChronoUnit.DAYS.between(
            LocalDateTime.now(),
            task.getDueDate()
        );

        // Overdue tasks get the highest boost to surface them immediately
        if (daysUntilDue < 0) {
            score += 30;
        } 
        // Tasks due today need immediate attention
        else if (daysUntilDue == 0) {
            score += 20;
        } 
        // Tasks due in next 2 days are urgent
        else if (daysUntilDue <= 2) {
            score += 15;
        } 
        // Tasks due within a week are moderately urgent
        else if (daysUntilDue <= 7) {
            score += 10;
        }
        // Tasks due beyond a week get no due date boost
    }

    // Status-based penalties: deprioritize completed and in-review tasks
    if (task.getStatus() == TaskStatus.DONE) {
        // Large penalty ensures completed tasks don't appear in priority lists
        score -= 50;
    } else if (task.getStatus() == TaskStatus.REVIEW) {
        // Smaller penalty for tasks awaiting review
        score -= 15;
    }

    // Tag-based boost: special tags indicate critical tasks
    if (task.getTags().stream().anyMatch(tag ->
        List.of("blocker", "critical", "urgent").contains(tag))) {
        // Boost applies if any special tag is present (doesn't stack)
        score += 8;
    }

    // Recency boost: recently updated tasks are likely active
    long daysSinceUpdate = ChronoUnit.DAYS.between(
        task.getUpdatedAt(),
        LocalDateTime.now()
    );
    // Only boost tasks updated within the last 24 hours
    if (daysSinceUpdate < 1) {
        score += 5;
    }

    return score;
}
```

### Potential Improvements

1. **Extract magic numbers**: Move weights to constants or configuration
2. **Add null checks**: Explicitly handle null task parameter
3. **Make weights configurable**: Allow customization without code changes
4. **Add logging**: Log score calculations for debugging
5. **Consider normalization**: Normalize scores to 0-100 range

## Final Combined Documentation

**Best Elements from Both Approaches:**

```java
/**
 * Calculates a composite priority score for a task based on multiple weighted factors.
 * 
 * <p>This method implements a multi-factor scoring algorithm that combines:
 * <ul>
 *   <li><b>Base Priority</b>: Priority level multiplied by 10 (LOW=10, MEDIUM=20, HIGH=30, URGENT=40)</li>
 *   <li><b>Due Date Proximity</b>: Points added based on how soon the task is due:
 *       <ul>
 *         <li>Overdue: +30 points</li>
 *         <li>Due today: +20 points</li>
 *         <li>Due in 2 days: +15 points</li>
 *         <li>Due in a week: +10 points</li>
 *       </ul>
 *   </li>
 *   <li><b>Status Penalties</b>: Reduces score for inactive tasks:
 *       <ul>
 *         <li>DONE: -50 points</li>
 *         <li>REVIEW: -15 points</li>
 *       </ul>
 *   </li>
 *   <li><b>Tag Boost</b>: +8 points for tasks with "blocker", "critical", or "urgent" tags</li>
 *   <li><b>Recency Boost</b>: +5 points for tasks updated within the last 24 hours</li>
 * </ul>
 * 
 * <p>The final score can be positive or negative. Higher scores indicate higher priority.
 * Completed tasks typically have negative scores to ensure they appear at the bottom
 * of sorted lists.
 * 
 * <p><b>Scoring Formula:</b>
 * <pre>
 * score = (priority_weight × 10) 
 *       + due_date_factor 
 *       - status_penalty 
 *       + tag_boost 
 *       + recency_boost
 * </pre>
 * 
 * @param task The task for which to calculate the priority score. Must not be null.
 *             The task should have all required fields initialized (priority, status, updatedAt).
 * @return An integer score representing the task's priority. Higher scores indicate
 *         higher priority. The score can be negative for completed tasks.
 * 
 * @throws NullPointerException if the task parameter is null or if task.getUpdatedAt() is null
 * 
 * @example Calculating score for an urgent task due tomorrow:
 * <pre>{@code
 * Task task = new Task("Fix critical bug");
 * task.setPriority(TaskPriority.URGENT);
 * task.setDueDate(LocalDateTime.now().plusDays(1));
 * task.setStatus(TaskStatus.TODO);
 * task.setUpdatedAt(LocalDateTime.now());
 * 
 * int score = TaskPriorityManager.calculateTaskScore(task);
 * // Result: 40 (URGENT base) + 15 (due in 1 day) + 5 (recently updated) = 60
 * }</pre>
 * 
 * @example Calculating score for a completed high-priority task:
 * <pre>{@code
 * Task task = new Task("Completed task");
 * task.setPriority(TaskPriority.HIGH);
 * task.setStatus(TaskStatus.DONE);
 * task.setUpdatedAt(LocalDateTime.now());
 * 
 * int score = TaskPriorityManager.calculateTaskScore(task);
 * // Result: 30 (HIGH base) - 50 (DONE penalty) = -20
 * }</pre>
 * 
 * @example Task without due date:
 * <pre>{@code
 * Task task = new Task("No deadline task");
 * task.setPriority(TaskPriority.MEDIUM);
 * task.setDueDate(null); // No due date
 * task.setStatus(TaskStatus.TODO);
 * 
 * int score = TaskPriorityManager.calculateTaskScore(task);
 * // Result: 20 (MEDIUM base) - due date factor is skipped
 * }</pre>
 * 
 * @note The scoring weights are currently hardcoded. To modify weights, edit the method directly.
 * @note Tasks without due dates skip the due date factor entirely (no penalty or boost).
 * @note The recency boost only applies to tasks updated within the last 24 hours (daysSinceUpdate < 1).
 * @note Multiple special tags don't stack - having "blocker" and "critical" still only adds +8 total.
 * @note The algorithm can produce negative scores, which is intentional for completed tasks.
 * 
 * @see TaskPriority
 * @see TaskStatus
 * @see #sortTasksByImportance(List)
 * @see #getTopPriorityTasks(List, int)
 */
public static int calculateTaskScore(Task task) {
    // Implementation with inline comments from Prompt 2...
}
```

## What I Learned

### Most Challenging Parts

1. **Explaining the scoring formula clearly**: The AI helped me understand that I needed to show the formula explicitly
2. **Documenting edge cases**: The AI pointed out edge cases I hadn't considered (null due dates, negative scores)
3. **Writing good examples**: The AI showed me how to write examples that demonstrate different scenarios

### Additional Information Needed

1. **Context about usage**: I needed to explain how this method is used in the broader system
2. **Business rules**: Understanding why certain weights were chosen helped write better documentation
3. **Related methods**: Documenting relationships to `sortTasksByImportance` and `getTopPriorityTasks`

### How to Use This Approach

1. **Start with Prompt 1** for comprehensive documentation structure
2. **Use Prompt 2** to understand intent and add inline comments
3. **Combine the best elements** from both approaches
4. **Review and refine** based on actual usage and feedback
5. **Keep documentation updated** as code evolves

## Comparison: Before vs. After

**Before:** Minimal JavaDoc comment, no examples, no edge case documentation

**After:** Comprehensive documentation with:
- Clear purpose and algorithm explanation
- Detailed parameter and return descriptions
- Multiple examples showing different scenarios
- Edge case notes and assumptions
- Related method references
- Inline comments explaining complex logic

