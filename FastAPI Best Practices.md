
#### 1. Never use `async def` for Blocking Operation

Blocking operations are tasks that make your program wait until they are finished before moving on .

E.g. 1
```python
print("Start")
time.sleep(10)
next_task()
```

E.g. 2
```python
time.sleep(10)
open("file.txt").read()
requests.get("https://api.com")
MongoClient().db.collection.find_one()
```


> [!NOTE] Blocking Operations
> Reading from or Writing to a file,
> Making HTTP requests with libraries like request,
> Running database queries using synchronous clients
> 
> **are blocking operations.**


#### IMPORTANT
**FAST API** runs endpoint functions defined with `async def` in the **main thread**. If we put blocking operations within `async def` , our application will become unresponsive! It won't be able to process other requests until that blocking operation finishes.

**The solution**? Just define these functions using the `def` keyword.

```python
# BAD
@app.get("/")
async def endpoint():
	time.sleep(10)
```

```python
# GOOD
@app.get("/")
def endpoint():
	time.sleep(10)
```


> [!NOTE] 
> FASTAPI is designed to recognize that we are performing blocking operations within these `def` endpoints, and **it will intelligently run them in separate threads**. This way our application remains responsive. 

![[image.png|552x299]]


#### 2. Use `async` friendly code.

Non-blocking means more requests can be handled concurrently.

```python
async def endpoint():
	# use non-blocking code
	...
```

Use non-blocking libraries as much as possible.

```python
async def endpoint():
	await asyncio.sleep(1)
	
	async with httpx.AsyncClient() as client:
		await client.get(url)
		
	client = AsyncIOMotorClient()
	await client.db.collection.find_one()
```



### 3. Avoid heavy computation in endpoints

It blocks the server - just like blocking operations.

> [!NOTE] Design
> FASTAPI is desinged for I/O - bound workloads, not for CPU/GPU heavy tasks.

Avoid processing images, videos or running heavy machine learning models directly in your endpoints. 

**What should you do then?**
If your ML model is lightweight and inferences fast (under < 100ms, low traffic) then you can get away with using FASTAPI directly. 

But for heavier ML models, **use dedicated inference engines** like Triton, TorchServe etc, and use FASTAPI to validate inputs and route the requests.  

![[image-1.png|326x112]]


For truly long running computations, use a **Queue + Worker** system.

![[image-2.png|541x228]]

When client sends a request to FastAPI, FastAPI **enqueues** a job to a **message broker** like *RabbitMQ*. A separate worker, e.g., *Celery* **pulls the job from the queue**, **runs** the heavy computation, and **stores the result** in the database. 


#### 4. Follow the same rules in FastAPI Dependencies.

- Define your dependency with `def` if you're doing any blocking operation inside.
- Use `async def` if it's light, CPU-bound, and entirely non-blocking. 
- Do not perform any heavy computation inside your dependency.


#### 5. Don't make your users wait

When you have operations that don't need to block the client from getting a response, things like **sending a confirmation email** or logging event, you should avoid making the client wait by doing it in the endpoint.

```python
# don't do this

@app.post("/register")
async def register_user(user_data: UserCreate):
	await send_email(user_data.email)
	await event_user_registered(user_data.email)
	return {"message": "OK"}
```


Instead, use FastAPI's built in **BackgroundTasks**. They are perfect for small, non-critical, fire and forget tasks. 

```python
@app.post("/register")
async def register_user(user_data: UserCreate, bg_tasks: BackgroundTasks):
	...
	bg_tasks.add_task(send_email, user_data.email)
	bg_tasks.add_task(event_user_registered, user_data.email)
	return {"message": "OK"}
```


> [!NOTE] Important
> 
Since these background tasks run within the same event loop as your main application, you will want to follow the same rules for deciding between `async def` and `def`.

![[image-3.png|597x88]]


However, it is crucial to understand the limitations. **Don't use** background tasks for anything **requiring guarantee delivery, retries** or **tasks** that run for **long duration**.

> [!NOTE] Remember
> Background tasks are not guaranteed!
> 
> If your FastAPI process crashes before a background task completes, that task will fail. No retries.

 For those more robust needs,  a dedicated **message queue** and **worker system** (like Celery) is still the superior choice. 


#### 6. Disable Swagger and ReDoc in Production
FastAPI automatically generates these docs which is great during development. But in production they can expose endpoints that might still be incomplete or lack proper  security. 

To prevent this, make sure to set `docs_url`, `redoc_url` and `openapi_url` to `None` in your production settings.

```python
app = FastAPI(
	docs_url = None if PRODUCTION else "/docs",
	redoc_url = None if PRODUCTION else "/redoc",
	openapi_url = None if PRODUCTION else "/openapi.json",
)
```

#### 7. Create a custom Pydantic BaseModel

Create a custom base model and use it to define your `Pydantic` models. 

```python
class CustomBaseModel(BaseModel):
	...
	
	
class UserSchema(CustomBaseModel):
	...
```

The benefit is that it's easier to manage your global configuration in one place.
For example, if you want to return camelCase to the frontend but prefer snake_case in Python, you can set an alias generator to handle that.

