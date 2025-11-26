# Exercise: Learning a New Programming Language with AI

## Exercise Overview

This exercise guides you through applying structured prompting techniques to learn a new programming language. You'll practice creating effective prompts, verifying your understanding, and building a small project in your target language.

## Prerequisites

- A language you're already comfortable with (your "source language")
- A target language you want to learn
- Access to an AI assistant (like Claude, ChatGPT, etc.)
- Development environment set up for your target language

## Exercise Structure

This exercise is broken into parts that align with the prompting strategies:

1. Create your learning journey plan
2. Apply the four-step prompting strategy
3. Use advanced prompting techniques
4. Build a simple project in your new language

---

## Part 1: Create Your Learning Journey Plan

### Step 1: Define Your Goals

Write 2-3 specific learning goals for your target language.

**Example Goals:**
- "Build a REST API in Go"
- "Create data visualizations in Julia"
- "Develop a mobile app in Swift"

**Your Goals:**
1. [Write your first goal]
2. [Write your second goal]
3. [Write your third goal]

### Step 2: Create a Learning Plan

Use AI to help create a structured learning plan.

**Prompt Template:**
```
I'm proficient in [YOUR SOURCE LANGUAGE] and want to learn [TARGET LANGUAGE] to [YOUR GOAL].

Could you help me create a structured learning journey plan with:
- 3-4 distinct learning phases
- Prerequisites for each phase
- 4-5 specific learning steps per phase
- Verification activities for each phase

Please structure it similar to this Java learning plan example:

[PASTE THE JAVA LEARNING PLAN FROM THE WORKSHOP]
```

**Example Learning Plan (Python to Java):**

**Phase 1: Language Fundamentals**
- Prerequisite: Basic programming knowledge in Python
- Learning Steps:
  1. Java syntax basics and differences from Python
  2. Understanding static typing vs. dynamic typing
  3. Java variable declarations and primitive types
  4. Control structures and how they differ from Python
  5. Verification: Write a simple program converting a Python script to Java

**Phase 2: Object-Oriented Programming in Java**
- Prerequisite: Understand Java syntax basics
- Learning Steps:
  1. Class structure and declaration
  2. Constructors, methods, and fields
  3. Access modifiers (public, private, protected)
  4. Inheritance and interfaces
  5. Verification: Design a class hierarchy implementing a simple concept

**Phase 3: Java-Specific Features**
- Prerequisite: OOP understanding in Java
- Learning Steps:
  1. Collections Framework (vs. Python collections)
  2. Exception handling
  3. Generics
  4. Lambda expressions and functional interfaces
  5. Verification: Refactor code to use appropriate collections and lambda expressions

**Phase 4: Java Ecosystem and Tooling**
- Prerequisite: Core Java knowledge
- Learning Steps:
  1. Build tools (Maven/Gradle)
  2. Testing frameworks (JUnit)
  3. Java standard libraries
  4. Common third-party libraries
  5. Verification: Create a project with proper structure, dependencies, and tests

### Step 3: Refine Your Plan

Review the AI-generated plan and modify it to better match your specific needs and interests.

**Reflection Questions:**
- Does the plan align with your goals?
- Are there topics you want to prioritize differently?
- What adjustments would make it more suitable for your learning style?

---

## Part 2: Apply the Four-Step Prompting Strategy

Choose a specific topic from your learning plan to explore using the four-step prompting strategy.

### Step 1: Conceptual Understanding

**Prompt Template:**
```
I'm currently proficient in [SOURCE LANGUAGE] and want to learn [TARGET LANGUAGE].
Before diving into code:

1. What are the key philosophical differences between [SOURCE] and [TARGET]?
2. What problems was [TARGET] designed to solve?
3. What mental models should I adjust coming from [SOURCE]?
4. What are common misconceptions [SOURCE] developers have about [TARGET]?
```

**Example Response Summary:**

