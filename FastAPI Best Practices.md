
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
```