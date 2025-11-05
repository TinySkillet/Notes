
Maybe we have some data coming from multiple sources on which we do some transformation in between and load that data into a target location. 
This entire job o **extracting**, **transformation** and **loading** is **called** **workflow.**

The same terminology is used in Apache Airflow but it is known as **DAG**.

#### **DAG**: Directed Acyclic Graph

It looks something like this:
![[image.png|484x143]]


At the heart of an Airflow is a DAG that defines the collection of different tasks and their dependencies.

- **Directed** means task moves in one direction.
- **Acyclic** means there are no loops, tasks do not move in circles.
- **Graph** is a visual representation of different tasks.

This entire flow is the **DAG**.

#### **Operator**
To create tasks in a DAG, we have something called the *operator*. Think of it like a function provided by Airflow which you can use to create the tasks.

There are lots of different operators in Airflow. Examples:

- **BashOperator** - executes a bash command
- **PythonOperator** - executes an arbitrary Python function
- **EmailOperator** - sends an email

**Use the @task decorator to execute an arbitrary Python function. The @task decorator is recommended over the classic *PythonOperator***

If you want to read data from **PostgreSQL** or store data in **Amazon S3**, there are different operators that will make your life easier. 


- An **Operator** is a template or a class. It's a "cookie cutter" that knows how to do one specific 
  type of thing (run bash, run Python, connect to a database).
  
- A **Task** is a specific instance of an operator. It's the "cookie" you make with the cutter. You 
  give it a unique task_id and specific parameters (like the exact bash_command to run).

#### **Executor**
To run a **DAG** we have something called **Executors.**  They define how your tasks will run.

For example,
- If you want to run your tasks sequentially, you can use a **Sequential Executor**.
- If you want to run your tasks parallel in a single machine, you can use the **Local Executor**.
- If you wan tot distribute your tasks across multiple machines, you can use the **Celery Executor**.



> [!NOTE] 
>Look into Azure One Lake



---


####  Declaring a DAG
```python
import datetime

from airflow import DAG
from airflow.operators.empty import EmptyOperator

with DAG(
	dag_id="my_dag_name",
	start_date=datetime.datetime(2021, 1, 1),
	schedule="@daily",
):
	EmptyOperator(task_id="task")
```

```python
my_dag = DAG(
	dag_id="my_dag_name",
	start_date=datetime.datetime(2021, 1, 1),
	schedule="@daily",
)

EmptyOperator(task_id="task", dag=my_dag)
```


Or you can use the `@dag` operator to turn a function into a **DAG generator.**


### How it all works

##### When using `CeleryExecutor`
1. **You -> Webserver:** You click "run" in the UI. The Webserver's only job here is to take that request and write it into Airflow's **Metadata Database**.
    
2. **Scheduler -> Database:** The **Scheduler** is constantly checking the database for two things: (a) DAGs that are due to run on their schedule, and (b) DAGs that were manually triggered (like yours).
    
3. **Scheduler Sees the Job:** The Scheduler sees the "run" request you created. It checks the DAG's dependencies and sees that the say_hello task is ready to run.
    
4. **Scheduler -> Queue:** The Scheduler places the say_hello task into a **Queue** (like a to-do list).
    
5. **Worker -> Queue:** The **Worker** is constantly listening to the Queue, waiting for a job to do. It sees say_hello, grabs it, and is now responsible for it.
    
6. **Worker Executes:** The Worker runs the command: echo 'Hello World...'.
    
7. **Worker -> Database:** After the command finishes, the Worker updates the status of the task (e.g., "success") in the **Metadata Database**.
    
8. **Webserver -> Database:** The Webserver reads this new status from the database and updates the UI to show you that the task succeeded.



##### When using `LocalExecutor`
1. **You -> Webserver -> Database:** The request is recorded.

2. **Scheduler -> Database**: The Scheduler sees the manually triggered run request. It checks dependencies and sees the say_hello task is ready.

	Scheduler Executes: This is the key difference. The Scheduler's process now does the following:
	- It forks a new process on the same machine.
	- This new child process is given the say_hello task.
	- The child process executes echo 'Hello World...'.

3. **Child Process -> Scheduler -> Database**: When the task is complete, the child process terminates and reports its final status (e.g., "success") back to the main Scheduler process. The Scheduler then updates the task's status in the metadata database.

4. **Webserver -> Database**: This is identical. The UI reads the new status from the database and shows you the green circle of success.