**Key Philosophical Differences:**
- Python: Dynamic typing, interpreted, emphasizes readability
- Java: Static typing, compiled, emphasizes type safety

**Problems Java Solves:**
- Platform independence through JVM
- Enterprise-scale application development
- Strong type safety for large codebases

**Mental Model Adjustments:**
- Think in terms of classes and objects from the start
- Understand compilation vs. interpretation
- Learn to work with explicit type declarations

**Common Misconceptions:**
- "Java is slower than Python" (not always true for long-running applications)
- "Java is only for enterprise" (used in many contexts)
- "Java is outdated" (actively developed and modern)

### Step 2: Step-by-Step Breakdown

**Prompt Template:**
```
I want to understand [SPECIFIC CONCEPT] in [TARGET LANGUAGE]. Could you break down:

1. How [CONCEPT] is implemented in [TARGET LANGUAGE]
2. How it compares to [SIMILAR CONCEPT] in [SOURCE LANGUAGE]
3. The key syntax and structures I need to understand
4. Common patterns and best practices

Let's not write complex code yet, just focus on structure and concepts.
```

**Example: Understanding Java Classes (Coming from Python)**

**How Classes Work in Java:**
- Every file contains one public class
- Classes must be explicitly declared
- Access modifiers control visibility
- Methods must specify return types

**Comparison to Python:**
- Python: Classes are more flexible, can have multiple per file
- Java: More structured, one public class per file
- Python: Implicit `self` parameter
- Java: Explicit `this` keyword

**Key Syntax:**
```java
public class MyClass {
    private String field;
    
    public MyClass(String field) {
        this.field = field;
    }
    
    public String getField() {
        return field;
    }
}
```

**Best Practices:**
- Use access modifiers appropriately
- Follow naming conventions (PascalCase for classes)
- Encapsulate fields with getters/setters
- Use constructors for initialization

### Step 3: Guided Implementation

**Prompt Template:**
```
I'm ready to implement my first [CONCEPT] in [TARGET LANGUAGE]. Could you guide me
through creating a simple [SPECIFIC EXAMPLE]? Please explain each part of the syntax,
especially the parts that differ from [SOURCE LANGUAGE]'s approach.
```

**Example: Creating a Student Class in Java**

**Guided Implementation:**

```java
// In Java, each class is typically in its own file
// File: Student.java

// 'public' means this class can be accessed from other packages
// 'class' keyword declares a class (similar to Python)
public class Student {
    
    // Private fields - these are like instance variables in Python
    // 'private' means only this class can access them directly
    private String name;
    private int id;
    private double[] grades;
    
    // Constructor - this is like __init__ in Python
    // Notice: no return type, same name as class
    public Student(String name, int id) {
        // 'this' refers to the current object (like 'self' in Python)
        this.name = name;
        this.id = id;
        // Arrays in Java have fixed size (unlike Python lists)
        this.grades = new double[5];
    }
    
    // Method to calculate average
    // 'public' means other classes can call this
    // 'double' is the return type (must specify in Java)
    public double calculateAverage() {
        double sum = 0;
        // Enhanced for loop (like 'for grade in grades' in Python)
        for(double grade : grades) {
            sum += grade;
        }
        // Must handle division by zero explicitly
        if (grades.length == 0) {
            return 0.0;
        }
        return sum / grades.length;
    }
}
```

**Key Differences from Python:**
1. **Type declarations**: Must specify types for parameters and return values
2. **Access modifiers**: `public`, `private`, `protected` control visibility
3. **Array vs. List**: Arrays are fixed-size, use ArrayList for dynamic lists
4. **`this` keyword**: Explicit reference to current object (vs. implicit `self` in Python)

### Step 4: Understanding Verification

**Prompt Template:**
```
I've created this [TARGET LANGUAGE] implementation:

[YOUR CODE HERE]

Could you:
1. Verify if I've followed [TARGET LANGUAGE] best practices?
2. Explain any improvements I should make?
3. Suggest what I should learn next?
4. Point out any [SOURCE LANGUAGE] habits that might be showing in my code?
```

