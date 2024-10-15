# Threading Summary

## Threads
Threads will be run to completion, so an application will not end while it has threads running. You can change the priority of a thread for as long as the thread is active. You can also abort a thread, but resources may be left stuck.

A Thread cannot deliver a result to another thread, but it can take in data.

Exceptions happening in a thread should be caught inside the thread.

Let’s start with a basic example. The thread will wait for 3 secs, then print something to the console. The main method will continue and print “In Main” without waiting for the thread, which is running in …. a different thread :-) :

```
using System;
using System.Threading;

class Program {      
    
    public static void Main(string[] args) {           
        Thread th = new Thread( () => 
        { 
            Thread.Sleep(3000);
            Console.WriteLine("In Thread"); 
        });
        
        th.Start();
        Console.WriteLine("In Main");
    }
}

//will display
In Main
In Thread
```
But what if we want to tell the thread what to print? We can set the parameter for the thread to have some data and do something with that data, and then for creating the thread we just need to pass that.

In the following example, the parameter passed has to be object, not your own object type. Also, do not call with an anonymous type that matches the MyObject because that will not work.

```
public class MyObject{
  public int j {get;set;}
  public int k {get;set;}
}
    
public static void Main(string[] args) {           
  ParameterizedThreadStart p = new ParameterizedThreadStart((object o) => { 
    MyObject obj = (MyObject) o;
    Console.WriteLine($"Got {obj.j} and {obj.k} to get {obj.j + obj.k}");
   });   
        
   Thread th = new Thread(p);
   MyObject mo = new MyObject();
   mo.j = 10; mo.k = 20;
   th.Start(mo);
 }
 ```
 You can do the same by creating the method (instead of using the anonymous one) and passing that method. Keep in mind it still has to have object in its parameter, so it matches what ParameterizedThreadStart expects.

 ```
 public class MyObject{
  public int j {get;set;}
  public int k {get;set;}
}
    
public static void MyMethod(object o){
  MyObject obj = (MyObject) o;
  Console.WriteLine($"Got {obj.j} and {obj.k} to get {obj.j + obj.k}");
}

public static void Main(string[] args) {           
  ParameterizedThreadStart p = new ParameterizedThreadStart(MyMethod);   
        
  Thread th = new Thread(p);
  MyObject mo = new MyObject();
  mo.j = 10; mo.k = 20;
  th.Start(mo);
 }
 ```
 We could have also done this:

 ```
 public class MyObject{
  public int j {get;set;}
  public int k {get;set;}
}
    
public static void Main(string[] args) { 
        
  Thread th = new Thread( (data) => {
    MyObject obj = (MyObject) data;
    Console.WriteLine($"Got {obj.j} and {obj.k} to get {obj.j + obj.k}");
  } );
  MyObject mo = new MyObject();
  mo.j = 10; mo.k = 20;
  th.Start(mo);
}
```
We have seen that when we start a thread, Main continues processing without waiting. The whole point of threads is that we are not waiting, but, what if we want to wait? with Join()

If we add Join() to the code we showed before:

```
static void Main(string[] args) {            
  Thread th = new Thread(() => {
  Thread.Sleep(3000);
  Console.WriteLine("In Thread"); 
  });

  th.Start();
  th.Join(); //JOIN and it will print In Thread, In Main.
  Console.WriteLine("In Main");
}
```
If we are handling various threads, a thread can be waiting on another thread too.

```
static void Main(string[] args) {            
  Thread th_1 = new Thread(() => {
    Thread.Sleep(3000);
    Console.WriteLine("th_1"); 
  });
        
  Thread th_2 = new Thread(() => {             
    th_1.Join();//JOIN THE OTHER THREAD
    Console.WriteLine("th_2"); 
  });
        
  th_1.Start();
  th_2.Start();
}
```
Like I said before, you cannot pass data from one thread to another. You could be accessing the same variables, but in that case, you should lock those variables.

```
using System;
using System.Threading;

class Program {      
    public static string _something = string.Empty;
    public static object _lock = new object();
                    
    static void Main(string[] args) { 

        Thread th2 = new Thread( () => { 
            lock(_lock){
                _something = DateTime.Now.ToString();  
            }
        });
                    
        Thread th1 = new Thread( () => {     
            th2.Join();
            Console.WriteLine(_something); });
                            
        th2.Start(); //start this one first
        th1.Start();
                        
    }
}
```
## Tasks and Running in Parallel

