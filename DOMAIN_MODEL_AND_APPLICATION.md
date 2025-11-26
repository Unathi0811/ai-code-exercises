# Exercise Part 3: Understanding Domain Model

## Extract Domain Model

### Core Entity Classes Identified

1. **Task** (`za.co.wethinkcode.taskmanager.model.Task`)
   - Primary domain entity representing a work item or task
   - Contains: id, title, description, priority, status, timestamps, due date, tags

2. **TaskPriority** (`za.co.wethinkcode.taskmanager.model.TaskPriority`)
   - Enum representing task urgency levels
   - Values: LOW(1), MEDIUM(2), HIGH(3), URGENT(4)
   - Provides value conversion between numeric and enum representations

3. **TaskStatus** (`za.co.wethinkcode.taskmanager.model.TaskStatus`)
   - Enum representing task lifecycle states
   - Values: TODO("todo"), IN_PROGRESS("in_progress"), REVIEW("review"), DONE("done")
   - Provides value conversion between string and enum representations

### Business Logic Related to Tasks

1. **Task Lifecycle Management**
   - Tasks are created with status `TODO` by default
   - Tasks can transition through statuses: TODO → IN_PROGRESS → REVIEW → DONE
   - When marked as DONE, `completedAt` timestamp is automatically set
   - `updatedAt` timestamp is maintained on all modifications

2. **Overdue Detection**
   - `isOverdue()` method checks if a task's due date has passed
   - Only considers tasks overdue if they are NOT in DONE status
   - Tasks without due dates are never considered overdue

3. **Task Updates**
   - `update()` method allows partial updates (only non-null fields are updated)
   - Automatically updates `updatedAt` timestamp on any modification
   - Tags can be added/removed individually or replaced entirely

4. **Priority Management**
   - Priority is numeric (1-4) but represented as enum for type safety
   - Default priority is MEDIUM when not specified
   - Priority can be converted from numeric value or name string

5. **Tag Management**
   - Tags are stored as a list of strings
   - Duplicate tags are prevented (addTag checks for existence)
   - Tags can be used for categorization and filtering

### Domain-Specific Terminology

- **Task**: A work item with title, description, and metadata
- **Priority**: Urgency level (LOW, MEDIUM, HIGH, URGENT)
- **Status**: Current state in task lifecycle (TODO, IN_PROGRESS, REVIEW, DONE)
- **Overdue**: Task with past due date that is not completed
- **Tag**: Categorization label for organizing tasks
- **Due Date**: Target completion date/time for a task
- **Completed At**: Timestamp when task was marked as DONE

---

## Form Initial Understanding

### Entity Relationship Diagram (Initial)

```
┌─────────────────────────────────────────────────────────┐
│                        Task                              │
├─────────────────────────────────────────────────────────┤
│ - id: String (UUID)                                     │
│ - title: String                                         │
│ - description: String                                    │
│ - priority: TaskPriority ───────┐                       │
│ - status: TaskStatus ───────────┼───┐                  │
│ - createdAt: LocalDateTime       │   │                  │
│ - updatedAt: LocalDateTime       │   │                  │
│ - dueDate: LocalDateTime         │   │                  │
│ - completedAt: LocalDateTime     │   │                  │
│ - tags: List<String>             │   │                  │
├──────────────────────────────────┼───┼──────────────────┤
│ + update(Task updates)           │   │                  │
│ + markAsDone()                    │   │                  │
│ + isOverdue(): boolean            │   │                  │
│ + addTag(String tag)              │   │                  │
│ + removeTag(String tag): boolean   │   │                  │
└──────────────────────────────────┼───┼──────────────────┘
                                   │   │
                    ┌──────────────┘   └──────────────┐
                    │                                 │
                    ▼                                 ▼
        ┌──────────────────────┐      ┌──────────────────────┐
        │   TaskPriority       │      │    TaskStatus        │
        │   (Enum)             │      │    (Enum)            │
        ├──────────────────────┤      ├──────────────────────┤
        │ LOW(1)               │      │ TODO("todo")         │
        │ MEDIUM(2)            │      │ IN_PROGRESS(...)     │
        │ HIGH(3)              │      │ REVIEW("review")    │
        │ URGENT(4)            │      │ DONE("done")         │
        ├──────────────────────┤      ├──────────────────────┤
        │ + getValue(): int    │      │ + getValue(): String │
        │ + fromValue(int)     │      │ + fromValue(String) │
        └──────────────────────┘      └──────────────────────┘
```

