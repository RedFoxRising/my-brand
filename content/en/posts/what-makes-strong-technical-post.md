---
title: "What Makes a Strong Technical Blog Post"
translationKey: "what-makes-strong-technical-post"
date: 2024-03-10
draft: true
tags: ["writing", "career", "learning-in-public"]
summary: "A guide to writing technical posts that demonstrate your thinking, help others, and build your professional brand."
---

## Why This Matters

When I started writing technical posts, I thought I needed to explain complex algorithms or build something groundbreaking. I was wrong.

The best technical posts aren't about showing off—they're about showing your thinking. Recruiters and hiring managers want to see:
- How you approach problems
- How you learn from failures
- How you communicate technical concepts
- That you can finish things

This post is a template for writing strong technical content, based on what I've learned from writing, reading others' posts, and getting feedback.

## The Problem Statement

**Start with the problem, not the solution.**

Bad example:
> "I built a React app with Redux."

Good example:
> "I was building a shopping cart feature and needed to share cart state across multiple components. Prop drilling was getting messy, so I needed a better state management solution."

### Why This Works

- Gives context for your decisions
- Shows you understand the "why" not just the "how"
- Helps readers determine if your solution applies to their problem
- Demonstrates problem-solving skills

### Template

```markdown
## The Problem

I was working on [project/feature] when I encountered [specific issue].

The challenge was [describe the constraint or difficulty].

I needed to [what you were trying to achieve].
```

## Provide Context

Don't assume readers know your setup. Briefly explain:
- What you were building
- What technologies you were using
- What constraints you had (time, resources, experience level)

### Example

```markdown
## Context

**Project:** Personal finance tracker web app
**Tech Stack:** React, Node.js, PostgreSQL
**My Experience:** Comfortable with React basics, first time using a database
**Constraint:** Needed to deploy for free (student budget)
```

This helps readers understand your decisions and whether your approach fits their situation.

## What You Tried

**Document your exploration process.**

This is where many posts fall short. Don't just show the final solution—show what you tried first. This is incredibly valuable because:

1. It shows your thought process
2. It helps others avoid the same dead ends
3. It demonstrates persistence
4. It's more realistic than "I built it perfectly the first time"

### Example

```markdown
## Attempt 1: Local State

My first approach was using React's `useState` in the parent component:

```javascript
function App() {
  const [cart, setCart] = useState([]);
  
  return (
    <div>
      <ProductList cart={cart} setCart={setCart} />
      <CartSummary cart={cart} />
      <Checkout cart={cart} setCart={setCart} />
    </div>
  );
}
```

**What happened:** This worked initially, but as I added more features, I was passing props through 3-4 levels of components. The code became hard to maintain.

**Why it failed:** Prop drilling made the component tree rigid. Adding a new feature that needed cart access meant updating multiple components.
```

## What Failed (And Why)

Be honest about failures. This is some of the most valuable content you can write.

### Template

```markdown
## Attempt 2: [Approach Name]

[Brief description of what you tried]

**Code example:**
```[language]
[relevant code snippet]
```

**What happened:** [Describe the result]

**Why it didn't work:** [Your analysis of why this approach failed]

**What I learned:** [Key takeaway from this failure]
```

### Real Example

```markdown
## Attempt 2: Context API

I switched to React Context to avoid prop drilling:

```javascript
const CartContext = createContext();

function CartProvider({ children }) {
  const [cart, setCart] = useState([]);
  
  return (
    <CartContext.Provider value={{ cart, setCart }}>
      {children}
    </CartContext.Provider>
  );
}
```

**What happened:** This eliminated prop drilling, but I noticed performance issues. Every cart update re-rendered all components consuming the context.

**Why it didn't work:** Context API re-renders all consumers when any value changes. With 50+ products on the page, adding one item to the cart caused unnecessary re-renders.

**What I learned:** Context API is great for infrequently changing data (theme, auth), but not ideal for frequently updated state like a shopping cart.
```

## What Worked

Now show your solution. But don't just dump code—explain your reasoning.

### Structure

1. **Why you chose this approach**
2. **How you implemented it**
3. **What makes it better than previous attempts**
4. **Trade-offs you accepted**

### Example