A Task is an object that represents a job (method) that’s going to be performed.

Tasks are created as background processes, so they can be ended even before completion if the foreground processes complete. Tasks do not have a priority.

Here we are creating 2 task actions and then starting them with Parallel.Invoke

```
using System;
using System.Threading;
using System.Threading.Tasks;

class Program {      

    static void Main(string[] args) {  
        Action task1 = new Action( () => {
            Thread.Sleep(2000);
            Console.WriteLine("task1");
        });
        Action task2 = new Action( () => Console.WriteLine("task2"));
        Parallel.Invoke (task1, task2);
    }
}

//displays
//task2
//task1
```
Could have also done this:

```
static void Main(string[] args) {                 
  Parallel.Invoke ( 
    () => { Thread.Sleep(2000); Console.WriteLine("task1");}, 
    () => Console.WriteLine("task2")
   );
 }
 ```
 So far we have created tasks by having some actions (could be methods) and starting them with Parallel.Invoke. We can also just use Task to do that.

We can have these methods:

```
public static void Method11()
{
  Console.WriteLine("method 1");
}
public static void Method22()
{
  Console.WriteLine("method 2");
}
public static void Method33()
{
  Console.WriteLine("method 3");
}
```
And we can create Tasks for those methods with this:
```
//START will be first, but the order of the other ones we don't know.
Console.WriteLine("START");

Task T1 = new Task(Method11);
T1.Start();

Task T2 = Task.Factory.StartNew(Method22);

Task T3 = Task.Run(Method33);

Console.WriteLine("END");
```
You can wait on a task, like this. Here you will not see END until you see “I am the new task”.
```
Console.WriteLine("START");

Task newTask = new Task(() => { 
  Thread.Sleep(2000); Console.WriteLine("I am the new task"); });

newTask.Start();
newTask.Wait();

Console.WriteLine("END");
```
You could also wait for an array of tasks with Task.WaitAll(array or list of tasks to wait on).

Tasks can return a result.
```
Task<String> newTask = Task.Run(() => { 
  Thread.Sleep(2000); return ("I am the new task"); });
newTask.Wait();
Console.WriteLine(newTask.Result); //I am the new task 
```
We can also continue one task with another one with ContinueWith.

```
Console.WriteLine("START");

Task<String> newTask = Task.Run(() => { 
  Thread.Sleep(2000); return ("I am the new task"); });  

  newTask.ContinueWith((prevTask) =>
   {
    Console.WriteLine("I continue from newTask and I print its result");
    Console.WriteLine("Result of new task: " + prevTask.Result);
    return "I am the 2nd task";
   }).ContinueWith( (prevTask2) =>
   {
    Console.WriteLine("I am the 3rd task and I print the 2nd's one result");
    Console.WriteLine("Result of 2nd task: " + prevTask2.Result);
   }).Wait();
  
  Console.WriteLine("END");

//displays
//START
//I continue from newTask and I print its result
//Result of new task: I am the new task
//I am the 3rd task and I print the 2nd's one result
//Result of 2nd task: I am the 2nd task
//END
```
A parent task can create children tasks. Children can be attached (real children) or not (called nested). If they are attached, the task will not complete until the children are done.

```
Task parent = Task.Factory.StartNew(() =>
{
   Console.WriteLine("parent task started");

   Console.WriteLine("starting indepdent nested task");
   Task.Factory.StartNew(() => { Thread.Sleep(4000); Console.WriteLine("Indep nested task after 4 secs"); });

   Console.WriteLine("starting child task - attached");
   Task.Factory.StartNew(() => { Thread.Sleep(2000); Console.WriteLine("attached child after 2 secs"); }, TaskCreationOptions.AttachedToParent);

   Console.WriteLine("parent child ending");

});

parent.Wait(); //Wait for attached
Console.WriteLine("Parent Done Waiting ");
```
## Async/Await

You can wait for a task to finish, but you wait for a task that returns something. You cannot do this.

```
Task t = new Task(() => { Console.Write("hey");  });

await t.Start();// WIll not compile
```
But this will work (await should be used in a method that is marked async).
```
Console.WriteLine("START");

Task t = new Task(() => { Console.Write("hey");  });

await  Task.Run(() => { Thread.Sleep(2000); Console.WriteLine("Running Task"); return "hello"; });    

Console.WriteLine("END");

//displays
//START
//Running Task
//END
```
## Some Parallel Operations

