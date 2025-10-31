
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

Reading from or writing to file,
Making HTTP request with libraries like request,
Database queries using synchronous clients  

are blocking operations.