```markdown
## Solution: Redux Toolkit

I chose Redux Toolkit because:
- Handles frequent state updates efficiently
- Provides DevTools for debugging
- Scales well as the app grows
- Has great documentation for beginners

**Implementation:**

```javascript
// cartSlice.js
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: { items: [] },
  reducers: {
    addItem: (state, action) => {
      state.items.push(action.payload);
    },
    removeItem: (state, action) => {
      state.items = state.items.filter(item => item.id !== action.payload);
    },
  },
});

export const { addItem, removeItem } = cartSlice.actions;
export default cartSlice.reducer;
```

**Why this works:**
- Components only re-render when their specific data changes
- State logic is centralized and testable
- Redux DevTools let me debug state changes easily

**Trade-offs:**
- More boilerplate than Context API
- Steeper learning curve
- Might be overkill for very simple apps

For my use case (growing app with complex state), these trade-offs were worth it.
```

## Include Visuals

Even simple diagrams help. You don't need fancy tools—even ASCII art or screenshots work.

### Examples

**Before/After Architecture:**
```
BEFORE (Prop Drilling):
App
├── ProductList (needs cart, setCart)
│   └── Product (needs cart, setCart)
│       └── AddToCartButton (needs setCart)
└── CartSummary (needs cart)

AFTER (Redux):
App
├── ProductList
│   └── Product
│       └── AddToCartButton (useDispatch)
└── CartSummary (useSelector)
```

**Performance Comparison:**
```
Context API: 50ms per cart update (50 component re-renders)
Redux:       5ms per cart update (2 component re-renders)
```

## Lessons Learned

End with concrete takeaways. What would you do differently? What advice would you give someone facing a similar problem?

### Template

```markdown
## Key Takeaways

1. **[Lesson 1]:** [Explanation]
2. **[Lesson 2]:** [Explanation]
3. **[Lesson 3]:** [Explanation]

## If I Did This Again

- [What you'd do differently]
- [What you'd keep the same]
- [What you'd explore next]
```

### Example

```markdown
## Key Takeaways

1. **Start simple:** I should have stuck with local state longer. Redux was overkill until I had real performance issues.

2. **Measure before optimizing:** I assumed Context API was slow before actually measuring. Always profile first.

3. **Read the docs thoroughly:** Redux Toolkit's documentation had examples that solved my exact use case. I wasted time trying to figure it out myself first.

4. **Trade-offs are okay:** No solution is perfect. Understanding and accepting trade-offs is part of engineering.

## If I Did This Again

- Start with local state
- Move to Context API when prop drilling becomes painful
- Only add Redux when I have measured performance issues
- Spend more time reading documentation before coding
```

## Template You Can Copy

Here's a template for your next technical post:

```markdown
---
title: "[Descriptive Title]"
date: YYYY-MM-DD
tags: ["tag1", "tag2", "tag3"]
summary: "One-sentence description of what this post covers."
---

## The Problem

[What problem were you trying to solve?]

## Context

- **Project:** [What you were building]
- **Tech Stack:** [Technologies used]
- **Experience Level:** [Your experience with these tools]
- **Constraints:** [Time, budget, or other limitations]

## What I Tried

### Attempt 1: [Approach Name]

[Description and code]

**What happened:** [Result]
**Why it didn't work:** [Analysis]
**What I learned:** [Takeaway]

### Attempt 2: [Another Approach]

[Repeat structure]

## What Worked

[Your solution with explanation]

**Why this works:** [Benefits]
**Trade-offs:** [Downsides you accepted]

## Results

[Metrics, outcomes, or impact]

## Key Takeaways

1. [Lesson 1]
2. [Lesson 2]
3. [Lesson 3]

## Resources

- [Link to code]
- [Helpful documentation]
- [Related articles]
```

## Final Thoughts

Strong technical posts don't require expert-level knowledge. They require:
- Honesty about your process
- Clear explanation of your thinking
- Willingness to show failures
- Concrete examples and code

Your "small" projects and "simple" problems are valuable to write about. Someone else is facing the same issue right now, and your post could help them.

Start writing. Your future self (and future readers) will thank you.

---

**Resources:**
- [Example GitHub repo with code from this post](https://github.com/yourusername/example-repo)
- [Redux Toolkit Documentation](https://redux-toolkit.js.org/)
- [React Context API Docs](https://react.dev/reference/react/useContext)
