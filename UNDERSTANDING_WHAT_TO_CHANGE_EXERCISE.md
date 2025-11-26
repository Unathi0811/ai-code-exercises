# Exercise: Understanding What to Change with AI

## Exercise Overview

This exercise helps you practice using AI to analyze and improve code. You'll work with three different code examples in Java, Python, and JavaScript, applying prompting strategies to make the code better.

## Setup

1. Open your preferred AI assistant (like Claude, ChatGPT, or GitHub Copilot)
2. Copy the prompt template and code example for each exercise
3. Follow the instructions to analyze and improve the code
4. Compare your AI-assisted solution with the sample solution provided
5. Reflect on what you learned and how you might apply it to your own code

**Note:** You do NOT have to have runnable code or tests for this exercise - rather compare solutions and document your learnings and insights to share with the group.

---

## Exercise 1: Code Readability Improvement (Java)

### The Code

```java
class UserMgr {
    private List<U> u_list;
    private DBConn db;

    public UserMgr(DBConn d) {
        db = d;
        u_list = new ArrayList<>();
    }

    public boolean a(String un, String pw, String em) {
        if (un.length() < 3 || pw.length() < 8 || !em.contains("@")) {
            return false;
        }

        for (U user : u_list) {
            if (user.getUn().equals(un)) {
                return false;
            }
        }

        U nu = new U(un, pw, em);
        u_list.add(nu);
        boolean res = db.execute("INSERT INTO users VALUES ('" + un + "', '" + pw + "', '" + em + "')");
        return res;
    }

    public U f(String un) {
        for (U user : u_list) {
            if (user.getUn().equals(un)) {
                return user;
            }
        }
        return null;
    }
}

class U {
    private String un;
    private String pw;
    private String em;

    public U(String un, String pw, String em) {
        this.un = un;
        this.pw = pw;
        this.em = em;
    }

    public String getUn() { return un; }
    public String getPw() { return pw; }
    public String getEm() { return em; }
}
```

### Instructions

1. Use the "Code Readability Improvement" prompt template
2. Specify that this is Java code with the standard Java naming conventions
3. Analyze the AI's suggestions for improving variable names, method names, and code structure
4. Note which readability issues the AI identified that you might have missed

### Prompt Used

```
I want to make this code more readable and maintainable. Please help me by:

1. Identifying parts that are difficult to understand
2. Suggesting better variable and function names
3. Recommending ways to break down complex sections
4. Pointing out any inconsistent style or formatting issues

[Paste code here]

Language/Framework: Java
Team coding standards: camelCase for variables and methods, PascalCase for classes, max line length 100 chars
```

### AI Analysis Results

**Issues Identified:**
1. **Cryptic class names**: `UserMgr` → `UserManager`, `U` → `User`
2. **Abbreviated variable names**: `u_list` → `users`, `un` → `username`, `pw` → `password`, `em` → `email`
3. **Single-letter method names**: `a()` → `addUser()`, `f()` → `findUser()`
4. **SQL injection vulnerability**: String concatenation in SQL query
5. **Missing validation**: Email validation is too simplistic
6. **No documentation**: Missing JavaDoc comments

### Refactored Code

```java
/**
 * Manages user operations including adding and finding users.
 */
public class UserManager {
    private List<User> users;
    private DatabaseConnection database;

    public UserManager(DatabaseConnection database) {
        this.database = database;
        this.users = new ArrayList<>();
    }

    /**
     * Adds a new user to the system after validation.
     * 
     * @param username The username (must be at least 3 characters)
     * @param password The password (must be at least 8 characters)
     * @param email The email address (must contain @ symbol)
     * @return true if user was added successfully, false otherwise
     */
    public boolean addUser(String username, String password, String email) {
        if (!isValidUsername(username) || !isValidPassword(password) || !isValidEmail(email)) {
            return false;
        }

        if (isUsernameTaken(username)) {
            return false;
        }

        User newUser = new User(username, password, email);
        users.add(newUser);
        
        // Use prepared statement to prevent SQL injection
        String sql = "INSERT INTO users (username, password, email) VALUES (?, ?, ?)";
        boolean result = database.execute(sql, username, password, email);
        
        return result;
    }

    /**
     * Finds a user by username.
     * 
     * @param username The username to search for
     * @return The User object if found, null otherwise
     */
    public User findUser(String username) {
        for (User user : users) {
            if (user.getUsername().equals(username)) {
                return user;
            }
        }
        return null;
    }

    private boolean isValidUsername(String username) {
        return username != null && username.length() >= 3;
    }

    private boolean isValidPassword(String password) {
        return password != null && password.length() >= 8;
    }

    private boolean isValidEmail(String email) {
        return email != null && email.contains("@");
    }

    private boolean isUsernameTaken(String username) {
        return findUser(username) != null;
    }
}

/**
 * Represents a user in the system.
 */
public class User {
    private String username;
    private String password;
    private String email;

    public User(String username, String password, String email) {
        this.username = username;
        this.password = password;
        this.email = email;
    }

    public String getUsername() { 
        return username; 
    }

    public String getPassword() { 
        return password; 
    }

    public String getEmail() { 
        return email; 
    }
}
```

