# Python Syntax Cheatsheet

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
```
```
amount = 48.34555

print(f"The amount is {amount}");
formatted_amount = "{:.2f}".format(amount); #48.34555
print(f"The amount is {formatted_amount}"); #48.35
```
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


