Temporal is often called **Durable Execution**. Think of it as a save game button for your code. If your server crashes, the power goes out, or an API call fails, Temporal ensures your code picks up exactly where it left off.

To understand **Temporal**, you need to understand the **Four Pillars**.

### 1. The Temporal Server
Think of it as the **Brain**. It tracks the state of every workflow. It doesn't run your code; it manages the *To-Do list*.


### 2. The Activity
Activities are where you put **non-deterministic** or **risky** code.

```python
@activity.defn
def say_hello(name: str) -> str:
	return f"Hello, {name}!"
```

If an activity fails *(e.g., the database id down)*, Temporal will **automatically retry** it forever (by default) with exponential backoff. Your workflow won't crash; it will just wait until the activity succeeds.

### 3. The Workflow
The workflow is the **Orchestrator**. It coordinates activities.

```python
@workflow.defn
class SayHello:
	@workflow.run
	async def run(self, name: str) -> str:
		return await workflow.execute_activity()
```
