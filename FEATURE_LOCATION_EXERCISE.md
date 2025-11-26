# Exercise Part 2: Finding Feature Implementation - Task Export to CSV

## Initial Search

### Search Terms Used
- `export`, `Export`, `EXPORT` - No matches found
- `csv`, `CSV`, `Csv` - No matches found
- `FileWriter`, `FileReader`, `write`, `Write`, `save`, `Save` - Found matches in TaskStorage.java
- Searched for file-related functionality and data transformation patterns

### Files Found
1. **TaskStorage.java** (`src/main/java/za/co/wethinkcode/taskmanager/storage/TaskStorage.java`)
   - Contains file I/O operations using `FileWriter` and `FileReader`
   - Handles JSON serialization/deserialization with Gson
   - Has `save()` method that writes tasks to a JSON file
   - Uses custom serializers for LocalDateTime handling

2. **TaskManagerCli.java** (`src/main/java/za/co/wethinkcode/taskmanager/cli/TaskManagerCli.java`)
   - Contains `formatTask()` method that formats Task objects for display
   - Handles command parsing and execution
   - Uses DateTimeFormatter for date formatting

3. **TaskManager.java** (`src/main/java/za/co/wethinkcode/taskmanager/app/TaskManager.java`)
   - Orchestrates business logic
   - Has methods like `getStatistics()` that transform task data
   - Uses TaskStorage to retrieve task lists

4. **Task.java** (`src/main/java/za/co/wethinkcode/taskmanager/model/Task.java`)
   - Domain model with all task properties
   - Has getters for all fields that would need to be exported

### Directories Checked
- `src/main/java/za/co/wethinkcode/taskmanager/storage/` - File I/O operations
- `src/main/java/za/co/wethinkcode/taskmanager/cli/` - Command handling and formatting
- `src/main/java/za/co/wethinkcode/taskmanager/app/` - Business logic
- `src/main/java/za/co/wethinkcode/taskmanager/util/` - Utility functions (TaskTextParser shows data transformation patterns)

## Form a Hypothesis

### Where CSV Export Functionality Might Belong

Based on my search, I believe the CSV export feature should be implemented in the following locations:

1. **Primary Location: `TaskStorage.java` or a new utility class**
   - Since `TaskStorage` already handles file I/O operations (JSON reading/writing), it makes sense to add CSV export capability here
   - Alternatively, create a new class like `TaskExporter.java` in the `util` package to follow separation of concerns (storage vs. export)

2. **Business Logic: `TaskManager.java`**
   - Add a method like `exportTasksToCSV(String filePath)` that orchestrates the export
   - This would retrieve tasks from storage and delegate to the export utility

3. **CLI Integration: `TaskManagerCli.java`**
   - Add a new command handler `handleExportCommand(String[] args)` in the `executeCommand()` switch statement
   - Parse command-line arguments for the export file path
   - Call the TaskManager export method

### Implementation and Components to Be Affected

#### 1. **TaskStorage.java** (No changes needed, but provides foundation)
   - **Current role**: Already handles file I/O operations using `FileWriter` and `FileReader`
   - **Why it's relevant**: It has a `save()` method that writes data to a file, which demonstrates the file writing pattern we'll follow for CSV export
   - **Implementation note**: We'll follow the same try-with-resources pattern used in `TaskStorage.save()`:
     ```java
     try (FileWriter writer = new FileWriter(filePath)) {
         // CSV writing operations
     } catch (IOException e) {
         System.err.println("Error: " + e.getMessage());
     }
     ```
   - **What we'll use**: The existing methods `getAllTasks()`, `getTasksByStatus()`, `getTasksByPriority()`, and `getOverdueTasks()` to retrieve tasks for export

