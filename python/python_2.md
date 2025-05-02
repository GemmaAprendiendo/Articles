# Python 2

# Try - Catch ( or Try Except actually)
```
def main():
    try:
        10/0
    except:
        print("oopps sorry")
        

main()
```
<br>

```
def main():
    try:
        10/0
    except ZeroDivisionError:
        print("oopps bad division")
    except:
        print("I don't know what happened")
        

main()
#oopps bad division
```
<br>

```
def main():
    try:
        10/0
    except ZeroDivisionError as zrdv:
        print(zrdv)
    except:
        print("I don't know what happened")
        

main() #division by zero
```

<br>

```
def main():
    try:
        raise KeyError
    except ZeroDivisionError as zrdv:
        print(zrdv)
    except KeyError:
        print("I came to a KeyError")
        

main() #I came to a KeyError


def main():
    try:
        raise KeyError("helloooo, my own key error")
    except ZeroDivisionError as zrdv:
        print(zrdv)
    except KeyError as ke:
        print(ke)
        

main() #'helloooo, my own key error'
```
<br>

```
def main():
    try:
        10/1
    except ZeroDivisionError as zrdv:
        print(zrdv)
    else:
        print("all good")
        

main() #all good


def main():
    try:
        10/1
    except ZeroDivisionError as zrdv:
        print(zrdv)
    else:
        print("all good")
    finally:
        print("I'm finally")
        

main() 
#all good
#I'm finally
```
<br>

```
def main():
    num = 55

    assert num < 30, "Not good"
main() 

#ERROR!
#Traceback (most recent call last):
#  File "<main.py>", line 5, in <module>
#  File "<main.py>", line 4, in main
#AssertionError: Not good
```

# Classes

```
class Person:
    def eat(self):
        print("I eat")
    def talk(self):
        print("I talk")
        

p1 = Person()
p2 = Person()

p1.eat()
p2.talk()
```
<br>

self is the same as this in C# and other languages

```
class Person:
    def __init__(self, name):
        self._name = name
    def eat(self):
        print(self._name + " I eat")
    def talk(self):
        print(self._name +" I talk")
        

p1 = Person("Jack")
p2 = Person("Mary")

p1.eat()
p2.talk()
```
We can also call the init directly and it will set the name but we should not do this `p1.__init__("hey")`

We can override python functions in a class

```
class Person:
    def __init__(self, name):
        self._name = name
    
    def __str__(self):
        return "I do str my own way"
        

p1 = Person("Mary")

print (str(p1))
# I do str my own way
```
<br>

```
class Person:
    def __init__(self, *names):
        self._names = names
    def theNames(self):
        for n in self._names:
            print(n)
   
        
p1 = Person(["Mary", "Joseph"])
p1.theNames()
#['Mary', 'Joseph']

p2 = Person("Andrew", "Peter")
p2.theNames()
# Andrew
#Peter
```

<br>

We can use `_themethod_` to indicate that a method is private, but doesn't mean we can't call it.

We can call that private method with `self._theprivatemethod_()`
We can also have method that start with set or get and use those to set and get values from the class.