# CPU Bound or I/O Bound

The OS will handle the I/O Bound works, such as access the database.
Async is suitable for CPU Bound job which is more than 5 million seconds, such as resource-consuming works.
Threads is a limited resource.

# Concurrent computing vs Parallel computing

- Concurrent computing

Multiple works whcih are computed in the same time and are interleaved execution.

- Parallel computing

Multiple works runs on different CPU in the same time.

# Problems

> Example: F1 change tires by more than 10 people

1. Fire and forget : A bad idea in some cases.
2. The main thread disappears and no one can get the result of the distributed threads.
3. The distributed thread returns result to the wrong main thread.
4. IIS is concurrent environment, but ASP.NET is default a sync environment.
5. Async-Await is easy to write, but we cannot directly get the state of the concurrent threads.


Usual problems in CSharp:

- Cannot catch the exception of concurrent job
- TPL (Task Parallel Library) and TAP (Task Asynchronous Pattern)
- Windows application hangs (UI thread is blocked)
- Is concurrent methods use threads? CPU Bound: Yes, I/O Bound: not always
- Dont use Sync designing in Async methods
- Async is not useful for Web Server CPU 100%. Its suitable for Web Server 50% but users feel slow performance. 




> Learning Async is for Performance and Responsive time

# Design

1. Async means **I'll call you backl**.
2. Notify the main thread: Success/Failure/Progress.
3. Improve the performance and Responsive time.
4. `Lock` too much codes is low performance.
5. IIS default can have 2,500 requests and 1,000 request queue. The 3,501 request will get 503 server error. 
6. Async-Await is Syntactic sugar, it uses Task(TAP) in the underlying structure.
7. .NET Core will use Async in default.
8. HttpClient is Thread-safe, can be static at async environment.


### Hyper Threading: 

Sockets: 1
Cors: 2
Logical processors: 4

1 CPU, 2 Cors and can run 4 scripts by hyper threading in the same time.

> 1. 1 CPU is suggested to have 2 threads or will be slow for [Context Switching]
> 2. Process Explorer


![](assets/001.png)

# Foreground thread vs Background thread

> Reference: [Foreground and Background Threads](https://docs.microsoft.com/en-us/dotnet/standard/threading/foreground-and-background-threads)

In C#, use [Thread.IsBackground](https://docs.microsoft.com/en-us/dotnet/api/system.threading.thread.isbackground?view=netframework-4.7.2).

If the main program has been aborted, but any foreground thread has not completed, the main program will not be aborted immediately and will waint the foreground thread completes the work.

On the other hand, if the background thread has not completed the work, once the main program stops and all the backgounrd threads will be terminated immediately.


## PLINQ

```
var list = new int[]{1,2,3,4,5}
var plist = from p in a.AsParallel()
            select p;
// The order of plist depends on the assigned schedule of every CPU
```


# Reference

- (Book)Threading and CSharp
- Bechmark .NET
- ILSpy