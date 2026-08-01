---
title: "How to Write About a Project (Even If It's Small)"
translationKey: "how-to-write-about-projects"
date: 2024-03-15
draft: true
tags: ["career", "portfolio", "writing"]
summary: "A practical guide to documenting your projects in a way that demonstrates skills and attracts opportunities—no matter how simple the project."
---

## The Insecurity Problem

I've talked to dozens of students who say: "I don't have anything worth writing about."

They've built:
- A to-do list app
- A weather dashboard
- A simple game
- A data visualization
- A Discord bot

But they don't write about them because "everyone's done that" or "it's too basic."

Here's the truth: The project's complexity doesn't matter. What matters is how you talk about it.

A well-documented "simple" project beats an undocumented "complex" one every time.

## Why Write About Small Projects?

### 1. Demonstrates Communication Skills
Writing clearly about technical work is a core skill. Most developers can code. Fewer can explain their code.

### 2. Shows Your Process
Recruiters want to see how you think, not just what you built. A simple project with clear reasoning is more impressive than a complex project with no explanation.

### 3. Builds Your Portfolio
You need content. Three well-documented small projects beat zero documented large projects.

### 4. Helps You Learn
Writing forces you to understand what you did and why. It's a learning tool, not just a showcase.

### 5. SEO and Discoverability
When recruiters Google you, they find your work. When other students search for solutions, they find your posts. Both are valuable.

## The Framework: Before/After

The simplest way to frame any project is: "Before this, X was a problem. After this, X is solved."

### Example: To-Do List App

**Weak framing:**
> "I built a to-do list app with React."

**Strong framing:**
> "I was using paper notes and losing track of tasks. I built a web app to organize my tasks by priority and due date. Now I have a single place to manage everything, and I learned React in the process."

See the difference? The second version:
- Explains the motivation (real problem)
- Describes the solution (specific features)
- Shows the outcome (solved problem + learned skill)

## Project Documentation Template

Here's a structure that works for any project, big or small:

### 1. Overview (2-3 sentences)

What is it? What does it do? Why did you build it?

```markdown
## Overview

A web-based to-do list application that helps me organize tasks by priority and due date. Built to solve my own problem of losing track of assignments and to learn React fundamentals.

**Live Demo:** [link]
**GitHub:** [link]
```

### 2. The Problem (1-2 paragraphs)

What problem were you solving? Be specific. Personal problems are valid problems.

```markdown
## The Problem

I was using a combination of paper notes, phone reminders, and random text files to track assignments, project deadlines, and personal tasks. I'd forget to check my notes, miss deadlines, and waste time searching for where I wrote something down.

I needed a single place to see all my tasks, organized by what's urgent and what's coming up. I also wanted to learn React, so building something I'd actually use seemed like the perfect project.
```

**Why this works:**
- Specific details (paper notes, text files)
- Real consequences (missed deadlines)
- Clear motivation (learn React + solve problem)

### 3. Tech Stack

List what you used and briefly why.

```markdown
## Tech Stack

- **Frontend:** React - wanted to learn component-based architecture
- **Styling:** CSS Modules - scoped styling without extra dependencies
- **Storage:** LocalStorage - simple persistence without a backend
- **Deployment:** Netlify - free hosting with easy deployment
```

Don't just list technologies. Add one sentence explaining your choice.

### 4. Key Features

What can it do? Use bullet points for clarity.

```markdown
## Key Features

- ✅ Add, edit, and delete tasks
- 🎯 Set priority levels (high, medium, low)
- 📅 Add due dates with visual indicators
- 🔍 Filter tasks by priority or completion status
- 💾 Automatic saving to browser storage
- 📱 Responsive design for mobile use
```

### 5. What I Learned

This is the most important section. What skills did you develop?

```markdown
## What I Learned

### React Fundamentals
This was my first React project. I learned:
- Component composition and props
- State management with `useState`
- Side effects with `useEffect`
- Conditional rendering
- Event handling

### Data Persistence
I learned about browser storage:
- LocalStorage API
- JSON serialization
- Handling storage limits
- Data migration when structure changes

### CSS and Responsive Design
- CSS Grid for layout
- Media queries for mobile responsiveness
- CSS variables for theming
- Mobile-first design approach

### Deployment
- Building for production
- Environment variables
- Continuous deployment with Git
- Custom domain setup
```

