## Task And Thread

| Difference | Task                                        | Thread                                                      |
|-------------|---------------------------------------------|-------------------------------------------------------------|
| `Purpose`     | Represents an asynchronous operation and is part of the Task Parallel Library. | Used for creating and manipulating a thread in Windows.      |
| `Result`      | Task can return a result.                   | No direct mechanism to return the result from a thread.      |
| `Cancellation` | Supports cancellation through cancellation tokens. | Thread doesn't support cancellation.                         |
| `Concurrent Processes` | Can have multiple processes happening at the same time. | Can only have one task running at a time.                   |
| `Asynchronous` | Easily implement Asynchronous using 'async' and 'await' keywords. | Asynchronous behavior is not as straightforward.            |
| Thread Pool  | Uses thread pool thread.                    | A new Thread() is not dealing with Thread pool thread.      |
| Level       | Higher level concept than Thread.            | Lower-level concept within a process.                       |