### Brief Explanation of Each Entity

**Task**: The central domain entity representing a work item. It encapsulates all task-related data and business logic. Tasks have a unique identifier (UUID), descriptive information (title, description), classification (priority, status, tags), temporal information (creation, update, due date, completion), and behavior (updating, marking done, checking overdue status).

**TaskPriority**: An enumeration that represents the urgency or importance level of a task. It uses numeric values (1-4) internally but provides a type-safe enum interface. The priority affects how tasks are displayed and potentially filtered, with URGENT being the highest priority.

**TaskStatus**: An enumeration representing the current state of a task in its lifecycle. Tasks progress from TODO (not started) through IN_PROGRESS (actively being worked on) and REVIEW (awaiting review) to DONE (completed). The status determines whether a task can be considered overdue and affects various business rules.

### Questions and Confusion About Business Logic

1. **Status Transitions**: Are there any restrictions on status transitions? Can a task go directly from TODO to DONE, or must it follow the sequence?

2. **Abandoned Tasks**: There's no "ABANDONED" status. How are abandoned or cancelled tasks handled? Should they be marked as DONE or remain in their current status?

3. **Priority Changes**: Can priority be changed after a task is created? Are there any business rules about when priority should or shouldn't change?

4. **Overdue Logic**: The `isOverdue()` method only checks if due date is before now and status is not DONE. Should there be additional logic for tasks that are overdue by a certain number of days?

5. **Completed At vs Updated At**: When a task is marked as DONE, both `completedAt` and `updatedAt` are set. But what if a task is updated after being marked DONE? Does `completedAt` remain unchanged?

6. **Tag Semantics**: Are tags case-sensitive? Can they contain special characters? Is there a maximum number of tags per task?

---

## Apply the Domain Understanding Prompt

I'd like you to act as a senior developer who deeply understands our codebase's domain model. I'm a junior developer trying to make sense of the business logic and domain concepts in this application.

### Here's what I've found in the codebase:

**Task.java** - The main domain entity with properties:
- id (UUID), title, description
- priority (TaskPriority enum), status (TaskStatus enum)
- createdAt, updatedAt, dueDate, completedAt (LocalDateTime)
- tags (List<String>)
- Business methods: update(), markAsDone(), isOverdue(), addTag(), removeTag()

**TaskPriority.java** - Enum with values LOW(1), MEDIUM(2), HIGH(3), URGENT(4)
- Provides fromValue(int) for conversion

**TaskStatus.java** - Enum with values TODO("todo"), IN_PROGRESS("in_progress"), REVIEW("review"), DONE("done")
- Provides fromValue(String) for conversion

### Based on this code, my current understanding is:

- **The system seems to be modeling**: A task management system where work items (tasks) progress through a lifecycle with different priority levels and can be organized using tags.

- **I think Task and TaskPriority/TaskStatus are related because**: Task uses these enums as properties, and they provide type safety and value conversion. TaskPriority represents urgency while TaskStatus represents lifecycle state.

- **The `isOverdue()` method appears to represent**: A business rule that a task is only considered overdue if it has a due date that has passed AND it's not already completed. This makes sense because completed tasks shouldn't be considered overdue.

- **I'm confused about why**: 
  - There's no explicit status transition validation - can tasks skip statuses?
  - There's no "ABANDONED" or "CANCELLED" status for tasks that won't be completed
  - The `update()` method allows partial updates but doesn't validate business rules (e.g., can you set status to DONE without using markAsDone()?)
  - Tags are just strings with no validation or constraints

### Could you, as a senior developer:

1. **Validate my current understanding, correcting any misconceptions**
2. **Help me recognize the core domain concepts represented in this code**
3. **Explain the relationships between these entities in business terms, not just technical relationships**
4. **Clarify any domain-specific terminology or patterns I might be missing**
5. **Help me connect these models to actual user-facing features or business processes**