#### 2. **TaskManagerCli.java** (Add export command handler)
   - **Current role**: Handles user commands and formats output for display
   - **Why it's relevant**: CSV export will be a new command, so this is where we add the command handler
   - **Implementation details**:
     - Add "export" case in the `executeCommand()` switch statement (around line 76):
       ```java
       case "export":
           handleExportCommand(args);
           break;
       ```
     - Add `handleExportCommand(String[] args)` method that:
       - Parses command arguments (file path, optional filters like `-s <status>` or `-p <priority>`)
       - Calls `taskManager.exportTasksToCSV()` with appropriate parameters
       - Provides user feedback on success/failure
     - Update `showHelp()` method to include: `export [filepath] [-s <status>] [-p <priority>] - Export tasks to CSV file`
   - **Pattern to follow**: Similar to `handleListCommand()` which also supports filtering options

#### 3. **TaskManager.java** (Add export orchestration method)
   - **Current role**: Orchestrates operations (like getting statistics) and coordinates between CLI and storage
   - **Why it's relevant**: CSV export should be coordinated here, similar to how `getStatistics()` retrieves and processes tasks
   - **Implementation details**:
     - Add method: `exportTasksToCSV(String filePath)` - exports all tasks
     - Add method: `exportTasksToCSV(String filePath, String statusFilter, Integer priorityFilter)` - exports filtered tasks
     - Implementation pattern:
       ```java
       public boolean exportTasksToCSV(String filePath, String statusFilter, Integer priorityFilter) {
           try {
               List<Task> tasks = listTasks(statusFilter, priorityFilter, false);
               TaskExporter.exportTasksToCSV(tasks, filePath);
               return true;
           } catch (IOException e) {
               System.err.println("Error exporting to CSV: " + e.getMessage());
               return false;
           }
       }
       ```
   - **Responsibilities**: Retrieve tasks from storage (with optional filtering), delegate to TaskExporter utility, handle errors

#### 4. **Task.java** (No changes needed, but provides data)
   - **Current role**: Holds all task information (title, description, dates, priority, status, tags, etc.)
   - **Why it's relevant**: CSV export needs to read all these fields using the existing getter methods
   - **What we'll use**: All existing getters:
     - `getId()`, `getTitle()`, `getDescription()`
     - `getPriority()`, `getStatus()`
     - `getCreatedAt()`, `getUpdatedAt()`, `getDueDate()`, `getCompletedAt()`
     - `getTags()`
   - **Implementation note**: The TaskExporter will call these getters to extract data for each CSV row

#### 5. **New File: TaskExporter.java** (in `util/` package)
   - **Purpose**: Handle CSV formatting and file writing logic
   - **Implementation details**:
     - Main method: `exportTasksToCSV(List<Task> tasks, String filePath) throws IOException`
       - Writes CSV header row with column names
       - Iterates through tasks and writes each as a CSV row
       - Uses try-with-resources for file handling
     - Helper method: `formatTaskAsCSVRow(Task task)` - converts a Task to a CSV row string
     - Helper method: `escapeCSVField(String field)` - handles CSV escaping:
       - Wraps fields containing commas, quotes, or newlines in double quotes
       - Escapes internal double quotes by doubling them
       - Handles null values (converts to empty string)
     - Date formatting: Uses `DateTimeFormatter.ISO_DATE` and `ISO_LOCAL_DATE_TIME` for consistency
   - **Pattern**: Similar to how `TaskTextParser` handles data transformation in the util package

#### 6. **README.md** (Update documentation)
   - **Current role**: Documents available commands and usage
   - **Implementation**: Add export command to the "Available commands" section:
     - `export <filepath> [-s <status>] [-p <priority>]` - Export tasks to CSV file
   - **Example**: `./gradlew run --args="export tasks.csv -s todo"`
   - **Document CSV format**: Explain column headers and date formats used

#### 7. **Tests** (Add comprehensive test coverage)
   - **TaskExporterTest.java** (new file):
     - Test CSV formatting with various task data
     - Test CSV escaping (commas, quotes, newlines in fields)
     - Test null value handling
     - Test file writing and file creation
     - Test with empty task list
   - **Update TaskManagerTest.java**:
     - Test `exportTasksToCSV()` method with all tasks
     - Test filtered exports (by status, by priority)
     - Test error handling (invalid file path, write permissions)
     - Mock TaskExporter to test TaskManager logic separately
   - **Update TaskManagerCliTest.java** (if it exists):
     - Test export command parsing
     - Test export command with filters
     - Test error messages for invalid arguments

