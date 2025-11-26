# Exercise: Performance Optimization Challenge

## Exercise Instructions

Select one of the following performance scenarios (each designed for a different performance issue):

1. **Slow Code Analysis**: Python data processing function
2. **Memory Usage Optimization**: Java image processing application
3. **Slow Database Query Analysis**: JavaScript/Node.js application with database access

Use the corresponding AI prompt from the "Identifying Performance Bottlenecks" section to analyze the performance issue.

Implement the suggested optimizations, measure performance before and after your changes, and document your optimization process, results, and key learnings.

**Important:** Check out the starter code at the GitLab Repo. For reference the specific code is also listed on the course page.

---

## Sample Performance Issues for Analysis

### 1. Slow Code Analysis (Python)

**Code with Performance Issue:**

```python
def find_product_combinations(products, target_price, price_margin=10):
    """
    Find all pairs of products where the combined price is within
    the target_price ± price_margin range.
    """
    results = []
    
    # For each possible pair of products
    for i in range(len(products)):
        for j in range(len(products)):
            # Skip comparing a product with itself
            if i != j:
                product1 = products[i]
                product2 = products[j]
                
                # Calculate combined price
                combined_price = product1['price'] + product2['price']
                
                # Check if the combined price is within the target range
                if (target_price - price_margin) <= combined_price <= (target_price + price_margin):
                    # Avoid duplicates like (product1, product2) and (product2, product1)
                    if not any(r['product1']['id'] == product2['id'] and
                               r['product2']['id'] == product1['id'] for r in results):
                        pair = {
                            'product1': product1,
                            'product2': product2,
                            'combined_price': combined_price,
                            'price_difference': abs(target_price - combined_price)
                        }
                        results.append(pair)
    
    # Sort by price difference from target
    results.sort(key=lambda x: x['price_difference'])
    return results
```

**Context:**
- This code is used in an e-commerce application to suggest product pairs
- The function typically processes 5,000+ products
- Current execution time: Approximately 20-30 seconds for 5,000 products
- Environment: Python 3.9 on a web server with 4GB RAM

### 2. Memory Usage Optimization (Java)

**Code with Performance Issue:**

```java
public static void processImageFolder(String inputFolder, String outputFolder) throws IOException {
    File[] imageFiles = folder.listFiles(...);
    
    // Store all images in memory first
    List<BufferedImage> images = new ArrayList<>();
    for (File imageFile : imageFiles) {
        BufferedImage image = ImageIO.read(imageFile);
        images.add(image);
    }
    
    // Process all images
    List<BufferedImage> processedImages = new ArrayList<>();
    for (BufferedImage image : images) {
        BufferedImage processed = applyEffects(image);
        processedImages.add(processed);
    }
    
    // Save all processed images
    for (int i = 0; i < imageFiles.length; i++) {
        ImageIO.write(processedImages.get(i), ...);
    }
}
```

**Error Message:**
```
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
```

**Context:**
- Processes a batch of images (applying grayscale effect)
- Error occurs when processing about 50-100 high-resolution images (2000x1500 pixels each)
- Application is running with default JVM memory settings (256MB)
- Environment: Java 11, Windows 10 with 8GB RAM

### 3. Slow Database Query Analysis (JavaScript/Node.js)

**Code with Performance Issue:**

```javascript
async function getCustomerOrderDetails(customerId, startDate, endDate) {
    const result = await pool.query(`
      SELECT
        o.order_id,
        o.order_date,
        o.total_amount,
        o.status,
        c.customer_name,
        c.email,
        (
          SELECT json_agg(...)
          FROM order_items oi
          JOIN products p ON oi.product_id = p.product_id
          WHERE oi.order_id = o.order_id
        ) as items,
        ...
      FROM orders o
      JOIN customers c ON o.customer_id = c.customer_id
      WHERE o.customer_id = $1
        AND o.order_date BETWEEN $2 AND $3
      ORDER BY o.order_date DESC
    `, [customerId, startDate, endDate]);
    
    return result.rows;
}
```

**Context:**
- PostgreSQL 13 database
- orders (~100,000 rows), customers (~10,000 rows), order_items (~500,000 rows)
- The query takes 8-10 seconds to execute for customers with many orders
- No additional indexes beyond primary keys
- Environment: Node.js 14 running on a server with 4 CPU cores and 8GB RAM

---

## Prompt 1: Slow Code Analysis

### Prompt Template

```
I have a piece of code that's running slowly. I'd like to understand why and how to improve it.

Here's the slow-performing code:

[paste your code here]

Context about the issue:
- What this code is supposed to do: [brief explanation of purpose]
- Typical input size/data: [describe the data size, like "processing 10,000 records"]
- Current performance: [describe how slow it is, like "takes about 30 seconds to run"]
- Environment: [language version, framework, hardware limitations if relevant]

Could you please:
1. Explain in simple terms why this code might be slow
2. Identify the specific operations or patterns that are likely causing the slowdown
3. Suggest 2-3 specific improvements I could make
4. Explain the performance concepts I should learn to avoid similar issues in the future
5. If there are any tools or techniques I could use to measure the actual bottlenecks, please suggest them

I'm particularly interested in learning the underlying performance concepts, not just getting a quick fix.
```

---

## Prompt 2: Memory Usage Optimization

### Prompt Template

```
My application is using too much memory and sometimes crashes with out-of-memory errors. I'd like to understand why and how to fix it.

Here's the code that I suspect is causing memory issues:

[paste your code here]

Additional context:
- Application type: [web app, data processing script, etc.]
- When the issue occurs: [e.g., "after processing about 1,000 images"]
- Error message (if any): [paste any error messages]
- Memory limit in my environment: [e.g., "8GB RAM on my development machine"]
- Language and runtime: [e.g., "Python 3.9, Node.js 16, etc."]

Could you please:
1. Explain in simple terms what might be causing the high memory usage
2. Identify specific memory-intensive operations or potential memory leaks
3. Suggest 2-3 practical ways to reduce memory consumption
4. Recommend any tools I could use to profile memory usage
5. Explain the memory management concepts I should understand for my language/environment

I want to both fix this issue and learn how to write more memory-efficient code in the future.
```

---

## Prompt 3: Slow Database Query Analysis

### Prompt Template

```
I have a database query that's running very slowly. I'd like to understand why and how to optimize it.

Here's the slow query:

[paste your query here]

Database context:
- Database type: [MySQL, PostgreSQL, MongoDB, etc.]
- Table sizes: [approximate number of rows in relevant tables]
- Table structure: [key columns and their data types]
- Existing indexes (if known): [any indexes you're aware of]
- When the slowness occurs: [all the time, or only with certain parameters?]
- Approximate execution time: [how long the query takes to run]

Could you please:
1. Explain in simple terms why this query might be slow
2. Identify specific parts of the query that are likely causing performance issues
3. Suggest improvements to make the query faster (both query rewrites and potential indexes)
4. Explain the database concepts I should learn to write better queries
5. Suggest how I could measure the query performance before and after changes

I'm a junior developer, so explanations about the underlying database concepts would be very helpful.
```

---

## Reflection Questions

1. **How did the optimization change your understanding of the algorithm, memory management, or database query patterns?**
2. **What performance improvements did you achieve? Were they significant enough to justify the code changes?**
3. **What did you learn about performance bottlenecks that you didn't know before?**
4. **How would you approach similar performance issues in the future?**
5. **What tools or techniques would you use to identify similar issues proactively?**

---

## Key Learnings

Document your three key learnings:

1. **[Learning 1]**
2. **[Learning 2]**
3. **[Learning 3]**