Just like we have a foreach, we also have a parallel foreach. It will run whatever we want to do for each item in parallel, but there is no guarantee about the order in which things will happen.

```
List<string> l = new List<string>();

l.Add("1");
l.Add("2");
l.Add("3");
l.Add("4");
l.Add("5");

Parallel.ForEach(l, (item) => Console.WriteLine(item));

//could print
//1
//4
//5
//3
//2
//or some other order
```
We have the same option with a for loop (again no guarantee on the order).

```
//Notice that the 0 is inclusive, but the 2nd argument is EXCLUSIVE, 
//so don't subtract 1.
Parallel.For(0, l.Count, (i) => Console.WriteLine(i + "-" + l[i]));
```
We can also add a stop condition to the for. For the same list as above:

```
ParallelLoopResult res = Parallel.For(0, l.Count,
            (int i, ParallelLoopState st) => {
             Console.WriteLine(i + "-" + l[i]);
             if (i == 2) { st.Stop(); }
            });

  Console.WriteLine(res.ToString());

//displays
//0-1
//2-3
//1-2
//System.Threading.Tasks.ParallelLoopResult
```
ParallelLoopState can also be added with the foreach one.

We can also use linq with parallel programming.

```
var res = from item in l.AsParallel()
    where (item == "2" || item == "4")
    select item;

```
The processing order is not guaranteed, but, you can do .AsParallel().AsOrdered() to preserve the order.

You can also do .AsParallel().AsSequential() to process sequentially.

We can also start the iteration among the results before it has completed, if we do this: res.ForAll(p => { Console.WriteLine(p.Age); });

If any queries throw exceptions, they will be thrown as an AggregateException at the end.

## Additional

If you are getting a cross-thread operation error, which will happen when the control you are trying to 
access has theControl.InvokeRequired is true, you can do something like this:
```
theControl.Invoke(new MethodInvoker(delegate { do whatever to the control here}));
```


Or same as above in other words:

### Threading and the UI

You can't update the UI from a thread that is not the one where the UI is running.  You can do something like this though:
```
TupleOrWhateverType varname = await SomeAsyncMethod();
YouCanSetAVariableTovarname = varname;
//for accessing the UI
comboboxorwhateverUIelement.Invoke(new MethodInvoker(delegate 
   {comboboxorwhateverUIelement.SelectedIndex = (int) varname;}));

//SelectedIndex or whatever property.
```

More on this.

There are two ways to safely call a Windows Forms control from a thread that didn't create that control. 
Use the System.Windows.Forms.Control.Invoke method to call a delegate created in the main thread, which in turn calls the control. 
Or, implement a System.ComponentModel.BackgroundWorker (see BackgroundWorker article).

The System.Windows.Forms.Control.InvokeRequired property, which compares the control's creating thread ID to the calling thread ID. 
If they're different, you should call the Control.Invoke method.

```
public void WriteTextSafe(string text)
{
    if (textBox1.InvokeRequired)
    {        
        textBox1.Invoke("updated from invoke required");
    }
    else
        textBox1.Text = "normal update";
}
```

Note:
Delegate.Invoke: Executes synchronously, on the same thread.
Delegate.BeginInvoke: Executes asynchronously, on a threadpool thread.

Control.Invoke: Executes on the UI thread, but calling thread waits for completion before continuing.
Control.BeginInvoke: Executes on the UI thread, and calling thread doesn't wait for completion.

You could have code like this so you don't have to be doing the above code over and over
```
public static void InvokeIfRequired(this Control control, MethodInvoker action)
{   
    if (control.InvokeRequired) {
        control.Invoke(action);
    } else {
        action();
    }
}

whateverControl.InvokeIfRequired(() =>
{
    // Do anything you want with the control here
    whateverControl.RtfText = value;    
});
```

## Exceptions in Async

You need the try / catch block in the Task code itself, not on the method starting the task.

We can also use a continuation task (see above for ContinueWith) to handle an exception, since the task 
scheduler will schedule the continuation when it detects that the previous task threw and unhandled
exception.

We can have the try / catch in the main method or whatever method using the task if we have a task.Wait,
the catch would be catching an AggregateException.