### Related Patterns Found

- **File I/O Pattern**: TaskStorage uses try-with-resources for FileWriter/FileReader
- **Data Transformation Pattern**: TaskManagerCli.formatTask() shows how to convert Task objects to formatted strings
- **Date Formatting Pattern**: Uses DateTimeFormatter.ISO_DATE and custom patterns
- **Command Pattern**: CLI uses switch statement to route commands to handlers

---

## Apply the Feature Location Prompt

I need to work on the **Task Export to CSV** feature in this codebase, but I'm not sure where the code for this feature lives.

### My approach so far:

- I've searched for keywords like: `export`, `csv`, `FileWriter`, `FileReader`, `write`, `save`, `format`
- I looked in the `storage/` directory which seemed relevant because it contains `TaskStorage.java` with file I/O operations
- I also checked the `cli/` directory for command handling patterns and the `app/` directory for business logic
- I think the feature might relate to:
  - `TaskStorage.java` - since it already handles file operations (JSON read/write)
  - `TaskManagerCli.java` - since it handles command parsing and has a `formatTask()` method for data formatting
  - `TaskManager.java` - since it orchestrates business operations and has methods that work with task lists
  - A new utility class in the `util/` package - following the pattern of other utility classes like `TaskTextParser`

### Project structure:

```
src/main/java/za/co/wethinkcode/taskmanager/
├── app/
│   └── TaskManager.java          (Business logic, orchestrates operations)
├── cli/
│   └── TaskManagerCli.java       (Command-line interface, command handlers)
├── model/
│   ├── Task.java                 (Domain model)
│   ├── TaskPriority.java        (Enum)
│   └── TaskStatus.java           (Enum)
├── storage/
│   └── TaskStorage.java          (File I/O, JSON persistence)
└── util/
    ├── TaskMergeService.java     (Utility for merging tasks)
    ├── TaskPriorityManager.java  (Priority utilities)
    └── TaskTextParser.java       (Text parsing utility)
```

### Based on my search, these files might be relevant, but I'm not sure:

1. **TaskStorage.java** - Has `save()` method using FileWriter, handles file operations, but currently only does JSON. Should CSV export go here or in a separate class?

2. **TaskManagerCli.java** - Has `formatTask()` method that converts Task to formatted string. Should CSV formatting be similar, or is this only for display?

3. **TaskManager.java** - Has `getStatistics()` that transforms task data. Should export be a similar method here?

4. **No existing export functionality** - I found no CSV or export code, so this is a new feature. Where should new export functionality live?

### Can you help me:

1. **Evaluate my search approach and suggest improvements**
   - Did I miss any important areas to search?
   - Are there other patterns I should look for?

2. **Identify which files and directories most likely contain the implementation for this feature**
   - Should CSV export be in TaskStorage, a new utility class, or TaskManager?
   - Where should the CLI command handler go?

3. **Suggest specific search terms or patterns that would be more effective**
   - What other code patterns should I look for?

4. **Explain what parts of the feature might be located in different areas of the codebase**
   - How should the responsibilities be divided between CLI, business logic, and utilities?

5. **Recommend a step-by-step investigation process to understand the complete feature flow**
   - What should I examine next to fully understand how to implement this?

### Also, what questions could I ask myself as I'm exploring the code to ensure I'm on the right track?

- How do other commands (like `list` or `stats`) flow through the system?
- What's the pattern for adding new commands to the CLI?
- How does TaskStorage handle file operations, and should CSV follow the same pattern?
- Are there any existing data transformation utilities I can reuse?

### What specific patterns should I look for to confirm I've found all the relevant parts?

- Command handler pattern in TaskManagerCli
- File I/O pattern in TaskStorage
- Data formatting/transformation patterns
- Error handling patterns for file operations

### After your guidance, could you give me a small challenge to test my understanding of how to navigate this feature's code?

---

## Document Your Findings

### Where to Implement CSV Export Feature

Based on the codebase analysis, here's where I would implement the CSV export feature:

