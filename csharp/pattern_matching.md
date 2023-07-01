# Summary of Pattern Matching

With new versions of C# we get new ways to match patterns and it was starting to get confusing to me, so I decided to write myself a summary.

Pattern matching was introduced with C#7. (Not including regular expressions in this article)

## Check object’s type in an if
When we have an if statement, we are now able to have an if depending on whether an object is of the type we are interested in. See the code below.

```
object i = 12; //object assigned an int
object s = "hello"; //object assigned a string
if (i is int x)//if the object i is int, it will be assigned to x
{
   Console.WriteLine(x);//will happen
}
if (s is int xx)
{
   Console.WriteLine(xx);//will not happen since s is not an int
}
if (s is string  xxx)
{
   Console.WriteLine("string " + xxx);//will happen
}
```

## Switch Cases Without Literal Values

We don’t need to have literal values on the case statements of a switch. Look at this code:

```
//We have 2 different classes
public class AClass
{
   public string? s { get; set; }
}
public class AClassToo
{
   public string? s { get; set; }
}
//we are going to assign instances of the classes to tupe objects
Object obj = new AClass();
((AClass)obj).s = "Hello Class";
Object obj2 = new AClassToo();
((AClassToo)obj2).s = "Hello Class Too";
//now assign one of those class objects to another object that we will use in the switch
Object theObj = obj;
//now in the switch (ac ir whatever name you want)
switch(theObj)
{
   case AClass ac when ac.s == "Hello":
      Console.WriteLine("class hello");
      break;  
   case AClass ac when ac.s == "Hello Class":
      Console.WriteLine("class hello class");
      break;
   case AClassToo ac2 when ac2.s == "Hello":
      Console.WriteLine("class 2 hello");
      break;
   case AClassToo ac2 when ac2.s == "Hello Class Too":
      Console.WriteLine("class hello Too");
      break;
   default:
      break;
}
//Will print class hello class because theObj is a AClass with s as "Hello Class"
//if we use the same code but have the assigment 
//Object theObj = obj2; instead (AClassToo type).
//it will print class hello too.
```
## Switch Statements with Switch Expressions

Starting in C# 8.

Switch expressions use => to indicate a return value. It makes the code shorter.

Similar to the previous switch but not the same because this type of switch returns a value for each case:
```
//assign the switch to a variable
string message = theObj switch
{
   AClass => "class hello", //if AClass return this string
   AClassToo => "Hello Class Too",
   null => "null",
   _ => "some other thing", //any other cases, return this
};
Console.WriteLine(message);
```
No case or break; keywords here.

To make it more similar to the original one:
```
string message = theObj switch
{
   AClass ac when ac.s == "Hello" => "class hello",
   AClass ac when ac.s == "Hello Class" => "class hello class",
   AClassToo act when act.s == "Hello"=> "classToo hello",
   AClassToo act when act.s == "Hello Class Too" => "classToo hello too",
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```
If you want to match on the class only, without checking any properties, we can do this:

```
string message = theObj switch
{
   AClass _ => "class hello",
   AClassToo _ => "classToo hello",
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```
## Switch Statements C#9

C#9 makes switch statements even more fun with nested statements and the support for > comparisons.

The example we had before, checking for the value of s, would be something like this with the nested expressions.

Notice that we don’t have == in the checking of the s value.

```
string message = theObj switch
{
   AClass ac  => ac.s switch //if this class, switch for s checking
   {
      "Hello" => "class hello",
      "Hello Class" => "class hello class",
      _ => "some other thing AClass",
   },
   AClassToo act => act.s switch
   {
      "Hello" => "classToo hello",
      "Hello Class Too" => "classToo hello too",
      _ => "some other thing AClassToo",
   },
   null => "null",
   _ => "some other thing",
};
```
If we were comparing amounts, we could be using something like:

```
Object obj3 = new AClass3();
((AClass3)obj3).i = 15;
string message = theObj switch
{
   AClass3 ac3  => ac3.i switch
   {
      > 5  => "> 5",
      _ => " <= 5",
   },
   null => "null",
   _ => "some other thing",
};
Console.WriteLine(message);
```
## Matching classes contents with Deconstruct

Imagine we have a class that has some data like this:

```
public class AClass4
{
   public int? I { get; set; }
   public string? S { get; set; }
   public AClass? ACLASS { get; set; }
   public void Deconstruct(out int? i, out string? s)
   {
      i = I;
      s = S;      
   }
}
```
We could have a switch like this:
```
var result = four switch
{
   AClass4(30, "how are you?") => "how are you 30",
   AClass4(25, "hey" ) => "hey 25",
   _ => "who?"
};
Console.WriteLine(result);
```
We can have the switch above because of the Deconstruct method we have in the class. We are using that method to do the comparison with the “four” instance of the class we are using in the switch.

If we have the class like this:
```
AClass4 four = new AClass4();
four.I = 30;
four.S = "how are you?";
four.ACLASS = new AClass();
four.ACLASS.s = "I am here";
```
We will get “how are you 30”. If any of the values is different to those passed, we will get “who?” or “hey 25” if it matches. Notice we are not checking the ACLASS of four because it’s not part of the Deconstruct method. We can include it.

With the same instance as above, but with the Deconstruct like this:
```
public void Deconstruct(out int? i, out string? s , out AClass? f)
{
   i = I;
   s = S;
   f= ACLASS;
}
```
We can have a switch checking with options when the class is null or not:
```
var result = four switch
{
   //if the AClass in the AClass4 is null
   AClass4(30, "how are you?", null ) => "how are you 30",
   //if the AClass in the AClass4 is set 
    AClass4(30, "how are you?", _  ) => "hey 30",
   _ => "who?"
};
Console.WriteLine(result);
//the above prints hey 30
```
If we want to also check on some properties of the inner class, we could also have something like this:
```
public void Deconstruct(out int? i, out string? s , out string? fs)
{
   i = I;
   s = S;
   fs= ACLASS.s;
}
var result = four switch
{
   AClass4(30, "how are you?", null ) => "how are you 30",
   AClass4(30, "how are you?", "I am here") => "hey 30 with class I am here",
   AClass4(30, "how are you?", _  ) => "hey 30",
   _ => "who?"
};
Console.WriteLine(result);
```
A Deconstruct method is usually used like this (for the same class we have been using)
```
var (ii, ss, cc) = four; //use the var to get the parts from the class
Console.WriteLine(ii);//ii will be the I from "four"
Console.WriteLine(ss); // S
Console.WriteLine(cc); //ACLASS.s
```
## Some Additional Switch Possibilities
We can also do this to check on some part of the class inside the class we are checking;
```
var result = four switch
{
   { ACLASS: { s:"I am here" } }  =>  "I am here",
   _ => "who?"
};
```
Adding another string to that inner class.
```
AClass4 four = new AClass4();
four.I = 30;
four.S = "how are you?";
four.ACLASS = new AClass();
four.ACLASS.s = "I am here";
four.ACLASS.s2 = "not me";
//this switch now will display who?
var result = four switch
{
   { ACLASS: { s:"I am here", s2:"me too" } }  =>  "I am here",
   _ => "who?"
};
//changing the class to have four.ACLASS.s2 = "me too" will display "I am here"
```
If we want to check not just on properties of the inner class but also the main class (four):
```
var result = four switch
{
   {S:"how are you?" ,ACLASS: { s:"I am here", s2:"me too" } }    =>  "I am here",
   _ => "who?"
};
```
To check for the class being {ACLASS: null}

We can also use Tuples. This example is from the MS docs (not the whole thing):
```
var newState = (state, operation, key.IsValid) switch
{
   (State.Opened, Operation.Close, _)      => State.Closed,
   (State.Opened, Operation.Lock, true)    => State.Locked,
   (State.Locked, Operation.Open, true)    => State.Opened,
   ...                        
   _ => state
};
```





