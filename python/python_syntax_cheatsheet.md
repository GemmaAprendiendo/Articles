# Python Syntax Cheatsheet (basic)

## Prints

`print("...");`

`myInput = input("...");`

`print("hello " + input("who are you?"));`

`#this is a comment`

`len("some string:);`

```
print("the question")
answer = input()
```
`print(type("hello"));`

```
stringIs = str(123);
print(type(stringIs));
#prints <class 'str'>
```

Concatenating a string to an int in python will not cast the int into a string, it will just fail.

```
print(float(123));
#prints 123.0
```
Anything coming from the input is a string.

6/3 will return 2.0 not 2

```
print(int(12.56));
#returns 12
print(int(154.123));
#returns 154
```
`print(int(2**3));  #returns 8`

```
print(round(8/3));
#2.6666 but it will round to 3

print(round(10/3));
#3.33333 but will round to 3

print(round(10/3, 3));
#will round to those decimal places: 3.333

print(8//3);
#2.6666 but will floor to just the int part, 2

print(1, 2, 3, sep=' < ')
#will print 1 < 2 <3 (the default is space)

print (round(115.52, -2)) # set the last 2 digits before . to 0 --> 100.0
print (round(15.52, -2)) # set the last 2 digits before . to 0 --> 0.0
print (round(1.52, -2)) # set the last 2 digits before . to 0 --> 0.0
print (round(1115.52, -2)) # set the last 2 digits before . to 0 --> 1100.00
print (round(1115.52, -3)) # set the last 3 digits before . to 0 --> 1000.0

```
```
amount = 48.34555

print(f"The amount is {amount}");
formatted_amount = "{:.2f}".format(amount); #48.34555
print(f"The amount is {formatted_amount}"); #48.35


print("Splitting", total_candies, "candy" if total_candies == 1 else "candies")
```

## conditionals 

```
number = input("give me a number: ");
if (int(number) > 5):
    print(">5");
else:
    print("<5");
```

```
number = input("give me a number: ");
if (int(number) > 10):
    print(">10");
elif (int(number)> 5):
    print(">5 and <10");
else:
    print("<10 and <5")
```
```
number = input("give me a number: ");
if (int(number) > 10 and int(number)%2 == 0):
    print(">10 and even");
else:
    print("<10 or >10 but odd");

#use or for and or
```

## imports

to import something, such as the random module `import random`, then use its functions with `random.whateverfunc`

For the modules, we can just name the file and import that directly, and use the things it has inside.

`random() ` --> 0 to 0.9999 float number
`ranint(1,10) ` --> 1 to 10

## arrays, lists

```
someArray = ["one", "two", "three", "four", "five"];
print(someArray[0]); #one
print(someArray[-1]) #five
```

```
someArray.append("six");
print(someArray);
```

`someArray.extend(["seve", "eight"])`

```
str = "hello how are you?"

array = str.split(" ");
print(array)
```

```
import random;

array = ["Anna","George", "Michael", "Maria"]

randomname = random.choice(array);
print(randomname)
```

```
array = [1,2,3,4,5]
array2 = [6,7,8,9,0]

both = [array, array2]
print(both) # prints [[1, 2, 3, 4, 5], [6, 7, 8, 9, 0]]

print(both[0]) # prints [1, 2, 3, 4, 5]

print(both[0][2]) # prints 3
```

```
fruits = ["apple", "manzana", "fresa"]

for fruit in fruits:
    print(fruit)
```
In the for, everything indented will be part of the loop (no {} needed, but make sure the indentation is right)

```
scores = [6,6, 10,8]

print(max(scores)) #10
```

The elements don't have to be the same type.  This is a valid one `[32, 'raindrops on roses', help]`

To find the first 3 elements of a list : `planets[0:3]` Leaving our the 0 and doing only `:3` it assumes 0.
Leaving out the second one assumes until the end `planets[3:]`

`planets[1:-1]` --> not index 0, and not index -1, which means the last one.

