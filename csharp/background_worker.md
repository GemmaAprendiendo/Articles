# How to Use a Background Worker

Just a summary of how to work with one of these so you can have code doing things in the 
background while the window is still responsive.

## Add the control

You can drag and drop the background worker from the toolbox.

## Set it up

In the constructor, you can add these lines:

bg1 = new BackgroundWorker();
bg1.WorkerReportsProgress = true;
bg1.WorkerSupportsCancellation = true;
bg1.DoWork += new System.ComponentModel.DoWorkEventHandler(bg1_DoWork);
bg1.ProgressChanged += new System.ComponentModel.ProgressChangedEventHandler(gb1_ProgressChanged);
bg1.RunWorkerCompleted += new System.ComponentModel.RunWorkerCompletedEventHandler(bg1_RunWorkerCompleted);

## Using the events above

Somewhere in your code (button probably), you need the code that should start the calculations that need to be done in the background.  You do it this way:

```
if (!bg1.IsBusy)
{
   bg1.RunWorkerAsync();  //this will call the DoWork
}
```

Since we said we are allowing cancellation, in a cancel button or wherever you want to cancel from

```
if (bg1.IsBusy)
{
  bg1.CancelAsync();
}
```

You will have the code that needs to run in the background here:

```
private void bg1_DoWork(object sender, DoWorkEventArgs e)
{
  BackgroundWorkfker worker = sender as BackgroundWorker;

  //your code here

  //when you need to report back so something can be shown in the screen

  worker.ReportProgress(1, "some string you want to send back, could be any object")  ;

  //in your code, you should check for this regularly to check if the user cancelled
  if (worker.CancellationPending)
  {
    e.Cancel = true;
    return;
  }
}
```

How to handle the progress being sent

```
private void bg1_ProgressChanged(object sender, ProgressChangedEventArgs e)
{
  e.UserState.ToString());//UserSate has the object you passed in.
}
```

to do something once the DoWork has completed:
```
private void bg1_RunWorkerCompleted (object sender, RunRowrkerCompletedEventArgs e)
{
  if (e.Cancelled)
  {
    //what do you want to do if you got here because they cancelled?
  }else
  {
    //what do you want to do if you got here because DoWork finished?
  }
}

```

That's all you need.