### Then, please ask me 3-5 questions that would test my understanding of this domain model.

### Finally, suggest a simple diagram I could sketch to visualize these relationships.

---

## Test Your Knowledge

### Questions from AI (with answers):

**Q1: If a task has been overdue for 30 days but is still in TODO status, what business implications does this have? How would you identify such tasks in the current system?**

**Answer**: In the current system, such tasks would be identified by calling `isOverdue()` which returns true. However, there's no distinction between tasks overdue for 1 day vs 30 days. The business implication is that these tasks are likely abandoned or forgotten, but the system doesn't have a mechanism to handle this. We could filter tasks using `getOverdueTasks()` in TaskStorage, but there's no automatic escalation or notification mechanism.

**Q2: What happens to a task's `completedAt` timestamp if someone calls `task.setStatus(TaskStatus.DONE)` directly instead of using `markAsDone()`?**

**Answer**: If someone bypasses `markAsDone()` and uses `setStatus()` directly, the `completedAt` timestamp would remain null. This breaks the business rule that completed tasks should have a completion timestamp. The `markAsDone()` method ensures both the status and timestamp are set correctly, maintaining data integrity.

**Q3: How would you implement a feature that automatically increases a task's priority if it's been overdue for more than 7 days?**

**Answer**: I would need to:
1. Add a method in TaskManager that finds overdue tasks older than 7 days
2. Check each task's current priority
3. If priority is not already URGENT, increase it (e.g., LOW→MEDIUM, MEDIUM→HIGH, HIGH→URGENT)
4. Update the task and save
5. This could be called periodically or on-demand

**Q4: In business terms, what's the difference between a task being in REVIEW status versus being DONE? When would you use each?**

**Answer**: REVIEW status means the work is complete but awaiting approval or verification. DONE means the task is fully complete and approved. In business terms, REVIEW is a quality gate - the work is done but needs validation before being considered truly complete. DONE is the final state indicating the task has passed all checks and is closed.

**Q5: If a task has tags ["urgent", "client-a", "bug"], how would you use these tags to support business processes?**

**Answer**: Tags enable flexible categorization and filtering:
- Filter all "bug" tasks to see technical debt
- Filter "client-a" tasks to see work for a specific client
- Combine filters to find urgent bugs for client-a
- Generate reports by tag (e.g., all client-a tasks)
- Tags are more flexible than fixed categories because tasks can have multiple tags

### Revised Entity Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                         Task                                 │
│                  (Aggregate Root)                            │
├─────────────────────────────────────────────────────────────┤
│ Identity:                                                    │
│   - id: UUID (immutable, generated on creation)            │
│                                                              │
│ Descriptive Properties:                                      │
│   - title: String (required)                                │
│   - description: String                                      │
│                                                              │
│ Classification:                                              │
│   - priority: TaskPriority ──────┐                          │
│   - status: TaskStatus ───────────┼───┐                     │
│   - tags: List<String>            │   │                     │
│                                                              │
│ Temporal Properties:                                         │
│   - createdAt: LocalDateTime (immutable)                    │
│   - updatedAt: LocalDateTime (mutable, auto-updated)         │
│   - dueDate: LocalDateTime (nullable)                       │
│   - completedAt: LocalDateTime (nullable, set on DONE)      │
├──────────────────────────────────┼───┼──────────────────────┤
│ Business Operations:                                         │
│   + update(Task) - Partial update, preserves invariants     │
│   + markAsDone() - Completes task, sets timestamp           │
│   + isOverdue(): boolean - Business rule: past due & !DONE  │
│   + addTag(String) - Prevents duplicates                     │
│   + removeTag(String) - Returns success status              │
└──────────────────────────────────┼───┼──────────────────────┘
                                   │   │
                    ┌──────────────┘   └──────────────┐
                    │                                   │
                    ▼                                   ▼
        ┌──────────────────────┐      ┌──────────────────────┐
        │   TaskPriority       │      │    TaskStatus        │
        │   (Value Object)     │      │    (Value Object)    │
        ├──────────────────────┤      ├──────────────────────┤
        │ LOW = 1              │      │ TODO                 │
        │ MEDIUM = 2           │      │ IN_PROGRESS          │
        │ HIGH = 3             │      │ REVIEW               │
        │ URGENT = 4           │      │ DONE                 │
        ├──────────────────────┤      ├──────────────────────┤
        │ Business Rules:      │      │ Business Rules:       │
        │ - Higher = more     │      │ - Sequential flow    │
        │   urgent            │      │   preferred          │
        │ - Affects display   │      │ - DONE is terminal   │
        │   and filtering      │      │ - Affects overdue    │
        │                      │      │   calculation        │
        └──────────────────────┘      └──────────────────────┘