`planets[-3:]` --> from the third one from the end, to the last one

sort a list: `sorted(thelist)`

add the elements: `sum(somelistwithnumbers)`

append appends an element at the end.  Pop removes the last element.  It also returns it.  We can get the index of an element by `thelist.index('thevalue')`;

Before trying to get the index, check if the element is in the list with `'TheValue' in TheList`

If we need to see everything available for a list type, we can do `help(thelist)`

## Lists vs Tuples

tuples are created with () instead of [], and they are immutable.

```
t = (1, 2, 3)

t = 1, 2, 3
```

if we have a function returning a tuple, we can assign the result to different variables `value1, value2 = x.someFunctionReturningTuple()`

**Swap 2 variables:**

```
a = 1
b = 0
a, b = b, a
print(a, b)
```

## List comprehensions

```
thearray = []
# range will create 10 iterations
for n in range(10):
    thearray.append(n**2)
print(thearray) # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

The above thing can be done this way
```
thearray = [n**2 for n in range(10)]
print(thearray)
```
Could also add an if in there

```
thearray = [number for number in range(10) if number*2 < 10]
print(thearray) # [0, 1, 2, 3, 4]

# similar to a select from where
```

Could have also done 

```
print ([32 for numbers in range(9)]) 
#[32, 32, 32, 32, 32, 32, 32, 32, 32]
```
## Loops
```
for number in range(1, 15):
    print(number) #prints 1 through 14

#range with stepping
for number in range(1, 15, 2):
    print(number) #prints 1 3 5 7 9 11 13
```
```
number = 0
while number < 10:
    print(number)
    number = number+1
```

## Functions
```
def myFunc():
    print("I")
    print("am")
    print("function")

myFunc()
```


```
def least_difference(a, b, c):
    """
    write something here
    that explains the function
    """
    diff1 = 1
    diff2 = 2
    diff3 = 3
    return min(diff1, diff2, diff3)

```
if we do the above, when we call help(least_difference) we will get the text in triple quotes as help.

Functions that don't return anything will return `None`.

### Functions - break, continue, pass
`Break` and `continue` work as in other languages.
`pass` is just used when we just have a comment inside the for or function or whateverw, so the code compiles with just the comment. Otherwise we need to put some code line in there, so we use `pass`

## Functions continued

```
def myFunc(name):
    print(name)
    print(f"the name is {name}")

myFunc("Jay")

def myFunc(name, lastName):
    print(f"the name is {name}")
    print(f"the last name is {lastName}")

myFunc("Jay", "Smith")
myFunc( lastName="Smith", name="Jayleen")

def greet(who="Colin"):
    print("Hello,", who)
#default argument

greet()
greet(who="Kaggle") #could specify the name of the argument, useful maybe if many


max(100, 51, 14, key=some_function_you_have) 
# will run the funtion you passed as key on those numbers, and apply the max to the results from the function.
```

You can pass other functions as arguments to your funtion.  These are higher-order functions (the ones that get the functions as parameters, not the ones that are passed along)

Take a range from an array/list

```

array = [1,2,3,4,5]

print(array[1:3])

#print [2, 3]

```

Go all the way to the end but skip every second one

```
array = [1,2,3,4,5,6,7,8,9]

print(array[1::2])

#[2, 4, 6, 8]
```

unpacking a list
```
a,b,c = [1,2,3]

print(a)#prints 1

a,*b = [1,2,3]

print(b)#prints 2,3

a,*b, c = [1,2,3,4,5]

print(c)#prints 5


```

Set - unordered collection of unique objects

`mySet = {1,2,3,4,5}`

ternary operator

```
#return this string if this condition, else return this other string

message = "0>1" if (0>1) else "1>0"
print (message)
#prints 1>0
```

`is` is not the same as equal.  IS will compare memory locations for equality, not the values in them.  However, if we compare 1 to 1, they will be stored in the same memory because they are so simple, so `is` will return true.  This works for simple types. 2 lists with the same elementes will still have different memory locations.



## enumerate

```
for i, char in enumerate("hellooooo"):
    print(i, char)