**Example Verification:**

**Your Code:**
```java
public class Student {
    private String name;
    private int id;
    private double[] grades;

    public Student(String name, int id) {
        this.name = name;
        this.id = id;
        this.grades = new double[5];
    }

    public double calculateAverage() {
        double sum = 0;
        for(double grade : grades) {
            sum += grade;
        }
        return sum / grades.length;
    }
}
```

**AI Feedback:**

**✅ What You Did Well:**
- Proper use of access modifiers
- Correct constructor syntax
- Good use of enhanced for loop
- Proper encapsulation with private fields

**🔧 Improvements:**
1. **Handle division by zero**: Add check for empty array
2. **Use ArrayList**: Consider `ArrayList<Double>` for dynamic sizing
3. **Add validation**: Validate constructor parameters
4. **Add getters**: Provide access to fields through methods

**📚 Next Steps:**
- Learn about Java Collections Framework
- Understand exception handling
- Study method overloading
- Explore JavaDoc comments

**🐍 Python Habits to Adjust:**
- You're thinking in terms of dynamic lists (good to learn ArrayList)
- Python's duck typing mindset (Java requires explicit types)
- Consider using `List<Double>` instead of arrays for flexibility

**Improved Version:**
```java
import java.util.ArrayList;
import java.util.List;

public class Student {
    private String name;
    private int id;
    private List<Double> grades;  // More flexible than array

    public Student(String name, int id) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be null or empty");
        }
        if (id <= 0) {
            throw new IllegalArgumentException("ID must be positive");
        }
        this.name = name;
        this.id = id;
        this.grades = new ArrayList<>();
    }

    public double calculateAverage() {
        if (grades.isEmpty()) {
            return 0.0;
        }
        double sum = 0;
        for (double grade : grades) {
            sum += grade;
        }
        return sum / grades.size();
    }
    
    // Getters
    public String getName() {
        return name;
    }
    
    public int getId() {
        return id;
    }
    
    public List<Double> getGrades() {
        return new ArrayList<>(grades);  // Return copy to maintain encapsulation
    }
}
```

---

## Part 3: Practice Advanced Prompting Techniques

Choose at least two of the advanced prompting techniques to practice.

### Technique 1: Using Context Effectively

**Prompt Template:**
```
I'm learning [TARGET LANGUAGE FEATURE] coming from [SOURCE LANGUAGE FEATURE].
Could you explain [TARGET CONCEPT] by comparing it to concepts I'm familiar
with in the [SOURCE LANGUAGE] ecosystem?
```

**Example:**
```
I'm learning Java streams coming from Python's pandas operations.
Could you explain Java streams by comparing them to pandas operations I'm familiar with?
```

**AI Response Summary:**
- **Java Streams** are like pandas operations but for collections
- `filter()` is similar to boolean indexing in pandas
- `map()` is like applying a function to a Series
- `collect()` is like converting back to a list/array
- Streams are lazy (like pandas operations) - they don't execute until terminal operation

### Technique 2: Promoting Deep Understanding

**Prompt Template:**
```
I've implemented this solution for [SPECIFIC PROBLEM] in [TARGET LANGUAGE]:

[YOUR CODE]

Could you help me understand:
1. What are the performance implications?
2. What alternative approaches could I have taken in [TARGET LANGUAGE]?
3. How would this need to change if the requirements scaled 10x?
4. How might this implementation be different if I used [ANOTHER FEATURE] instead?
```

### Technique 3: Learning Through Teaching

**Prompt Template:**
```
Could you verify my understanding? Here's how I would explain [TARGET LANGUAGE CONCEPT]
to another developer:

[YOUR EXPLANATION]

What parts of my understanding are correct? What am I missing or misunderstanding?
```

