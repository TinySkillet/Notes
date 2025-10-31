
#### 1. Never use `async def` for Blocking Operation

Blocking operations are tasks that make your program wait until they are finished before moving on .

E.g. 1
```python
print("Start")
time.Sleep(10)
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
def endpoint():
	time.sleep(10)
```


> [!NOTE] 
> FASTAPI is designed to recognize that we are performing blocking operations within these `def` endpoints, and **it will intelligently run them in separate threads**. This way our application remains responsive. 

![[image.png|477x258]]


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

![[image-1.png|504x227]]


For truely, long running computations, use a **Queue + Worker** system.

![[image-2.png|518x218]]

When client sends a request to FastAPI, FastAPI **enqueues** a job to a **message broker** like *RabbitMQ*. A separate worker, e.g., *Celery* p**ulls the job from the queue**, **runs** the heavy computation, and **stores the result** in the database. 


#### 4. Follow the same ru