```
prints:
```
0 h
1 e
2 l
3 l
4 o
5 o
6 o
7 o
8 o
```



Info for function in python comes between trippe quotes (''')

```
def hello():
    '''
    this is a hello function
    '''
    print("hello")

hello()
```

```
def func(*args): #accet any num of arguments with the *
    print (*args) #1 2 3 4 5
    print(args) #(1,2,3,4,5)
    return sum(args) #15
    
print(func(1,2,3,4,5))


def func(*args, **kwargs): #accept any num of arguments with the *
    print (*args) #1 2 3 4 5
    print(args) #(1,2,3,4,5)
    print(kwargs)
    return sum(args) #15
    
print(func(1,2,3,4,5, num1=5, nuym2=10))
```

If we have different types of parameters the order matters

in **Kaggle notebooks**: Add a new code cell by clicking on an existing code cell, hitting the `escape` key, and then hitting the `a` or `b` key. The a key will add a cell above the current cell, and b adds a cell below.

Also on those notebooks, you'll call q1.check(), q1.hint(), q1.solution(), for question 2, you'll call q2.check(), and so on. (to get the code checked, or get a hint, or see the solution)

## string and number , operations

```
viking_song = "Spam " * spam_amount
print(viking_song)
```
shows : spam spam spam spam

```
print(min(1, 2, 3))
print(abs(-32))
print(float(10))
print(int('807') + 1)
```

## swap variables
```
a = [1, 2, 3]
b = [3, 2, 1]

a, b = b, a
```

## strings and dictionaries

all strings are treated as true, except the empty string ""

print() adds the new line at the end unless we say otherwise
```
print("Try programiz.pro")
print("hey")
print("no new line - ", end="")
print ("hey") #no new line - hey
```
```
thestring = "hello"
print([char for char in thestring]) #['h', 'e', 'l', 'l', 'o']
```
As opposed to lists, strings are **immutable**.

```
longsentence = "hello how are you"
arraywiththewords = longsentence.split()
print(arraywiththewords) #['hello', 'how', 'are', 'you']

longsentence = "hello-how-are-you"
arraywiththewords = longsentence.split('-')
print(arraywiththewords) #['hello', 'how', 'are', 'you']
```
```
longsentence = "@@".join(["how", "are", "you?"])
print(longsentence) #how@@are@@you?
```
```
print (str(10))
```
```
print ("{}, you'll always be the {}st planet to me.".format("You", 1))
#You, you'll always be the 1st planet to me.
```
```
somestring.isdigit() #whether all charaters are digits
```

Some harder things with strings
```
def word_search(doc_list, keyword):

	list_of_indexes = [] #array will be populated with indexes

	#use for with index and word so we can access the index
	#casefold to ignore the casing without using upper or lower
	for index, word in enumerate(doc_list):
		splitted_in_words = word.split()
		if ( keyword.casefold() in map(str.casefold, splitted_in_words) 
			or (keyword+".").casefold() in map(str.casefold, splitted_in_words) 
			or (keyword+",").casefold() in map(str.casefold, splitted_in_words)) :
				list_of_indexes.append(index)
				
	 return list_of_indexes
```
```
#dictionary
numbers = {'one':1, 'two':2, 'three':3}
print(numbers['two']) #2

newdic = {number: number*2 for number in range(10)}
print(newdic) #{0: 0, 1: 2, 2: 4, 3: 6, 4: 8, 5: 10, 6: 12, 7: 14, 8: 16, 9: 18}
```

## Other

Get help about a function: `help(round)`

all numbers are treated as true, except 0



```
def exactly_one_topping(ketchup, mustard, onion):
    """Return whether the customer wants exactly one of the three available toppings
    on their hot dog.
    """
    pass
    return (ketchup + mustard + onion) == 1
	
return (int(ketchup) + int(mustard) + int(onion)) == 1
```

we can return `return None`, not the string "None", just None.