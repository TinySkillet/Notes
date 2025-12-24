Temporal is a **Durable Execution** platform. It ensures that your code runs to completion, no matter what. If you code is in the middle of a process and the server crashes, the network goes down, or a third-party API fails, Temporal remembers exactly where your code was and resumes it once the issue is resolved.

## The Save Game Analogy
Think of Temporal like a **Video Game Save Point.**

- **Standard Code**: If the power goes out, you lose all progress and have to start the game from the beginning.
- **Temporal Code**: Every time you perform an action, the game *auto-saves*. If the power goes out, you restart exactly where at the last save point with all your items and stats intact.

## The Core Architecture
Temporal consists of three main parts working together.

1. **Temporal Server**: The **Brain**. It maintains a database of your workflow's history. It does **not** run your code; it just tracks the *state*.
2. **Temporal Worker**: The **Muscle**. This is your server running your Python code. It asks the Server, *Is there any work to do?*
3. **Task Queue**: The **Bridge**. It's a dynamic queue where the Server places tasks and Worker picks them up.


## The Four Pillars of Temporal Code
To write a Temporal application, you must define these four components.

#### 1. The Activity
An **Activity** is a function that performs a single, idempotent action. This is where you put *risky* code. 

- **Examples**: Calling a Stripe API, querying a DB, sending an email.
- **Rule**: Activities can fail. Temporal will automatically retry them based on your policy.

#### 2. The Workflow
A **Workflow** is a function that coordinates Activities. It defines the *steps* of your process.

- **The Golden Rule**: A workflow must be **deterministic**. If you run it 100 times with the same input, it must do the exact same thing.
- **Forbidden in Workflows**: `date`