#### 1. **New Utility Class: `TaskExporter.java`** (in `util/` package)
   - **Purpose**: Handle CSV formatting and file writing
   - **Responsibilities**:
     - Convert Task objects to CSV rows
     - Handle CSV escaping (commas, quotes, newlines in fields)
     - Write CSV header row
     - Write task data rows to file
   - **Pattern**: Similar to how `TaskTextParser` handles data transformation
   - **Methods**:
     - `exportTasksToCSV(List<Task> tasks, String filePath) throws IOException`
     - `formatTaskAsCSVRow(Task task)` - private helper
     - `escapeCSVField(String field)` - private helper for escaping

#### 2. **Business Logic: `TaskManager.java`**
   - **Purpose**: Orchestrate the export operation
   - **Modifications**:
     - Add method: `exportTasksToCSV(String filePath)` 
     - Add method: `exportTasksToCSV(String filePath, String statusFilter, Integer priorityFilter)` - for filtered exports
   - **Responsibilities**:
     - Retrieve tasks from storage (with optional filtering)
     - Call TaskExporter utility
     - Handle errors and return success/failure status
   - **Pattern**: Similar to how `getStatistics()` retrieves and processes tasks

#### 3. **CLI Integration: `TaskManagerCli.java`**
   - **Purpose**: Add export command to CLI
   - **Modifications**:
     - Add "export" case in `executeCommand()` switch statement (around line 76)
     - Add `handleExportCommand(String[] args)` method
     - Update `showHelp()` method to include export command documentation
   - **Command Format**: `export [filepath] [-s <status>] [-p <priority>]`
   - **Pattern**: Follows the same pattern as `handleListCommand()` with optional filters

#### 4. **TaskStorage.java** (No changes needed)
   - Already has all necessary methods to retrieve tasks:
     - `getAllTasks()`
     - `getTasksByStatus(TaskStatus status)`
     - `getTasksByPriority(TaskPriority priority)`
     - `getOverdueTasks()`

### Related Components That Would Be Affected

1. **README.md** - Update documentation to include export command usage
2. **Tests** - Would need to add tests for:
   - `TaskExporterTest.java` - Test CSV formatting and file writing
   - Update `TaskManagerTest.java` - Test export methods
   - Update `TaskManagerCliTest.java` (if it exists) - Test export command

### Implementation Plan

#### Step 1: Create TaskExporter Utility
- Create `TaskExporter.java` in `util/` package
- Implement CSV formatting logic
- Handle edge cases (null values, special characters, dates)
- Write unit tests

#### Step 2: Add Export Method to TaskManager
- Add `exportTasksToCSV(String filePath)` method
- Add filtered export variants
- Handle IOException and provide user feedback
- Test with TaskManagerTest

#### Step 3: Add CLI Command Handler
- Add export case to `executeCommand()`
- Implement `handleExportCommand()` with argument parsing
- Support optional filters (status, priority)
- Update help text
- Test command-line usage

#### Step 4: Update Documentation
- Add export command to README.md
- Document CSV format (column headers, date formats, etc.)

### CSV Format Design

Based on the Task model, the CSV should include:
- `id` - Task UUID
- `title` - Task title
- `description` - Task description
- `priority` - Priority value (1-4) or name (LOW, MEDIUM, HIGH, URGENT)
- `status` - Status value (todo, in_progress, review, done)
- `created_at` - ISO date-time format
- `updated_at` - ISO date-time format
- `due_date` - ISO date format (or empty)
- `completed_at` - ISO date-time format (or empty)
- `tags` - Comma-separated list of tags (may need special handling)

### Key Patterns to Follow

1. **File I/O Pattern** (from TaskStorage):
   ```java
   try (FileWriter writer = new FileWriter(filePath)) {
       // write operations
   } catch (IOException e) {
       System.err.println("Error: " + e.getMessage());
   }
   ```

2. **Command Handler Pattern** (from TaskManagerCli):
   ```java
   case "export":
       handleExportCommand(args);
       break;
   ```

3. **Data Retrieval Pattern** (from TaskManager):
   ```java
   List<Task> tasks = getStorage().getAllTasks();
   // process tasks
   ```

4. **Error Handling Pattern**: Follow the pattern used in TaskManager methods that return boolean or handle errors via System.err.println

