Exercise 1: Code Readability Improvement (Java)
Original Issues Identified:
- Class names (UserMgr, U) are cryptic and don’t follow Java naming conventions.
- Method names (a, f) are unclear and don’t describe their purpose.
- Variable names (un, pw, em) are abbreviated and hard to read.
- SQL query concatenation is unsafe (SQL injection risk).
AI Suggestions (Readability):
- Rename UserMgr → UserManager, U → User.
- Rename methods: a → addUser, f → findUserByUsername.
- Use descriptive variable names: username, password, email.
- Use prepared statements instead of string concatenation for SQL.
Insights:
- I initially noticed the unclear method names, but the AI also flagged SQL injection risk, which I hadn’t considered in a “readability” exercise.
- Readability improvements often overlap with security and maintainability.

 Exercise 2: Function Refactoring (Python)
Original Issues Identified:
- process_orders is too long and does too many things (validation, pricing, shipping, tax, inventory updates).
- Hard to test individual parts because everything is bundled.
AI Suggestions (Refactoring):
- Break into smaller functions:
- validate_order(order, inventory, customer_data)
- calculate_price(order, inventory, customer_data)
- calculate_shipping(price, customer_data)
- calculate_tax(price)
- update_inventory(item_id, quantity)
- Keep process_orders as an orchestrator that calls these helpers.
Insights:
- I thought about splitting validation and pricing, but the AI also suggested separating shipping and tax, which makes testing easier.
- Smaller functions improve readability, testability, and reusability.

 Exercise 3: Code Duplication Detection (JavaScript)
Original Issues Identified:
- Repeated loops for calculating averages and highest values (age, income, score).
- Same logic duplicated three times.
AI Suggestions (Duplication Removal):
- Create a helper function:
function calculateStats(userData, key) {
  const total = userData.reduce((sum, user) => sum + user[key], 0);
  const average = total / userData.length;
  const highest = Math.max(...userData.map(user => user[key]));
  return { average, highest };
}
- Then call:
return {
  age: calculateStats(userData, 'age'),
  income: calculateStats(userData, 'income'),
  score: calculateStats(userData, 'score')
};


Insights:
- I noticed duplication but thought of copy-pasting with minor tweaks. The AI suggested a generic helper, which is cleaner and more maintainable.
- For junior developers, readability of helper functions is key—sometimes explicit duplication is easier to understand, but here the helper is simple enough.

 Reflection
- Most useful prompting strategy: Code duplication detection, because it highlighted a clean abstraction I hadn’t considered.
- AI suggestions I hadn’t thought of: SQL injection risk in Java example, splitting shipping/tax in Python refactor.
- Suggestions I disagreed with: In JavaScript, I’d keep explicit loops if the team is very junior, but the helper function is still a good long-term choice.
- Adapting prompts: I’d tailor them to my stack (React, Node, Prisma) and ask AI to suggest idiomatic patterns.
- Safeguards before production: Always run tests, code reviews, and static analysis before applying AI-suggested refactors.

