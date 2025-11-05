
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


#### **Executor**
To run a **DAG** we have something called **Executors.**  They define how your tasks will run.

For example,
- If you want to run your tasks sequentially, you can use a **Sequential Executor**.
- If you want to run your tasks parallel in a single machine, you can use the **Local Executor**.
- If you wan tot distribute your tasks across multiple machines, you can use the **Celery Executor**.


Look into Azure One Lake