Business Rules:
1. Tasks start as TODO with MEDIUM priority
2. Overdue = (dueDate < now) AND (status != DONE)
3. markAsDone() sets both status and completedAt
4. Tags are case-sensitive strings, no duplicates
5. updatedAt is maintained on all modifications
```

### Glossary of Domain Terms

| Term | Definition | Usage Context |
|------|------------|---------------|
| **Task** | A work item with title, description, and metadata | Core domain entity representing something that needs to be done |
| **Priority** | Urgency level of a task (LOW, MEDIUM, HIGH, URGENT) | Used to indicate importance and affect task ordering/filtering |
| **Status** | Current lifecycle state (TODO, IN_PROGRESS, REVIEW, DONE) | Tracks progress through task completion workflow |
| **Overdue** | Task with past due date that is not completed | Business rule: dueDate < now AND status != DONE |
| **Tag** | Categorization label for organizing tasks | Flexible metadata for filtering and grouping tasks |
| **Due Date** | Target completion date/time | Optional deadline for task completion |
| **Completed At** | Timestamp when task was marked DONE | Set automatically when markAsDone() is called |
| **Lifecycle** | Sequence of status transitions | TODO → IN_PROGRESS → REVIEW → DONE (preferred flow) |

---

# Exercise Part 4: Practical Application

## Scenario

Your team needs to implement a new business rule: **"Tasks that are overdue for more than 7 days should be automatically marked as abandoned unless they are marked as high priority."**

## Planning

### Files That Would Need to Be Modified

1. **TaskStatus.java** (NEW STATUS NEEDED)
   - Add `ABANDONED("abandoned")` to the enum
   - Update `fromValue()` method to handle "abandoned"

2. **Task.java** (POSSIBLY)
   - May need a method like `markAsAbandoned()` similar to `markAsDone()`
   - Or could use existing `setStatus()` method

3. **TaskManager.java** (PRIMARY LOCATION)
   - Add method: `markAbandonedTasks()` or `processAbandonedTasks()`
   - Logic: Find tasks overdue > 7 days, check priority, mark as abandoned
   - Could be called on-demand or scheduled

4. **TaskStorage.java** (POSSIBLY)
   - May need a method to find tasks overdue by X days
   - Currently has `getOverdueTasks()` but doesn't filter by days overdue

5. **TaskManagerCli.java** (CLI INTEGRATION)
   - Add command: `abandon` or `process-abandoned`
   - Could be manual trigger or automatic

### Outline of Changes

#### Change 1: Add ABANDONED Status
```java
// TaskStatus.java
public enum TaskStatus {
    TODO("todo"),
    IN_PROGRESS("in_progress"),
    REVIEW("review"),
    DONE("done"),
    ABANDONED("abandoned");  // NEW
    
