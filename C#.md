## Task And Thread
Here are some differences between a task and a thread.

- The Thread class is used for creating and manipulating a thread in Windows. A Task represents some asynchronous operation and is part of the - - - Task Parallel Library, a set of APIs for running tasks asynchronously and in parallel.
- The task can return a result. There is no direct mechanism to return the result from a thread.
- Task supports cancellation through the use of cancellation tokens. But Thread doesn't.
- A task can have multiple processes happening at the same time. Threads can only have one task running at a time.
- We can easily implement Asynchronous using ’async’ and ‘await’ keywords.
- A new Thread()is not dealing with Thread pool thread, whereas Task does use thread pool thread.
- A Task is a higher level concept than Thread.