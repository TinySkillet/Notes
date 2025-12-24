Temporal is a **Durable Execution** platform. It ensures that your code runs to completion, no matter what. If you code is in the middle of a process and the server crashes, the network goes down, or a third-party API fails, Temporal remembers exactly where your code was and resumes it once the issue is resolved.

## The Save Game Analogy
Think of Temporal like a **Video Game Save Point.**

- **Standard Code**: If the power goes out, you lose all progress and have to start the game from the beginning.
- **Temporal Code**: Every time you perform an action, the game *auto-saves*. If the power goes out, you restart exactly where at the last save point with all your items and stats intact.

## The Core Architecture
Temporal consists of three main parts working together.