### Key Learnings

1. **Naming conventions matter**: Clear, descriptive names make code self-documenting
2. **Security is critical**: SQL injection vulnerabilities are serious issues
3. **Extract validation logic**: Separate validation into helper methods improves readability
4. **Documentation helps**: JavaDoc comments explain purpose and parameters

---

## Exercise 2: Function Refactoring (Python)

### The Code

```python
def process_orders(orders, inventory, customer_data):
    results = []
    total_revenue = 0
    error_orders = []

    for order in orders:
        # Check if item is in inventory
        item_id = order['item_id']
        quantity = order['quantity']
        customer_id = order['customer_id']

        if item_id not in inventory:
            error_orders.append({'order_id': order['order_id'], 'error': 'Item not in inventory'})
            continue

        # Check if enough quantity available
        if inventory[item_id]['quantity'] < quantity:
            error_orders.append({'order_id': order['order_id'], 'error': 'Insufficient quantity'})
            continue

        # Check if customer exists
        if customer_id not in customer_data:
            error_orders.append({'order_id': order['order_id'], 'error': 'Customer not found'})
            continue

        # Calculate price
        price = inventory[item_id]['price'] * quantity

        # Apply discount if customer is premium
        if customer_data[customer_id]['premium']:
            price = price * 0.9

        # Update inventory
        inventory[item_id]['quantity'] -= quantity

        # Calculate shipping based on customer location
        shipping = 0
        if customer_data[customer_id]['location'] == 'domestic':
            if price < 50:
                shipping = 5.99
        else:
            shipping = 15.99

        # Add tax
        tax = price * 0.08

        # Calculate final price
        final_price = price + shipping + tax

        # Update total revenue
        total_revenue += final_price

        # Create result
        result = {
            'order_id': order['order_id'],
            'item_id': item_id,
            'quantity': quantity,
            'customer_id': customer_id,
            'price': price,
            'shipping': shipping,
            'tax': tax,
            'final_price': final_price
        }
        results.append(result)

    return {
        'processed_orders': results,
        'error_orders': error_orders,
        'total_revenue': total_revenue
    }
```

### Instructions

1. Use the "Function Refactoring" prompt template
2. Specify that this function should process orders, update inventory, and track revenue
3. Identify how the function can be broken down into smaller, more focused functions
4. Compare your AI-generated refactoring with your own ideas

### Prompt Used

```
I have a function that I think is doing too much. Please help me refactor it by:

1. Identifying the different responsibilities this function has
2. Suggesting how to break it into smaller, focused functions
3. Improving the organization and flow of the code
4. Pointing out any other issues you notice

[Paste function here]

What this function should do: Process orders, validate inventory, calculate prices, apply discounts, calculate shipping and tax, update inventory, and track revenue
```

### AI Analysis Results

**Responsibilities Identified:**
1. Order validation (inventory check, customer check)
2. Price calculation (base price, discount, shipping, tax)
3. Inventory management (updating quantities)
4. Revenue tracking
5. Error handling and reporting

### Refactored Code