**Why this works:**
- Specific skills, not vague claims
- Shows depth of learning
- Demonstrates you understand what you did

### 6. Challenges and Solutions

What went wrong? How did you fix it? This shows problem-solving.

```markdown
## Challenges

### Challenge 1: State Management Complexity

**Problem:** As I added features, managing state became messy. I had separate state for tasks, filters, and UI elements, and they weren't syncing properly.

**Solution:** I restructured my state to use a single source of truth and derived values from it. Instead of storing filtered tasks in state, I computed them on render.

```javascript
// Before: Multiple state variables
const [tasks, setTasks] = useState([]);
const [filteredTasks, setFilteredTasks] = useState([]);
const [filter, setFilter] = useState('all');

// After: Single source of truth
const [tasks, setTasks] = useState([]);
const [filter, setFilter] = useState('all');

// Compute filtered tasks
const filteredTasks = tasks.filter(task => {
  if (filter === 'completed') return task.completed;
  if (filter === 'active') return !task.completed;
  return true;
});
```

**What I learned:** Keep state minimal and derive values when possible. This reduces bugs and makes the code easier to reason about.

### Challenge 2: Mobile Layout

**Problem:** The desktop layout broke on mobile. Buttons were too small, and the task list was hard to read.

**Solution:** I adopted a mobile-first approach, designing for small screens first, then adding complexity for larger screens.

```css
/* Mobile first */
.task-item {
  padding: 1rem;
  font-size: 1rem;
}

/* Desktop enhancement */
@media (min-width: 768px) {
  .task-item {
    padding: 0.75rem;
    font-size: 0.9rem;
  }
}
```

**What I learned:** Mobile-first design is easier than trying to make a desktop design responsive. Start small and add complexity.
```

### 7. Results and Impact

What happened after you built it? Even small impacts count.

```markdown
## Results

- 🎯 I've used it daily for 3 months
- ✅ Tracked 200+ tasks without missing a deadline
- 👥 Shared with 3 classmates who now use it
- 📈 Reduced time spent organizing tasks by ~30 minutes/week
- 💡 Got feedback that led to 5 feature improvements
```

**Why this works:**
- Concrete numbers (200+ tasks, 3 months)
- Real usage (daily use)
- External validation (others use it)
- Measurable impact (30 minutes saved)

### 8. Future Improvements

What would you add next? Shows you're thinking ahead.

```markdown
## Future Improvements

- **Backend integration:** Move from LocalStorage to a database for cross-device sync
- **Collaboration:** Allow sharing tasks with others
- **Recurring tasks:** Support for daily/weekly repeating tasks
- **Categories:** Organize tasks by project or area of life
- **Analytics:** Visualize productivity trends over time
```

### 9. Takeaways

End with 3-5 concrete lessons.

```markdown
## Key Takeaways

1. **Build for yourself:** Projects you'll actually use are more motivating and lead to better results.

2. **Start simple:** I wanted to add 20 features initially. Starting with core functionality and iterating was much better.

3. **Get feedback early:** Showing it to friends after week 1 led to improvements I wouldn't have thought of.

4. **Documentation matters:** Writing this post helped me understand what I actually learned.

5. **Small projects are valid:** This "simple" to-do app taught me more than following a tutorial would have.
```

## Real Example: Reframing a "Simple" Project

Let's take a basic project and show how to present it well.

### Project: Weather Dashboard

**Weak version:**
> "I made a weather app using a weather API. It shows the temperature and conditions."

**Strong version:**

---

## Weather Dashboard

A clean, fast weather dashboard that shows current conditions and a 5-day forecast for any city.