**Example:**
```
Could you verify my understanding? Here's how I would explain Java interfaces:

"An interface in Java is like a contract that a class promises to fulfill.
It defines methods that implementing classes must provide, but doesn't include
the implementation itself. It's somewhat similar to Python's abstract base
classes, but more focused on defining behavior rather than sharing code."

What parts of my understanding are correct? What am I missing or misunderstanding?
```

### Technique 4: Using a Tutor

**Prompt Template:**
```
I'd like you to be my [TARGET LANGUAGE] tutor as I learn it coming from [SOURCE LANGUAGE].
Instead of giving direct answers, please:
- Ask me guiding questions
- Break down complex concepts into smaller pieces
- Give hints when I'm stuck
- Point out conceptual differences from [SOURCE LANGUAGE]
- Correct misunderstandings gently

Let's start with understanding how [TARGET LANGUAGE] handles [SPECIFIC CONCEPT]
differently from [SOURCE LANGUAGE].
```

---

## Part 4: Build a Mini-Project

Now put your learning into practice by building a small project in your target language.

### Step 1: Define Your Project

Choose a simple project that uses the concepts you've learned. Good options include:
- A command-line tool that processes text
- A simple API endpoint
- A data processing script
- A basic UI component (if applicable)

**Your Project Idea:**
[Describe your project]

### Step 2: Plan the Implementation

**Prompt Template:**
```
I want to build a [PROJECT DESCRIPTION] in [TARGET LANGUAGE]. I'm coming from
[SOURCE LANGUAGE] and have been learning [TARGET LANGUAGE] concepts including
[LIST CONCEPTS YOU'VE LEARNED].

Could you help me:
1. Break this project into small, manageable components
2. Suggest what library/framework components I should use
3. Outline the key files/classes I'll need to create
4. Identify potential challenges I might face coming from [SOURCE LANGUAGE]
```

### Step 3: Implement Step by Step

Implement your project step by step, using AI for guidance when needed.

**Tips:**
- Start with the simplest component
- Test each component as you build it
- Ask AI for help when stuck, but try to solve problems yourself first
- Document your code as you go

### Step 4: Review and Refactor

**Prompt Template:**
```
I've completed my [TARGET LANGUAGE] project:

[YOUR CODE OR GITHUB LINK]

Could you:
1. Review the code for [TARGET LANGUAGE] idioms and best practices
2. Suggest refactoring opportunities to make it more idiomatic
3. Identify any remaining [SOURCE LANGUAGE] patterns I should adjust
4. Recommend next steps to continue improving my [TARGET LANGUAGE] skills
```

---

## Reflection Questions

After completing the exercise, reflect on your experience:

### Which prompting strategies were most effective for your learning style?

**Your Answer:**
[Reflect on which strategies helped you learn most effectively]

### What surprised you about the target language that wasn't immediately obvious?

**Your Answer:**
[Note unexpected aspects of the language]

### How did your mental models from your source language help or hinder learning?

**Your Answer:**
[Reflect on how prior knowledge influenced your learning]

### What would you do differently in your next learning session?

**Your Answer:**
[Identify improvements for future learning]

### What gaps remain in your understanding of the target language?

**Your Answer:**
[Note areas that need more practice]

---

## Extension Activities

If you finish early or want to continue your learning:

1. **Create a "cheat sheet"** comparing key concepts between your source and target languages
2. **Expand your mini-project** with additional features
3. **Practice explaining** a complex concept from your target language in your own words
4. **Find an open-source project** in your target language and analyze a small portion of it
5. **Create your own custom prompting template** for learning future languages

---

## Key Learnings Summary

Document your three key learnings from this exercise:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

---

## Conclusion

This exercise demonstrated how to use AI as a learning companion rather than just a code generator. By following structured prompting strategies, you can:

- Build deep understanding of new languages
- Make connections to languages you already know
- Verify your understanding through active practice
- Create meaningful projects that reinforce learning

Remember: **Drive your own learning** - use AI to support your journey, not replace it.