```python
class CustomBaseModel(BaseModel):
	class Config:
		alias_generator = to_camel
		populate_by_name = True
		json_encoders = {
			datetime: datetime.isoformat,
			Decimal: float,
			ObjectId: str,
		}
```

Another common use case is **simplifying JSON encoding** when returning data. If you try to return a datetime or Decimal object, you'll get an encoding error. So, define your encoders globally. 

You can format `datetime` objects as `string`, convert `Decimal` to `float`, and if you're working with MongoDB - convert `ObjectId` instances to `string`.


#### 8. Don't manually construct response models in your FastAPI endpoints

If you've set a `response_model`, there's no performance benefit to creating that model yourself just to return it. **FastAPI will do it anyway.**

![[image-4.png|475x207]]


![[image-5.png|367x297]]

It first converts your return value to a dictionary or a list, validates it against the `response_model` , and then encodes it to JSON. **Just return a plain dictionary, and let FastAPI take care of the rest.**


```python
@ap.get("/user", response_model=UserOut)
async def get_user():
	...
	return UserOut(**user_data)

# No performance benefit
```

```python
@app.get("/user", respones_model=UserOut)
async def get_user():
	...
	return user_data

# Let FastAPI handle the response
```


#### 9. Validate with Pydantic, Not in endpoints

Push as much validation and structure definition as possible into your Pydantic model. **That's what they're built for.**


> [!NOTE] Important
> Don't scatter validation across your route functions with if statements and manual checks. It might seem easier at first, but it quickly turns into a mess. You lose consistent error responses, and clients get cryptic 400s with no clue why their request failed.
> 
> You end up repeating the same logic in multiple places. And the worst part, your OpenAPI docs won't reflect any of that hidden validation.


If Pydantic does not support the validation you need, don't write it inside the endpoint function - add custom validation logic directly inside the model instead.


#### 10. Use dependencies for DB-based validation

If your validation logic requires a database query,  like checking whether a resource belongs to the current user before allowing changes, don't put that logic directly in your endpoint.

```python
# DONT DO THIS
@app.put("posts/{post_id}")
async def update_post(...):
	post = await db.get(post_id)
	if post.user_id != current_user.id:
		raise HTTPExceptoin(...)
```


Instead create a **dependency** and use it.

```python
# INSTEAD use dependency injection
async def validate_owner(
	post_id: int,
	user = Depends(get_user)
): 
	post = await db.get(post_id)
	if post.user_id != user.id:
		raise HTTPException(403)
	return post


@app.put("posts/{post_id}")
async def update_post(
	post = Depends(validate_owner)
):
	...
```

The first benefit is that you can easily reuse the dependency across multiple endpoints with just a single  line of code.

Second, there's no performance hit either. **FastAPI caches dependencies per request**, your dependency will be evaluated once in a request.  It is efficient and helps to keep your endpoint clean.


#### 11. Avoid creating a new DB connection in every endpoint.

You should use a connection pool and access these connections through dependency injection.
There are two common ways to do this. 

The **first one** is storing the db connections pool in the app state.

```python
async def lifespan(app):
	app.state.pool = await create_pool()
	yield
	await app.state.pool.close()
	
	
async def get_conn(request: Request):
	async with request.app.state.pool.acquire() as conn:
		yield conn
		

@app.get("")
async def endpoint(db_conn = Depends(get_conn)):
	...
```

- Initialize the connection pool within the lifespan function.
- Store it in the app.state.
- After that create a dependency that retrieves a connection from this pool. You will typically use an `async with` block to ensure the conneciton is properly released.


**Second** is the legacy **global-style pool**. This approach involves defining the connection pool as a global variable and populating it during the app's lifespan event.

```python
pool = None

async def lifespan(app):
	global pool
	pool = await create_pool()
```


> [!NOTE] Note
> While the first method is the recommended approach, you will still come across the global style pool in many existing codebases. 



#### 12. Use the new lifespan event for managing app level resources

It's better to use FastAPI's newer lifespan feature instead of the older `@app.on_event("startup")` and  `@app.on_event("shutdown")` decorators.

```python
# OLD WAY

@app.on_event("startup")
def setup():...


@app.on_event("shutdown")
def cleanup():...


# NEW WAY

async def lifespan(app):
	...      # <- DB, Redis, etc.
	yield    # <- App runs here
	...      # <- Close all resources
	
app = FastAPI(lifespan=lifespan)
```

`lifespan` gives you a single, unified context to handle setup - like initializing database connections, cache clients or starting background tasks - and cleanup when the app shuts down.


#### 13. Don't hardcore secrets.

Keep them in a `.env` file. Make sure you add `.env` to your `.gitignore`.

Avoid using `os.environ[]` all over your codebase. It gets messy fast. A better approach is to centralize everything in a config class, like Pydantic's `BaseSettings`  class. 

```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
	db_url: str
	api_key: str
	debug: bool
	
settings = Settings()
```


> [!NOTE]  
It validates all your environment variables upfront. If something's missing or misconfigured, it fails early right when the server starts, and it makes switching between development and production environments a lot smoother.


#### 14. Use structured logging

It's highly recommended to use Python's built in `logging` library pair