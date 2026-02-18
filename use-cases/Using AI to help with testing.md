Part 1: Understanding What to Test
Exercise 1.1: Behavior Analysis
Function under test: calculateTaskScore
Conversation outcomes:
- Behaviors to test:
- Priority weights (LOW, MEDIUM, HIGH, URGENT).
- Due date proximity scoring (overdue, today, within 2 days, within 7 days).
- Status penalties (DONE, REVIEW).
- Tag boosts (blocker, critical, urgent).
- Recently updated tasks (+5 boost).
- Edge cases identified:
- No due date provided.
- Invalid or malformed date.
- Empty tags array.
- Multiple tags including both critical and non-critical.
- Tasks already completed but still overdue.
Test cases list (at least 5):
- Task with HIGH priority and due today → should get base + due date boost.
- Task marked DONE → should have large negative adjustment.
- Task with “blocker” tag → should get tag boost.
- Task updated within last 24 hours → should get recency boost.
- Task with no due date and no tags → should only reflect priority weight.

Exercise 1.2: Test Planning
Functions under test: calculateTaskScore, sortTasksByImportance, getTopPriorityTasks
Testing plan:
- Unit tests:
- Verify calculateTaskScore outputs correct scores for different inputs.
- Check sorting order in sortTasksByImportance.
- Ensure getTopPriorityTasks returns correct number of tasks.
- Integration tests:
- Feed multiple tasks into workflow and confirm top tasks are correct.
Priority of test cases:
- Core scoring logic (unit).
- Sorting correctness (unit).
- Top tasks selection (integration).
- Edge cases (unit).
- Combined workflow (integration).
Dependencies:
- Mock task objects with controlled properties.
- Date manipulation utilities.
Expected outcomes:
- Scores match expected values.
- Sorted list is in descending importance.
- Top tasks list length matches limit.

🧪 Part 2: Improving a Single Test
Exercise 2.1: Writing First Test
Initial simple test:
test('calculateTaskScore returns higher score for HIGH priority than LOW', () => {
  const lowTask = { priority: 'LOW', status: 'PENDING', tags: [], dueDate: null, updatedAt: new Date() };
  const highTask = { priority: 'HIGH', status: 'PENDING', tags: [], dueDate: null, updatedAt: new Date() };
  expect(calculateTaskScore(highTask)).toBeGreaterThan(calculateTaskScore(lowTask));
});


Improved test (after AI feedback):
- Clarify purpose: comparing priority weights.
- Add precise assertions for expected values.
test('HIGH priority task scores 20 points higher than LOW priority task', () => {
  const now = new Date();
  const lowTask = { priority: 'LOW', status: 'PENDING', tags: [], dueDate: null, updatedAt: now };
  const highTask = { priority: 'HIGH', status: 'PENDING', tags: [], dueDate: null, updatedAt: now };
  expect(calculateTaskScore(lowTask)).toBe(10);
  expect(calculateTaskScore(highTask)).toBe(30);
});



Exercise 2.2: Testing Due Date Calculation
Principles of good test:
- Use fixed dates to avoid flakiness.
- Cover overdue, today, near future, and far future.
Example test:
test('Task due tomorrow gets +15 score boost', () => {
  const today = new Date();
  const tomorrow = new Date(today);
  tomorrow.setDate(today.getDate() + 1);

  const task = { priority: 'MEDIUM', status: 'PENDING', tags: [], dueDate: tomorrow, updatedAt: today };
  const score = calculateTaskScore(task);

  expect(score).toBe(20 + 15); // base 20 + due date boost
});



⚙️ Part 3: Test-Driven Development Practice
Exercise 3.1: New Feature (Assigned to current user → +12 boost)
TDD steps:
- Failing test:
test('Task assigned to current user gets +12 boost', () => {
  const task = { priority: 'MEDIUM', status: 'PENDING', tags: [], dueDate: null, updatedAt: new Date(), assignedTo: 'me' };
  const score = calculateTaskScore(task, 'me');
  expect(score).toBe(32); // base 20 + 12 boost
});
- Minimal code: Add check in function:
if (task.assignedTo === currentUser) score += 12;
- Refactor: Ensure consistent parameter handling.
- Next test: Verify tasks assigned to others do not get boost.

Exercise 3.2: Bug Fix (Days since update calculation)
Failing test:
test('Days since update should calculate correctly', () => {
  const today = new Date();
  const yesterday = new Date(today);
  yesterday.setDate(today.getDate() - 1);

  const task = { priority: 'MEDIUM', status: 'PENDING', tags: [], dueDate: null, updatedAt: yesterday };
  const score = calculateTaskScore(task);

  expect(score).toBe(20); // no recency boost, since updated 1 day ago
});


Fix: Correct calculation to use whole days instead of milliseconds division.

🔗 Part 4: Integration Testing
Workflow test:
test('Integration: top priority tasks are correctly selected', () => {
  const today = new Date();
  const tasks = [
    { priority: 'LOW', status: 'PENDING', tags: [], dueDate: null, updatedAt: today },
    { priority: 'HIGH', status: 'PENDING', tags: ['blocker'], dueDate: today, updatedAt: today },
    { priority: 'MEDIUM', status: 'DONE', tags: [], dueDate: today, updatedAt: today }
  ];

  const topTasks = getTopPriorityTasks(tasks, 2);
  expect(topTasks.length).toBe(2);
  expect(topTasks[0].priority).toBe('HIGH');
});



✨ Reflection
- Confidence change: Grew as tests covered more behaviors and edge cases.
- Scrutiny: Due date and recency logic required the most careful checks.
- Most valuable technique: TDD forced clarity on expected behavior before coding.
- Learned: Testing is about verifying behavior, not implementation details.
- Future approach: Start with unit tests, expand to integration, use TDD for new features.
- Tools: Use Jest/Mocha for unit tests, sinon for mocking, and CI pipelines to run tests automatically.
