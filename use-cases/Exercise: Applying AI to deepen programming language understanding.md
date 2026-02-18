Activity 1: Idiomatic Code Transformation
Objective: Make code more idiomatic for your chosen language.
Example (JavaScript):
// Original
function sumArray(arr) {
  let total = 0;
  for (let i = 0; i < arr.length; i++) {
    total += arr[i];
  }
  return total;
}

// Idiomatic
function sumArray(arr) {
  return arr.reduce((total, num) => total + num, 0);
}


Key Learnings:
- Idiomatic code often leverages built-in language features (reduce in JS, list comprehensions in Python, streams in Java).
- Clearer, shorter code improves readability without sacrificing functionality.
- Understanding idioms helps you write code that feels “native” to the language, making collaboration easier.

🟩 Activity 2: Code Quality Detective
Objective: Spot code smells and improve maintainability.
Example (Python):
# Original
def get_user_info(user):
    if user['age'] > 18:
        if user['active']:
            return "Adult Active"
        else:
            return "Adult Inactive"
    else:
        if user['active']:
            return "Minor Active"
        else:
            return "Minor Inactive"

# Improved
def get_user_info(user):
    status = "Active" if user['active'] else "Inactive"
    age_group = "Adult" if user['age'] > 18 else "Minor"
    return f"{age_group} {status}"


Checklist of Issues Identified:
- Nested conditionals → harder to read.
- Repeated return strings → duplication.
- No clear separation of concerns.
Key Learnings:
- Simplify nested logic with expressions or helper variables.
- Reduce duplication by combining repeated patterns.
- Readability and maintainability improve when logic is expressed clearly.

🟨 Activity 3: Understanding Language Feature
Objective: Learn an advanced feature (e.g., Python decorators).
Explanation:
- Decorators allow you to wrap functions with additional behavior.
- Example:
def log_calls(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log_calls
def greet(name):
    return f"Hello, {name}"


Use Cases:
- Logging function calls.
- Access control (authentication).
- Performance measurement (timing).
Project Idea: Build a small Flask app with decorators for logging and authentication.
Key Learnings:
- Decorators add reusable behavior without modifying core logic.
- They’re powerful for cross-cutting concerns like logging or caching.
- Common mistake: forgetting to preserve function metadata (use functools.wraps).

✨ Group Discussion Themes
- Idiomatic patterns make code concise and expressive.
- Code quality improvements often revolve around reducing duplication and clarifying logic.
- Advanced features (like decorators, generators, streams) help abstract common tasks elegantly.
- Common theme: readability and maintainability are the biggest wins across all exercises.