    // fromValue() already handles this automatically
}
```

#### Change 2: Add Helper Method to Task (Optional)
```java
// Task.java
public void markAsAbandoned() {
    this.status = TaskStatus.ABANDONED;
    this.updatedAt = LocalDateTime.now();
    // Note: completedAt remains null for abandoned tasks
}
```

#### Change 3: Add Method to Find Tasks Overdue by Days
```java
// TaskStorage.java
public List<Task> getTasksOverdueByDays(int days) {
    LocalDateTime cutoff = LocalDateTime.now().minusDays(days);
    return tasks.values().stream()
        .filter(task -> task.isOverdue() && 
                       task.getDueDate() != null &&
                       task.getDueDate().isBefore(cutoff))
        .collect(Collectors.toList());
}
```

#### Change 4: Add Business Logic to TaskManager
```java
// TaskManager.java
public int markAbandonedTasks() {
    List<Task> overdueTasks = getStorage().getTasksOverdueByDays(7);
    int markedCount = 0;
    
    for (Task task : overdueTasks) {
        // Only mark as abandoned if not HIGH or URGENT priority
        if (task.getPriority() != TaskPriority.HIGH && 
            task.getPriority() != TaskPriority.URGENT) {
            task.markAsAbandoned(); // or task.setStatus(TaskStatus.ABANDONED)
            getStorage().save();
            markedCount++;
        }
    }
    
    return markedCount;
}
```

#### Change 5: Add CLI Command (Optional)
```java
// TaskManagerCli.java
case "process-abandoned":
    handleProcessAbandonedCommand();
    break;

