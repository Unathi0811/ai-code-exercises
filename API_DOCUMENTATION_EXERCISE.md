# Exercise: API Documentation

## Exercise Overview

In this exercise, you'll practice using AI to create comprehensive API documentation that helps other developers understand and use your APIs effectively. You'll apply different prompt strategies to document endpoints, create examples, and explain authentication and error handling.

## Exercise Options

You have three options for this exercise:

1. **Document your own API**: Select an endpoint from an API you're working on
2. **Document a JavaScript/Express API example**: Use the provided Express.js example
3. **Document a Python/Flask API example**: Use the provided Flask example

## Instructions

1. Select the API endpoint you want to document
2. Apply Prompt 1 to generate comprehensive endpoint documentation
3. Review the documentation for completeness and accuracy
4. Apply Prompt 2 to convert the documentation to a different format (e.g., Markdown to OpenAPI)
5. Apply Prompt 3 to create a developer-friendly usage guide for the endpoint
6. Compare your documentation with a partner and provide feedback

**Note:** You do NOT have to actually run the exercise code or create a runnable project from it. The exercise is only about using extracts from the code to create the documentation.

---

## Prompt 1: Endpoint Documentation Generation

### Prompt Template

```
Please create comprehensive documentation for this API endpoint:

Endpoint: [HTTP method] [endpoint path]

Implementation code:

```[language]
[Paste endpoint implementation code]
```

Please include:
1. A clear description of the endpoint's purpose
2. Request parameters (path, query, body) with types and descriptions
3. Response format with status codes and examples
4. Authentication requirements
5. Potential error responses with codes and messages
6. At least 2 example requests with their responses
7. Any rate limiting or special considerations
```

### Example: Task Manager API Endpoint

**Endpoint to Document:**
- **Method**: POST
- **Path**: `/api/tasks`
- **Purpose**: Create a new task

**Implementation Code:**
```java
@PostMapping("/api/tasks")
public ResponseEntity<TaskResponse> createTask(@RequestBody TaskCreateRequest request) {
    Task task = taskManager.createTask(
        request.getTitle(),
        request.getDescription(),
        request.getPriority(),
        request.getDueDate(),
        request.getTags()
    );
    return ResponseEntity.status(HttpStatus.CREATED)
        .body(new TaskResponse(task));
}
```

### Generated Documentation (Example)

**POST /api/tasks**

Creates a new task in the task management system.

**Request Body:**
```json
{
  "title": "Complete project documentation",
  "description": "Write comprehensive API documentation",
  "priority": 3,
  "dueDate": "2025-12-31",
  "tags": ["documentation", "api"]
}
```

**Response:**
- **201 Created**: Task successfully created
- **400 Bad Request**: Invalid request data
- **401 Unauthorized**: Missing or invalid authentication

---

## Prompt 2: API Reference Conversion

### Prompt Template

```
Please convert this API information into a structured [Swagger/OpenAPI/API Blueprint/Markdown] document:

API details:
[Paste API information, can be informal description]

I need the documentation to include:
1. Endpoint definitions with paths and methods
2. Request parameters and body schemas
3. Response schemas with status codes
4. Example requests and responses
5. Error handling information

Format the output as a complete, valid [Swagger/OpenAPI/etc.] document.
```

---

## Prompt 3: API Usage Guide Creation

### Prompt Template

```
Please create a developer guide for using this API endpoint. The guide should explain how to:

1. Authenticate with the API
2. Properly format requests
3. Handle and interpret responses
4. Deal with common errors
5. Include example code in [language] for making requests

API information:
[Paste endpoint documentation]

Target audience: [Describe experience level and background of users]
Tone: [Friendly/formal/technical/etc.]
```

---

## Reflection Questions

After completing the exercise, consider:

1. **Which parts of the API were most challenging to document?**
2. **How did you adjust your prompts to get better results?**
3. **Which documentation format did you find most effective for your API?**
4. **How would you incorporate this approach into your development workflow?**

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

