
---

## 🧾 Project Information
- **Project name:** EventHub  
- **Description:** A web application for creating and managing events, with attendee registration and ticket sales.  
- **Key features:**  
  - Event creation and management  
  - Attendee registration and ticket sales  
  - Email notifications and reminders  
  - Event check-in system  
  - Analytics dashboard  
- **Technologies used:** Node.js, Express, MongoDB, React, Redux, Stripe API  
- **Installation requirements:** Node.js 14+, MongoDB, npm or yarn  
- **Project structure:**  
  ```
  /client - React frontend application
  /server - Express backend API
  /server/routes - API route definitions
  /server/controllers - Business logic
  /server/models - MongoDB schemas
  /server/middleware - Auth and validation middleware
  /client/src/components - React components
  /client/src/pages - Page components
  /client/src/redux - Redux store, actions, and reducers
  /client/src/utils - Utility functions
  ```

---

## 📘 Prompt 1: Comprehensive README.md

```markdown
# EventHub

EventHub is a full-stack web application for creating and managing events. It allows organizers to set up events, manage attendees, sell tickets, and track analytics, while attendees can register, purchase tickets, and receive notifications.

## Features
- Create and manage events
- Register attendees and sell tickets
- Automated email notifications and reminders
- Event check-in system
- Analytics dashboard for organizers

## Technologies
- Node.js, Express (backend)
- MongoDB (database)
- React, Redux (frontend)
- Stripe API (payments)

## Installation
### Prerequisites
- Node.js v14+
- MongoDB installed locally or accessible remotely
- npm or yarn package manager

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/your-org/eventhub.git
   ```
2. Install dependencies:
   ```bash
   cd eventhub
   npm install
   ```
3. Configure environment variables in `.env`:
   - `MONGO_URI` – MongoDB connection string
   - `STRIPE_SECRET_KEY` – Stripe API key
   - `JWT_SECRET` – Secret for authentication
4. Start the backend:
   ```bash
   npm run server
   ```
5. Start the frontend:
   ```bash
   npm run client
   ```

## Usage
- Navigate to `http://localhost:3000` to access the frontend.
- Use the dashboard to create events, manage attendees, and view analytics.

## Configuration
- Environment variables in `.env` control database, authentication, and payment settings.
- Modify `/server/config` for advanced backend configuration.

## Troubleshooting
- **MongoDB connection errors:** Ensure `MONGO_URI` is correct and MongoDB is running.
- **Stripe errors:** Verify your API key and test mode settings.
- **Authentication issues:** Check `JWT_SECRET` consistency across backend services.

## Contributing
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-feature`)
3. Commit changes (`git commit -m "Add new feature"`)
4. Push to branch (`git push origin feature/new-feature`)
5. Open a Pull Request

## License
MIT License
```

---

## 📖 Prompt 2: Step-by-Step Guide  
**Feature:** Creating a new event (Beginner level)

### Guide: How to Create a New Event in EventHub
**Prerequisites:**
- Access to EventHub dashboard
- Organizer account with login credentials

**Steps:**
1. Log in to your EventHub account.  
2. Navigate to the **Dashboard** → **Create Event**.  
3. Fill in event details:  
   - Title  
   - Description  
   - Date and time  
   - Venue/location  
   - Ticket price and quantity  
   *(Screenshot placeholder)*  
4. Click **Save & Publish**.  
5. The event will now appear in the public event list.  

**Common mistakes:**
- Forgetting to set ticket quantity → attendees cannot register.  
- Incorrect date format → event may not display properly.  

**Troubleshooting:**
- If event doesn’t appear, check if it was saved as **Draft**.  
- Verify MongoDB connection if saving fails.  

---

## 📗 Prompt 3: FAQ Document

### EventHub FAQ

**Getting Started**
- **Q: How do I install EventHub locally?**  
  A: Clone the repo, install dependencies with `npm install`, configure `.env`, and run `npm run server` + `npm run client`.

- **Q: Do I need a Stripe account?**  
  A: Yes, for ticket sales. You can use Stripe’s test mode for development.

**Features**
- **Q: Can I send reminders to attendees?**  
  A: Yes, EventHub automatically sends email notifications before the event.

- **Q: How do I check in attendees?**  
  A: Use the built-in check-in system via the dashboard.

**Troubleshooting**
- **Q: Why can’t I connect to MongoDB?**  
  A: Ensure MongoDB is running and your `MONGO_URI` is correct.  

- **Q: Why are payments failing?**  
  A: Check your Stripe API key and ensure you’re using test mode for development.

**Advanced**
- **Q: Can I customize themes?**  
  A: Yes, modify frontend styles in `/client/src/components` and `/client/src/pages`.

---

## ✨ Reflection
- **Most challenging to document:** Explaining configuration options clearly (needed to specify environment variables).  
- **Prompt adjustments:** Adding explicit feature notes improved clarity in the step-by-step guide.  
- **Document structure insight:** README is best for setup, guides for workflows, FAQ for quick answers.  
- **Workflow use:** I’d generate README first, then layer on guides and FAQs for user support.

---