```python
def process_orders(orders, inventory, customer_data):
    """Process a list of orders and return results with revenue tracking."""
    results = []
    error_orders = []
    total_revenue = 0

    for order in orders:
        validation_result = validate_order(order, inventory, customer_data)
        
        if not validation_result['is_valid']:
            error_orders.append({
                'order_id': order['order_id'],
                'error': validation_result['error']
            })
            continue

        order_result = process_valid_order(order, inventory, customer_data)
        results.append(order_result)
        total_revenue += order_result['final_price']

    return {
        'processed_orders': results,
        'error_orders': error_orders,
        'total_revenue': total_revenue
    }


def validate_order(order, inventory, customer_data):
    """Validate that an order can be processed."""
    item_id = order['item_id']
    customer_id = order['customer_id']

    if item_id not in inventory:
        return {'is_valid': False, 'error': 'Item not in inventory'}

    if inventory[item_id]['quantity'] < order['quantity']:
        return {'is_valid': False, 'error': 'Insufficient quantity'}

    if customer_id not in customer_data:
        return {'is_valid': False, 'error': 'Customer not found'}

    return {'is_valid': True}


def process_valid_order(order, inventory, customer_data):
    """Process a validated order and return the result."""
    item_id = order['item_id']
    quantity = order['quantity']
    customer_id = order['customer_id']

    # Calculate base price
    base_price = calculate_base_price(inventory[item_id], quantity)
    
    # Apply discounts
    discounted_price = apply_discount(base_price, customer_data[customer_id])
    
    # Calculate shipping
    shipping = calculate_shipping(discounted_price, customer_data[customer_id])
    
    # Calculate tax
    tax = calculate_tax(discounted_price)
    
    # Calculate final price
    final_price = discounted_price + shipping + tax

    # Update inventory
    update_inventory(inventory, item_id, quantity)

    return {
        'order_id': order['order_id'],
        'item_id': item_id,
        'quantity': quantity,
        'customer_id': customer_id,
        'price': discounted_price,
        'shipping': shipping,
        'tax': tax,
        'final_price': final_price
    }


def calculate_base_price(item, quantity):
    """Calculate the base price for an item and quantity."""
    return item['price'] * quantity


def apply_discount(price, customer):
    """Apply customer discount if applicable."""
    if customer.get('premium', False):
        return price * 0.9
    return price


def calculate_shipping(price, customer):
    """Calculate shipping cost based on customer location and order value."""
    location = customer.get('location', 'international')
    
    if location == 'domestic':
        return 5.99 if price < 50 else 0
    else:
        return 15.99


def calculate_tax(price):
    """Calculate tax on the order."""
    return price * 0.08


def update_inventory(inventory, item_id, quantity):
    """Update inventory after processing an order."""
    inventory[item_id]['quantity'] -= quantity
```

### Key Learnings

1. **Single Responsibility Principle**: Each function now has one clear purpose
2. **Improved testability**: Smaller functions are easier to test individually
3. **Better readability**: Function names describe what they do
4. **Easier maintenance**: Changes to shipping logic don't affect validation logic

---

## Exercise 3: Code Duplication Detection (JavaScript)

### The Code

```javascript
function calculateUserStatistics(userData) {
  // Calculate average age
  let totalAge = 0;
  for (let i = 0; i < userData.length; i++) {
    totalAge += userData[i].age;
  }
  const averageAge = totalAge / userData.length;

  // Calculate average income
  let totalIncome = 0;
  for (let i = 0; i < userData.length; i++) {
    totalIncome += userData[i].income;
  }
  const averageIncome = totalIncome / userData.length;

  // Calculate average score
  let totalScore = 0;
  for (let i = 0; i < userData.length; i++) {
    totalScore += userData[i].score;
  }
  const averageScore = totalScore / userData.length;

  // Find highest age
  let highestAge = userData[0].age;
  for (let i = 1; i < userData.length; i++) {
    if (userData[i].age > highestAge) {
      highestAge = userData[i].age;
    }
  }

  // Find highest income
  let highestIncome = userData[0].income;
  for (let i = 1; i < userData.length; i++) {
    if (userData[i].income > highestIncome) {
      highestIncome = userData[i].income;
    }
  }

  // Find highest score
  let highestScore = userData[0].score;
  for (let i = 1; i < userData.length; i++) {
    if (userData[i].score > highestScore) {
      highestScore = userData[i].score;
    }
  }

  return {
    age: {
      average: averageAge,
      highest: highestAge
    },
    income: {
      average: averageIncome,
      highest: highestIncome
    },
    score: {
      average: averageScore,
      highest: highestScore
    }
  };
}
```

### Instructions

1. Use the "Code Duplication Detection" prompt template
2. Identify the repeated patterns in the code
3. See how the AI suggests consolidating these patterns
4. Evaluate which approach might be most readable for a team of junior developers

### Prompt Used

```
I suspect there might be repeated patterns in this code that could be consolidated. Please help me by:

1. Identifying similar code segments that appear multiple times
2. Suggesting ways to eliminate the duplication (e.g., helper functions, loops)
3. Showing me what the refactored code could look like
4. Explaining the benefits of the suggested changes

[Paste code with potential duplication]
```

