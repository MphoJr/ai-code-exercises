
Exercise Part 1: Understanding a Specific Feature (Task Creation & Status Updates)

Files identified:
- `TaskController.js` → handles incoming requests for task creation and updates.
- `TaskService.js` → contains business logic for creating tasks and updating status.
- `TaskModel.js` → defines the database schema for tasks.

Relevant snippets (example):
```js
// TaskController.js
router.post('/tasks', async (req, res) => {
  const task = await TaskService.createTask(req.body);
  res.json(task);
});

router.put('/tasks/:id/status', async (req, res) => {
  const updated = await TaskService.updateStatus(req.params.id, req.body.status);
  res.json(updated);
});
```

Execution flow:
1. User submits a request (via UI or API).
2. Controller receives request → calls service.
3. Service validates data → interacts with TaskModel.
4. Task is stored in DB; response returned to client.

Main components:
- Controller (entry point).
- Service (business logic).
- Model (data persistence).

Design patterns:
- **Layered architecture** (Controller → Service → Model).
- **Repository pattern** for database access.

 Exercise Part 2: Task Prioritization System

Initial understanding:
- Task priority is likely an enum (`LOW`, `MEDIUM`, `HIGH`).
- Stored in the Task entity and used for sorting/filtering.

Guided questions revealed:
- Priority affects not only sorting but also **business rules** (e.g., overdue tasks with high priority are exempt from abandonment).
- There may be a **utility function** that maps priority to numeric weights for scheduling.

Misconceptions clarified:
- Initially thought priority was only cosmetic (UI sorting).  
- Discovered it drives **logic in overdue handling and notifications**.

Key insight:
- Priority is a **core domain concept**, not just metadata.

Exercise Part 3: Mapping Data Flow (Task Completion)

**Entry points:**
- `PUT /tasks/:id/complete` endpoint in `TaskController`.

Code handling state change:
```js
// TaskService.js
async function markComplete(taskId) {
  const task = await TaskModel.findById(taskId);
  task.status = 'COMPLETED';
  await task.save();
  return task;
}
```

Data flow diagram (simplified):
```
UI → Controller → Service → Model → Database → Response → UI update
```
State changes:
- Status field changes from `PENDING`/`IN_PROGRESS` → `COMPLETED`.

Persistence:
- Change saved in DB via ORM (Prisma/Mongoose/JPA).

Potential failure points:
- Invalid task ID.
- DB connection issues.
- Race conditions if multiple updates occur simultaneously.

Exercise Part 4: Reflection & Presentation

High-level architecture:
- Frontend: React components for task UI.
- **Backend: REST API with controllers, services, models.
- **Database: Relational (Postgres) or document (MongoDB).

Three key features:
1.Task creation: Controller → Service → Model → DB.
2. Prioritization: Enum-driven logic affecting sorting and rules.
3. Completion: Status update persisted in DB, reflected in UI.

Interesting design pattern:
- Layered architecture with clear separation of concerns.
- Business rules encapsulated in service layer.

Challenges:
- Understanding how priority influenced business rules.  
- Prompts helped uncover hidden logic beyond surface-level assumptions.


Expected Outcomes Achieved
- Can explain the application at a high level.
- Mapped data flow for task completion.
- Understood state management and persistence.
- Demonstrated effective use of AI prompts to clarify misconceptions.