private static void handleProcessAbandonedCommand() {
    int count = taskManager.markAbandonedTasks();
    System.out.println("Marked " + count + " tasks as abandoned.");
}
```

### Questions to Ask Team Before Implementing

1. **Status Behavior**: Should ABANDONED tasks be excluded from normal task lists? Should they appear in statistics?

2. **Reversibility**: Can abandoned tasks be "un-abandoned" and returned to active status, or is ABANDONED a terminal state like DONE?

3. **Priority Definition**: The requirement says "high priority" - does this mean only HIGH and URGENT, or just HIGH? Should MEDIUM priority tasks be protected?

4. **Execution Timing**: Should this run automatically (scheduled/cron) or be a manual command? If automatic, how often?

5. **Notification**: Should users be notified when their tasks are marked as abandoned?

6. **Edge Cases**: 
   - What if a task is overdue by 7 days but was updated recently (updatedAt < 7 days ago)?
   - Should tasks already in DONE status be excluded (they shouldn't be overdue anyway)?
   - What about tasks in REVIEW status that are overdue?

7. **Reporting**: Should there be a way to see which tasks were abandoned and when?

8. **Business Impact**: Are there any downstream systems or reports that need to account for ABANDONED status?

## Reflection

### How AI Prompts Helped

1. **Domain Understanding Prompt**: Helped me understand that TaskStatus is a value object with specific business meanings. This made it clear that adding ABANDONED requires understanding its relationship to other statuses and business rules.

2. **Feature Location Prompt**: Guided me to identify that TaskManager is the right place for orchestration logic, while TaskStorage handles data retrieval. This separation of concerns made the implementation plan clearer.

3. **Code Understanding Prompts**: Helped me recognize patterns like `markAsDone()` which I can follow for `markAsAbandoned()`, and the filtering patterns in TaskStorage that I can extend.

### Aspects Still Unclear

1. **Status Transition Rules**: The codebase doesn't enforce status transitions. Should ABANDONED be reachable from any status, or only specific ones?

2. **Concurrency**: If multiple users run the abandon process simultaneously, could there be race conditions?

3. **Performance**: If there are thousands of tasks, iterating through all of them might be slow. Should there be indexing or optimization?

4. **Testing Strategy**: How should I test the 7-day threshold? Mock time or use actual dates?

### Next Steps to Deepen Understanding

1. **Review Test Files**: Look at existing tests to understand testing patterns and coverage expectations

2. **Check for Scheduled Tasks**: See if there's any existing scheduled job infrastructure

3. **Review Statistics Method**: Understand how `getStatistics()` works to see if ABANDONED should be included

4. **Study Error Handling**: Review how other operations handle errors to maintain consistency

5. **Check CLI Patterns**: Review how other commands handle optional parameters and output formatting

## Personal Reflection

### Which Prompt Was Most Helpful for Building Your Understanding?

The **Domain Understanding Prompt** was the most valuable for building my understanding. While the structure and feature location prompts helped me navigate the codebase technically, the domain understanding prompt forced me to think about the "why" behind the code, not just the "how."

Specifically, it helped me understand:
- **Business semantics**: That TaskStatus values aren't just arbitrary strings but represent meaningful business states with implications for how tasks behave
- **Domain relationships**: How Task, TaskPriority, and TaskStatus relate in business terms, not just as technical dependencies
- **Business rules**: The logic behind methods like `isOverdue()` - understanding that a task is only overdue if it has a due date AND isn't done, which reflects real-world business logic

The interactive questioning aspect was particularly valuable. When the AI asked me to explain what happens if someone bypasses `markAsDone()`, it revealed gaps in my understanding of data integrity and business rule enforcement. This type of Socratic questioning helped me build a deeper, more accurate mental model.

The domain understanding prompt also helped me recognize patterns I might have missed - like the fact that business logic is embedded in the domain entities themselves (Task.isOverdue(), Task.markAsDone()), which is a key architectural insight for extending the system.

### What Would You Do Differently Next Time You Approach an Unfamiliar Codebase?

1. **Start with tests before implementation code**: I would read the test files first to understand expected behavior, edge cases, and the "contract" that code must fulfill. Tests often reveal business rules and constraints that aren't obvious from implementation.

2. **Create a knowledge map earlier**: Instead of taking notes linearly, I'd create a visual map (mind map or diagram) showing relationships between components, data flow, and key concepts. This would help me see patterns and gaps more quickly.

3. **Use the "5 Whys" technique more**: When I encounter code that seems odd, I'd ask "why" multiple times to understand the root reason, not just the surface-level explanation. This would help me understand design decisions and constraints.

4. **Pair AI prompts with code execution**: I'd run the code, add logging, and trace execution paths while using AI prompts. The combination of static analysis (AI) and dynamic analysis (execution) provides a more complete picture.

5. **Build a glossary from the start**: I'd create a domain glossary immediately when encountering new terminology, rather than trying to remember everything. This would help me understand conversations, documentation, and code more quickly.

6. **Focus on one feature end-to-end**: Instead of trying to understand everything at once, I'd pick one feature and trace it completely from user input to data storage, understanding all layers involved. This provides a concrete anchor point for understanding the broader system.

7. **Ask for code reviews earlier**: I'd share my understanding with a team member or use AI to review my mental model earlier in the process, catching misconceptions before they become entrenched.

8. **Document assumptions explicitly**: I'd maintain a separate "assumptions" list that I validate as I learn more, rather than letting unverified assumptions guide my understanding.

### What Additional Tools or Resources Would Complement the AI Prompting Approach?

1. **Interactive Code Visualization Tools**:
   - **Code dependency graphs**: Tools like `jdeps` for Java or IDE plugins that show class dependencies visually
   - **Call hierarchy viewers**: To see how methods are called across the codebase
   - **Execution flow diagrams**: Tools that generate sequence diagrams from code

2. **Documentation Generation Tools**:
   - **API documentation generators**: To understand interfaces and contracts (Javadoc, etc.)
   - **Architecture decision records (ADRs)**: If the project maintains them, these explain "why" decisions were made
   - **UML diagram generators**: To visualize class relationships and system structure

3. **Testing and Exploration Tools**:
   - **Test coverage tools**: To identify untested code paths that might indicate edge cases or important functionality
   - **Mutation testing**: To understand which parts of code are critical
   - **Property-based testing tools**: To discover edge cases and constraints

4. **Version Control Analysis**:
   - **Git history exploration**: Tools like `git log --all --graph` or GUI tools to understand how the codebase evolved
   - **Blame annotations**: To see who wrote what and when, which can provide context
   - **Commit message analysis**: To understand the intent behind changes

5. **Runtime Analysis Tools**:
   - **Debuggers with breakpoints**: To step through code execution and see actual data flow
   - **Logging frameworks**: To add strategic logging and understand runtime behavior
   - **Profiling tools**: To understand performance characteristics and identify bottlenecks

6. **Collaboration Tools**:
   - **Code review platforms**: To see discussions and decisions about code
   - **Team wikis or documentation**: For domain knowledge and business context
   - **Pair programming sessions**: To learn from experienced team members

7. **Static Analysis Tools**:
   - **Linters and code quality tools**: To understand coding standards and patterns
   - **Complexity analyzers**: To identify complex areas that might need extra attention
   - **Security scanners**: To understand security considerations in the code

8. **Domain-Specific Resources**:
   - **Business documentation**: Requirements, user stories, or product documentation
   - **Domain expert interviews**: Conversations with product owners or business analysts
   - **User manuals or help documentation**: To understand how the system is intended to be used

9. **Learning and Reference Tools**:
   - **Design pattern references**: To recognize patterns used in the codebase
   - **Framework documentation**: For understanding library choices and conventions
   - **Language-specific best practices**: To understand idiomatic code in the language

10. **AI-Enhanced Tools**:
    - **Semantic code search**: Beyond grep, tools that understand code meaning
    - **Code explanation tools**: That provide inline explanations
    - **Automated documentation generators**: That create documentation from code and AI analysis

The key is using these tools **in combination** with AI prompting:
- Use AI prompts to build initial understanding and ask questions
- Use tools to verify, explore, and deepen that understanding
- Use AI again to interpret what tools reveal
- Create a feedback loop between AI insights and tool-based verification

This multi-tool approach ensures you're not relying solely on AI's interpretation but are actively exploring and validating your understanding through multiple channels.

---

# Final Submission: Codebase Understanding Summary

## Initial vs. Final Understanding

### Initial Understanding

Initially, I saw the Task Manager as a simple CRUD application with:
- Basic task entities with properties
- File-based storage using JSON
- Command-line interface for user interaction
- Simple enums for priority and status

I assumed:
- Status transitions were unrestricted
- All business logic was straightforward
- The architecture was flat with minimal separation of concerns

### Final Understanding

After exploration, I now understand:

**Architecture**: The system follows a layered architecture with clear separation:
- **Presentation Layer**: TaskManagerCli handles user interaction and command parsing
- **Business Logic Layer**: TaskManager orchestrates operations and enforces business rules
- **Data Access Layer**: TaskStorage abstracts persistence and provides query methods
- **Domain Model Layer**: Task, TaskPriority, TaskStatus represent core business concepts

**Business Rules**:
- Tasks have a lifecycle with preferred status transitions
- Overdue calculation has specific business logic (must have due date AND not be DONE)
- Priority affects task visibility and potentially business decisions
- Tags provide flexible categorization beyond fixed fields

**Design Patterns**:
- Repository Pattern (TaskStorage)
- Service Layer Pattern (TaskManager)
- Command Pattern (CLI command routing)
- Value Objects (TaskPriority, TaskStatus enums)
- Factory methods (fromValue() in enums)

**Key Insights**:
- Business logic is embedded in domain entities (Task.isOverdue(), Task.markAsDone())
- The system prioritizes simplicity over complex workflows
- File-based storage is appropriate for a lightweight CLI tool
- The codebase follows Java conventions and uses modern libraries (Gson, Commons CLI)

## Most Valuable Insights from Each Prompt

### Prompt 1: Understanding Project Structure
**Insight**: The layered architecture became clear - CLI → Business Logic → Storage → Model. This helped me understand where new features should be implemented.

**Key Learning**: Configuration files (build.gradle.kts) revealed dependencies and build system, which helped understand the technology stack.

### Prompt 2: Finding Feature Implementation
**Insight**: The pattern of file I/O in TaskStorage showed me how to implement CSV export. The separation between storage (data access) and utilities (transformation) clarified where export logic should live.

**Key Learning**: Searching for similar patterns (FileWriter usage) led me to the right location faster than guessing.

### Prompt 3: Understanding Domain Models
**Insight**: Understanding that TaskStatus and TaskPriority are value objects with business meaning, not just technical enums. This helped me understand why certain methods exist and how to extend the model correctly.

**Key Learning**: Business rules are encoded in methods like `isOverdue()` - understanding the "why" behind these methods is crucial.

### Prompt 4: Understanding Code Functionality
**Insight**: Tracing the flow from CLI command → TaskManager method → TaskStorage operation → Task entity helped me understand the complete execution path.

**Key Learning**: Data flows through layers, and each layer has specific responsibilities. Modifications should respect these boundaries.

## Approach to Implementing the New Business Rule

### Step 1: Domain Model Extension
- Add ABANDONED status to TaskStatus enum
- Consider adding `markAsAbandoned()` method to Task for consistency

### Step 2: Data Access Enhancement
- Extend TaskStorage with `getTasksOverdueByDays(int days)` method
- This follows the existing pattern of filtered queries

### Step 3: Business Logic Implementation
- Add `markAbandonedTasks()` to TaskManager
- Implement the rule: overdue > 7 days AND priority not HIGH/URGENT
- Return count of marked tasks for feedback

### Step 4: CLI Integration (Optional)
- Add command to trigger the process
- Could be manual (`process-abandoned`) or automatic (scheduled)

### Step 5: Testing
- Test with tasks overdue by various amounts (6, 7, 8 days)
- Test with different priority levels
- Test edge cases (no due date, already DONE, etc.)

### Key Considerations
- Maintain consistency with existing patterns (markAsDone, isOverdue)
- Follow error handling patterns from existing code
- Ensure ABANDONED status is handled in statistics and filtering
- Consider backward compatibility with existing data

## Strategies for Approaching Unfamiliar Code

### 1. Start with Structure, Not Details
- **Approach**: Examine directory structure, configuration files, and entry points first
- **Why**: Provides mental map before diving into implementation
- **Tool**: Directory listing, README, build files

### 2. Follow the Data Flow
- **Approach**: Trace how data moves from input to output
- **Why**: Reveals relationships between components
- **Tool**: Start at entry point (main method, API endpoint) and follow calls

### 3. Identify Patterns Before Modifying
- **Approach**: Look for existing patterns (naming conventions, architectural patterns, error handling)
- **Why**: Consistency is key to maintainable code
- **Tool**: Search for similar functionality, review multiple files

### 4. Understand Domain Before Implementation
- **Approach**: Learn business concepts and terminology before writing code
- **Why**: Prevents implementing features that don't align with business needs
- **Tool**: Domain model analysis, glossary creation, entity relationship diagrams

### 5. Use AI Prompts Strategically
- **Approach**: Start broad (structure), then narrow (features), then deep (domain)
- **Why**: Builds understanding incrementally
- **Tool**: Structured prompts that build on previous understanding

### 6. Document as You Learn
- **Approach**: Take notes, create diagrams, write summaries
- **Why**: Reinforces learning and creates reference material
- **Tool**: Markdown files, diagrams, code comments

### 7. Test Understanding Through Small Changes
- **Approach**: Make small, safe modifications to validate understanding
- **Why**: Confirms mental model matches reality
- **Tool**: Add logging, modify output, create test cases

### 8. Ask "Why" Before "How"
- **Approach**: Understand purpose and business rules before implementation details
- **Why**: Prevents solving wrong problems
- **Tool**: Domain understanding prompts, business logic analysis

## Additional Tools and Resources

### Complementary to AI Prompting

1. **Code Navigation Tools**
   - IDE features (Go to Definition, Find Usages, Call Hierarchy)
   - Code search tools (ripgrep, grep, semantic search)

2. **Documentation Generation**
   - Generate diagrams from code structure
   - Create API documentation
   - Build dependency graphs

3. **Testing Tools**
   - Test coverage analysis to find untested code paths
   - Mutation testing to understand edge cases
   - Integration tests to understand system behavior

4. **Version Control Analysis**
   - Git history to understand evolution
   - Blame annotations to find original authors
   - Commit messages for context

5. **Static Analysis**
   - Linters to understand code style
   - Complexity analysis to find complex areas
   - Dependency analysis to understand coupling

6. **Runtime Analysis**
   - Debugging to trace execution
   - Logging to understand behavior
   - Profiling to understand performance characteristics

## Conclusion

The AI prompting approach provided a structured way to explore an unfamiliar codebase, but it works best when combined with:
- Traditional code reading and navigation
- Testing and experimentation
- Documentation review
- Team communication

The key is using AI prompts to guide exploration rather than replace it, building understanding incrementally from structure → features → domain → implementation details.