**Live Demo:** [weather.yoursite.com](https://weather.yoursite.com)  
**GitHub:** [github.com/you/weather-dashboard](https://github.com/you/weather-dashboard)

### The Problem

I was tired of opening weather.com and dealing with ads, slow loading, and cluttered interfaces. I wanted a fast, clean way to check the weather—especially when planning my bike commute to campus.

### What Makes This Different

- **Fast:** Loads in under 1 second
- **Clean:** No ads, no clutter, just weather data
- **Useful:** Shows "feels like" temperature and wind speed (important for biking)
- **Persistent:** Remembers your last searched city

### Tech Stack

- **Frontend:** Vanilla JavaScript (wanted to practice fundamentals without frameworks)
- **API:** OpenWeatherMap API
- **Styling:** CSS Grid and custom properties
- **Deployment:** GitHub Pages

### What I Learned

**API Integration:**
- Making HTTP requests with `fetch()`
- Handling API errors and rate limits
- Parsing JSON responses
- Securing API keys (environment variables)

**Async JavaScript:**
- Promises and async/await
- Error handling with try/catch
- Loading states and user feedback

**UI/UX:**
- Skeleton screens while loading
- Error messages that help users
- Responsive design for mobile
- Accessibility (keyboard navigation, ARIA labels)

### Challenges

**Challenge: API Key Security**

I initially hardcoded my API key in the JavaScript file. A classmate pointed out this was visible in the browser's dev tools.

**Solution:** I moved the API key to a backend proxy (simple Netlify function) that makes the actual API call. The frontend calls my proxy, which adds the API key server-side.

**What I learned:** Never expose API keys in frontend code. Always use a backend proxy or environment variables.

### Results

- ⚡ Loads 5x faster than weather.com
- 🚴 I check it every morning before biking to class
- 👥 10 friends bookmarked it and use it regularly
- 📱 Works perfectly on mobile
- ⭐ 15 GitHub stars from sharing on Reddit

### Future Plans

- Add weather alerts
- Support for multiple saved cities
- Historical weather data
- Weather-based activity suggestions ("Good day for biking!")

### Takeaways

1. **Solve your own problems:** I use this daily because it solves a real problem I had.
2. **Simple can be better:** A focused tool that does one thing well beats a complex tool that does everything poorly.
3. **Security matters:** Learning about API key security early saved me from a costly mistake.
4. **Share your work:** Posting on Reddit led to great feedback and users.

---

See how much more compelling that is? Same project, better presentation.

## Common Mistakes to Avoid

### 1. Apologizing for Simplicity

**Don't say:**
- "This is just a simple project..."
- "I know this isn't very impressive..."
- "Everyone's done this before..."

**Instead:**
- Focus on what you learned
- Emphasize the problem you solved
- Show your unique approach or insights

### 2. Only Showing Code

Code without context is hard to evaluate. Always explain:
- Why you wrote it this way
- What problem it solves
- What alternatives you considered

### 3. Skipping the "Why"

Don't just say what you built. Explain:
- Why you built it
- Why you chose these technologies
- Why you made specific decisions

### 4. No Visuals

Add screenshots, diagrams, or GIFs. Visual proof makes your project more tangible.

### 5. Ignoring Failures

Failures are learning opportunities. Share what didn't work and what you learned from it.

## Linking to GitHub

Your project post should link to your GitHub repo. Make sure your repo has:

### Essential README Sections

```markdown
# Project Name

Brief description

## Features

- Feature 1
- Feature 2

## Installation

```bash
git clone https://github.com/you/project
cd project
npm install
npm start
```

## Usage

[How to use it]

## Technologies

- Tech 1
- Tech 2

## What I Learned

[Brief summary]

## License

MIT
```

### Good Commit Messages

Show your development process with clear commits:

```
✅ Good:
- "Add task filtering by priority"
- "Fix mobile layout overflow issue"
- "Implement LocalStorage persistence"

❌ Bad:
- "Update"
- "Fix stuff"
- "Changes"
```

## Action Steps

Ready to document your project? Here's what to do:

1. **Pick a project** (any project, even if it's "simple")
2. **Answer these questions:**
   - What problem did it solve?
   - What did you learn?
   - What challenges did you face?
   - What would you do differently?
3. **Write a draft** using the template above
4. **Add visuals** (screenshots, diagrams, code snippets)
5. **Link to your code** (GitHub repo with good README)
6. **Publish and share**

## Final Thoughts

Your "simple" projects are more valuable than you think. They demonstrate:
- You can finish things
- You can learn independently
- You can communicate technical concepts
- You solve real problems

Stop waiting for the "perfect" project to write about. Document what you've built, share what you've learned, and help others who are one step behind you.

The best time to start was yesterday. The second best time is now.

---

**Resources:**
- [Template repository with examples](https://github.com/yourusername/project-template)
- [My project portfolio](https://yoursite.com/projects)
- [More writing tips](https://yoursite.com/posts/writing-tips)

**Questions?** Reach out on [Twitter](https://twitter.com/you) or [email](mailto:your.email@example.com). I'm happy to review your project writeups!
