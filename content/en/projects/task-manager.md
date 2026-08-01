---
title: "Task Manager Web App"
translationKey: "task-manager"
date: 2024-01-15
draft: true
tags: ["javascript", "react", "web-development"]
summary: "A full-stack task management application with user authentication and real-time updates."
---

## Overview

A web-based task manager that helps users organize their daily tasks with categories, priorities, and due dates. Built to learn full-stack development and practice working with databases.

**Live Demo:** [demo-link.com](https://demo-link.com)  
**GitHub:** [github.com/yourusername/task-manager](https://github.com/yourusername/task-manager)

## The Problem

I was using multiple apps to track assignments, personal tasks, and project deadlines. I wanted a single place to manage everything with a clean interface and the ability to customize categories for different areas of my life.

## Tech Stack

- **Frontend:** React, CSS Modules
- **Backend:** Node.js, Express
- **Database:** PostgreSQL
- **Authentication:** JWT
- **Deployment:** Vercel (frontend), Railway (backend)

## Key Features

- ✅ User authentication and authorization
- ✅ Create, edit, and delete tasks
- ✅ Organize tasks by custom categories
- ✅ Set priorities and due dates
- ✅ Filter and search functionality
- ✅ Responsive design for mobile and desktop

## What I Learned

### Database Design
This was my first time designing a relational database schema. I learned about:
- Primary and foreign keys
- One-to-many relationships (users to tasks)
- Indexing for query performance

### Authentication Security
Implementing JWT authentication taught me about:
- Password hashing with bcrypt
- Token-based authentication
- Protecting routes and API endpoints
- Handling token expiration

### State Management
Managing application state in React was challenging. I learned:
- When to use local state vs. context
- How to avoid prop drilling
- Optimizing re-renders for performance

## Challenges

**Challenge 1: Real-time Updates**  
Initially, users had to refresh to see new tasks. I solved this by implementing polling every 30 seconds and later explored WebSockets for true real-time updates.

**Challenge 2: Mobile Responsiveness**  
The initial design broke on mobile devices. I learned CSS Grid and Flexbox properly and adopted a mobile-first approach.

**Challenge 3: Database Queries**  
Some queries were slow with many tasks. I added database indexes and learned about query optimization.

## Results

- 🎯 Successfully deployed and used by 5 friends for beta testing
- 📈 Handles 100+ tasks per user without performance issues
- 💡 Received feedback that improved the UI significantly
- 🚀 Learned the complete development lifecycle from design to deployment

## Future Improvements

- Add task sharing and collaboration features
- Implement recurring tasks
- Add data visualization (task completion trends)
- Mobile app version using React Native
- Email reminders for due dates

## Takeaways

This project taught me that building something end-to-end is the best way to learn. Every feature required solving real problems, from database design to user experience. The most valuable lesson was learning to break down large features into small, manageable tasks.

If you're interested in the code or want to contribute, check out the [GitHub repository](https://github.com/yourusername/task-manager)!