### AI Analysis Results

**Duplication Patterns Identified:**
1. **Average calculation**: Repeated 3 times with different fields
2. **Maximum value finding**: Repeated 3 times with different fields
3. **Loop structure**: Same loop pattern used 6 times

### Refactored Code

```javascript
function calculateUserStatistics(userData) {
  if (!userData || userData.length === 0) {
    return {
      age: { average: 0, highest: 0 },
      income: { average: 0, highest: 0 },
      score: { average: 0, highest: 0 }
    };
  }

  const statistics = {
    age: calculateStatistics(userData, 'age'),
    income: calculateStatistics(userData, 'income'),
    score: calculateStatistics(userData, 'score')
  };

  return statistics;
}

function calculateStatistics(userData, field) {
  const values = userData.map(user => user[field]);
  const average = calculateAverage(values);
  const highest = Math.max(...values);
  
  return { average, highest };
}

function calculateAverage(values) {
  const sum = values.reduce((total, value) => total + value, 0);
  return sum / values.length;
}
```

### Alternative Approach (More Explicit)

```javascript
function calculateUserStatistics(userData) {
  if (!userData || userData.length === 0) {
    return createEmptyStatistics();
  }

  return {
    age: calculateFieldStatistics(userData, 'age'),
    income: calculateFieldStatistics(userData, 'income'),
    score: calculateFieldStatistics(userData, 'score')
  };
}

function calculateFieldStatistics(userData, fieldName) {
  const fieldValues = userData.map(user => user[fieldName]);
  
  return {
    average: calculateAverage(fieldValues),
    highest: findMaximum(fieldValues)
  };
}

function calculateAverage(values) {
  if (values.length === 0) return 0;
  const sum = values.reduce((acc, val) => acc + val, 0);
  return sum / values.length;
}

function findMaximum(values) {
  return Math.max(...values);
}

function createEmptyStatistics() {
  return {
    age: { average: 0, highest: 0 },
    income: { average: 0, highest: 0 },
    score: { average: 0, highest: 0 }
  };
}
```

### Key Learnings

1. **DRY Principle**: Don't Repeat Yourself - reduces bugs and maintenance burden
2. **Array methods**: Modern JavaScript provides powerful array methods (map, reduce, Math.max)
3. **Edge case handling**: Empty arrays need special handling
4. **Readability vs. conciseness**: Balance between compact code and clarity

---

## Reflection Questions

### Which prompting strategy did you find most useful? Why?

**Answer:** The "Code Duplication Detection" prompt was most useful because it helped identify patterns I didn't immediately see. The AI pointed out that the same loop structure was repeated 6 times, which made the refactoring benefits very clear.

### What kinds of improvements did the AI suggest that you might not have thought of?

**Answer:**
1. Using array methods (map, reduce) instead of traditional loops
2. Extracting helper functions for common operations
3. Handling edge cases (empty arrays, null checks)
4. Using spread operator with Math.max for finding maximum values

### Were there any suggestions the AI made that you disagreed with? Why?

**Answer:** The AI suggested using arrow functions everywhere, but for a team of junior developers, traditional function declarations might be more readable and easier to debug. However, the functional approach with array methods is more idiomatic JavaScript.

### How might you adapt these prompts for your specific codebase or tech stack?

**Answer:**
1. Add specific framework conventions (e.g., React hooks patterns)
2. Include team-specific style guide references
3. Mention performance requirements if relevant
4. Reference existing code patterns in the codebase

### What safeguards would you put in place before applying AI-suggested refactoring to production code?

**Answer:**
1. **Comprehensive tests**: Ensure all existing tests pass
2. **Code review**: Have a senior developer review the changes
3. **Incremental changes**: Refactor one function at a time
4. **Performance testing**: Verify no performance regressions
5. **Documentation**: Update any affected documentation
6. **Version control**: Use feature branches and small commits

---

## Next Steps

1. Try these prompts on code from your own projects
2. Create a personal template library with these and other useful prompts
3. Practice explaining the reasoning behind refactoring decisions to build your communication skills
4. Remember to always have tests in place before refactoring real code

---

## Summary

This exercise demonstrated how AI can help identify code quality issues and suggest improvements. The three exercises covered:

1. **Readability**: Improving naming, structure, and documentation
2. **Function organization**: Breaking down complex functions into smaller, focused ones
3. **Code duplication**: Identifying and eliminating repeated patterns

Each exercise provided valuable insights into writing better code and using AI as a learning tool rather than just a code generator.

