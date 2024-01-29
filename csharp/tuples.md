# Tuple, ValueTuple

## Differences Between ValueTuple and Tuple

There are 2 types of Tuple types in C#, System.Tuple and System.ValueTuple.

System.Tuple => Reference, members are properties, read-only.

System.ValueTuple = > value, members are fields, not read only.

Very simple Tuple of both types:

```
System.Tuple<Int32> intTup = new Tuple<Int32>(100);
Console.WriteLine (intTup); //display (100)
Console.WriteLine (intTup.Item1); // displays 100

System.ValueTuple<Int32> intTupVal = new ValueTuple<Int32>(100);
Console.WriteLine (intTupVal); //(100)
Console.WriteLine (intTupVal.Item1); //100
```
So what’s the difference?

For starters, trying intTup.Item1 = 200; will give you a compile error. But intTupVal.Item1 = 200; you can do, and it will work.

Tuple types, being reference, are harder on the memory heap and memory allocation, so ValueTuple will give you better performance.

Tuple types and ValueTuple types can be created the same way, BUT, ValueTuple types have one additional way of being created.

Another way to create the Tuple and ValueTuple is with the Create keyword:

```
var intTup = System.Tuple.Create<Int32>(100);
var intTupVal = System.ValueTuple.Create<Int32>(200);
```
For the ValueTuple there is this additional way of creating a new one:

```
(int, int, int) tup1 = (100,200, 300);
var tup2 = (400,500,600);

//to see the type:
Console.WriteLine(tup2.GetType()); 
//System.ValueTuple`3[System.Int32,System.Int32,System.Int32]
```
Also, to access the elements of a Tuple or ValueTuple, we have to go Item1, Item2 etc, which is confusing. But with ValueTuple types we can name the elements:

```
(int first, int second , int third) tup1 = (100,200, 300);
 Console.WriteLine(tup1.first);//100

var tup2 = (first:100, second:200, third:300);
Console.WriteLine(tup2.second); //200
```
Trying to create a ValueTuple this way if it only has 1 value is not going to compile. It works OK with the other ways to create it (already mentioned).

ValueTuple types can hold more elements than Tuple types. If you have a Tuple, you can convert it to ValueTuple with ToValueTuple.

##  Nested Tuple

Because Tuple types are limited as far as number of elements, we may need to have a Tuple instead of another Tuple so we can pass everything we need.

```
var numbers = Tuple.Create(1, 2, 3, 4, 5, 6, 7, Tuple.Create(8, 9, 10, 11, 12));
Console.WriteLine(numbers.Item1); //1
Console.WriteLine(numbers.Rest.Item1); //(8, 9, 10, 11, 12)
Console.WriteLine(numbers.Rest.Item1.Item1);   //8
```
## Comparing ValueTuple, Tuple Types

With ValueTuple types, even if we are giving different names to the elements, what gets compared is the values.

```
(int x, int y) valtup1 = (3,4);
(int i, int j) valtup2 = (3,4);
       
Console.Write(valtup1 == valtup2);//True

```
However, for the reference type Tuple:

```
Tuple<int,int> tup1 = new Tuple<int, int>(3,4);
Tuple<int,int> tup2 = new Tuple<int, int>(3,4);
       
Console.Write(tup1 == tup2);// False
```
## Returning a ValueTuple in a method

Besides returning a type ValueTuple, we can also indicate what names the elements should have.

```
public static (int, int) GetAValueTuple(){
   return (first:20, second:30);
 }

 var v = GetAValueTuple();
 Console.WriteLine(v);
 //Console.WriteLine(v.first); will not find first

 (int x, int y) w = GetAValueTuple();
 Console.WriteLine(w);
 Console.WriteLine(w.x); //will not find first but x is 20

 //BUT, if we name the return and the return type!
public static (int first, int second) GetAValueTuple(){
  return (first:20, second:30);
}
var v = GetAValueTuple();
Console.WriteLine(v.first);//20

//We can also do 
(int x, int y) = GetAValueTuple();
Console.WriteLine(x);//20
Console.WriteLine(y);//30
```

# Get a list of tuples from a list of tuples

```
List<Tuple<int,int>> sublist = originalListWithMoreItems.Select(row => new Tuple<int, int>(row.Item1, row.Item2)).ToList();
```