
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