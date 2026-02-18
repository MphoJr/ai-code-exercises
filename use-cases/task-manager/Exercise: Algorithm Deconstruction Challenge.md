Algorithm 1: Task Priority Sorting Algorithm
How did the AI’s explanation change your understanding of the algorithm?
Initially, I thought the algorithm was a simple priority-based sort. After the AI explanation, I realized it’s a multi-factor scoring system:
- Priority weight is the baseline.
- Due date proximity adds urgency.
- Status (done/review) reduces importance.
- Tags and recent updates boost relevance.
This clarified that the algorithm balances urgency, importance, and recency rather than just sorting by priority.

2. What aspects were still difficult to understand after AI explanation?
- The weighting choices (e.g., why overdue adds +30, done subtracts -50) feel somewhat arbitrary.
- How these scores interact in edge cases (e.g., a high-priority task that’s overdue but also marked “review”).
- Whether the scoring system was tuned empirically or just chosen heuristically.

3. How would you explain this algorithm to another junior developer?
I’d say:
“This algorithm assigns each task a score based on several factors: its priority level, how close the due date is, whether it’s completed, and whether it has critical tags. It then sorts tasks by score so the most urgent and important tasks appear first. Think of it like a weighted checklist where priority is the foundation, but urgency and special conditions can push tasks higher or lower.”


4. Did you test this understanding against AI?
Yes — by asking the AI to walk through sample tasks (e.g., a high-priority overdue task vs. a low-priority task due today). The AI confirmed the scoring outcomes matched my expectations, which validated my understanding.

5. How might you improve the algorithm based on your understanding?
- Parameter tuning: Adjust weights (e.g., overdue penalty vs. priority boost) based on real user feedback.
- Configurable weights: Allow admins/users to customize scoring rules.
- Machine learning approach: Collect task completion data and train a model to optimize sorting.
- Edge case handling: Ensure completed tasks don’t accidentally outrank active ones due to tags or updates.


Algorithm 2: Task Text Parser
How did the AI’s explanation change your understanding of the algorithm?
At first glance, I thought the parser was just splitting text into tokens. After the AI explanation, I realized it’s a rule-based parser that:
- Extracts priority markers (!N or !urgent).
- Extracts tags (@tag).
- Extracts date markers (#date).
- Cleans the title by removing those markers.
- Converts natural language dates (“tomorrow”, “friday”) or ISO dates (YYYY-MM-DD) into actual Date objects.
This showed me the algorithm is essentially a lightweight DSL (domain-specific language) for tasks.

2. What aspects were still difficult to understand after AI explanation?
- How flexible the date parsing is (e.g., does #nextweek always mean exactly 7 days later, or does it align with Monday?).
- How multiple date markers are handled (the code breaks after the first valid one).
- Whether tags and priorities can conflict (e.g., !urgent and !2 in the same string).

3. How would you explain this algorithm to another junior developer?
I’d say:
“This parser takes a free-form task description and looks for special markers. Anything starting with @ becomes a tag, anything starting with ! sets the priority, and anything starting with # sets the due date. After extracting those, the remaining text is the task title. It’s like shorthand commands embedded in plain text that get converted into a structured task object.”


4. Did you test this understanding against AI?
Yes — by walking through examples like:
- "Buy milk @shopping !2 #tomorrow" → Title: “Buy milk”, Tag: shopping, Priority: Medium, Due date: tomorrow.
- "Finish report !urgent #2026-02-20 @work" → Title: “Finish report”, Tag: work, Priority: Urgent, Due date: Feb 20, 2026.
The AI confirmed these outputs matched the intended parsing logic.

5. How might you improve the algorithm based on your understanding?
- Support multiple dates: Instead of breaking after the first, allow multiple date markers (e.g., start/end dates).
- Configurable syntax: Let users define custom markers (e.g., %project instead of @project).
- Natural language parsing: Integrate with NLP libraries to handle phrases like “next Friday” or “in 3 days”.
- Error handling: Provide feedback if a marker is invalid instead of silently ignoring it.
- Extensibility: Add support for recurring tasks (#weekly, #monthly).


The AI helped me see this parser as a structured shorthand interpreter rather than just regex extraction. It’s a neat way to turn free-form text into structured tasks, but it could be improved with more flexible date handling and extensibility.

Algorithm 3: Task List Merging (Two-Way Sync)
. How did the AI’s explanation change your understanding of the algorithm?
At first, I thought the merge was just a simple union of local and remote tasks. After the AI explanation, I realized it’s a two-way synchronization algorithm with conflict resolution rules:
- Tasks unique to local → created remotely.
- Tasks unique to remote → created locally.
- Tasks present in both → resolved by comparing timestamps and special rules (e.g., completed status always wins).
- Tags are merged as a union, ensuring no loss of metadata.
This clarified that the algorithm is designed to keep both sources consistent while preserving the most recent or most important changes.

2. What aspects were still difficult to understand after AI explanation?
- How conflicts are handled when timestamps are equal but fields differ (e.g., same update time but different titles).
- Whether merging tags could cause duplication or unintended growth if tags are added frequently.
- How deletions are handled (the algorithm doesn’t seem to account for tasks removed in one source).

3. How would you explain this algorithm to another junior developer?
I’d say:
“This algorithm takes two sets of tasks — one local and one remote — and merges them into a single consistent list. If a task only exists in one place, it gets copied to the other. If it exists in both, the algorithm compares timestamps to decide which version is newer, and applies special rules for completed tasks. Tags are combined from both versions. The result is a merged list plus instructions on what needs to be updated or created in each source.”


4. Did you test this understanding against AI?
Yes — by walking through scenarios:
- A task only in local → flagged for remote creation.
- A task only in remote → flagged for local creation.
- A task in both, remote newer → local updated.
- A task in both, local newer → remote updated.
- A task completed in one source → completion status overrides the other.
The AI confirmed these outcomes matched the intended design.

5. How might you improve the algorithm based on your understanding?
- Handle deletions: Add logic for tasks deleted in one source (e.g., tombstone markers).
- Conflict resolution strategy: Allow configurable rules (e.g., prefer remote always, or prefer local always).
- Granular field merging: Instead of “most recent wins,” merge fields individually (title, description, due date) if updated separately.
- Performance optimization: Use diffing rather than copying entire objects.
- Audit logging: Record which source was updated and why, for debugging sync issues.


The AI helped me see this as a synchronization algorithm with conflict resolution, not just a merge. It ensures consistency across local and remote sources, prioritizes recency and completion status, and merges tags intelligently. Improvements could make it more robust for deletions and complex conflicts.


