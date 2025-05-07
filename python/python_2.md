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

We can set properties in a class this way:

age = property(fget=some method, fset = some other method)

```
class C1:
    
    def set_age(self, age):
        if age > 30:
            raise ValueError("no I said 30 or younger")
        self._age = age
        
    def get_age(self):
        pass
    
    age = property(fget=get_age, fset = set_age)
    

c = C1()

c.age = 35
```
# Inheritance

```
class Car:
    def __init__(self, type):
        self._type = type
    
    def WhatAmI(self):
        print("I'm a " + self._type)
        

c1 = Car("sedan")
c1.WhatAmI()

#I'm a sedan
```
<br/>

Bellow, we have class Car and class Make, which inherits Car.  Once we define a constructor `(__init__)` for the Make class, the one in the Car class (ancestor) will not be called, but we can call it in the Make's constructor.

Then from Make we can call the methods from Make and from Car
```
class Car:
    def __init__(self, type):
        self._type = type
    
    def WhatAmI(self):
        print("I'm a " + self._type)
        
class Make(Car):
    
    def  __init__(self, brand, type):
        self._brand = brand
        Car.__init__(self,type)

c1 = Make("Lexus", "sedan")
c1.WhatAmI()

#I'm a sedan
```
We can also inherit from the Exception class to create our own Exception classes.
To override a method in the parent class, all we have to do is have the same method in the child.
In Python you cannot have the same method with different arguments in the same class (or hierarchy), this type of method overloading doesn't work in Python.

In the example above, another way we could have called the parent's constructor could have been

```
class Car:
    def __init__(self, type):
        self._type = type
    
    def WhatAmI(self):
        print("I'm a " + self._type)
        
class Make(Car):
    
    def  __init__(self, brand, type):
        self._brand = brand
        super().__init__(type) #--> we are not passing self if using super

c1 = Make("Lexus", "sedan")
c1.WhatAmI()

#I'm a sedan
```
<br/>

```
class Car:
    name = "some name"
    
    def __init__(self, n):
        self.name = n
    
    def TheName(self):
        print(self.name)
        


c1 = Car("4Runner")
c2 = Car("Land Cruiser")

c1.TheName() #4Runner
c2.TheName() #Land Cruiser

```
<br/>

BUT!, set it with the class name instead of self ...and it's a static variable
```
class Car:
    name = "some name"
    
    def __init__(self, n):
        Car.name = n #set it with the class name instead of self
    
    def TheName(self):
        print(Car.name)
        


c1 = Car("4Runner")
c2 = Car("Land Cruiser")

c1.TheName()  #Land Cruiser
c2.TheName() #Land Cruiser
```

<br/>
Use @classmethod to mark a method as static, and return the class variable or whatever.  Call the method with the  class name.  Instead of declaring the class method with `(self)` m so it with `cls`

```

class Car:
    name = "some name"
    
    def __init__(self, n):
        Car.name = n #set it with the class name instead of self
    
	@classmethod
    def TheName(cls):
        return Car.name

c1 = Car("4Runner")
c2 = Car("Land Cruiser")

print(c1.TheName())  #Land Cruiser
print(c2.TheName()) #Land Cruiser

```

COULD HAVE DONE ` return cls.name` instead of `return Car.name`

You can have multiple inheritance, on inheriting from another and from another, or different classes inheriting from the same one.  If they override a specific method, how is Python looking to find it?

```
class C1:
    
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C1 it is")

class C2(C1):
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C2 it is")
        
#class C3(C1, C2):
#    def __init__(self):
#        pass

c1 = C1()
c2 = C2()
#c3 = C3()

c1.TheMethod() #C1 it is
c2.TheMethod() #C2 it is
#c3.TheMethod()
```
<br/>

BUT
```
class C1:
    
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C1 it is")

class C2(C1):
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C2 it is")
        
class C3(C1, C2):
    def __init__(self):
        pass

c1 = C1()
c2 = C2()
c3 = C3()

c1.TheMethod() 
c2.TheMethod() 
c3.TheMethod()

#ERROR!
#Traceback (most recent call last):
 # File "<main.py>", line 16, in <module>
#TypeError: Cannot create a consistent method resolution
#order (MRO) for bases C1, C2

```

Doesn't know what to do.  However

```
class C1:
    
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C1 it is")

class C2(C1):
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C2 it is")
        
class C3(C2):
    def __init__(self):
        pass

c1 = C1()
c2 = C2()
c3 = C3()

c1.TheMethod() #C1 it is
c2.TheMethod() #C2 it is
c3.TheMethod() #C2 it is

```
We could have done this to see how it would look for the method in the chain.

```
class C1:
    
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C1 it is")

class C2(C1):
    def __init__(self):
        pass
    
    def TheMethod(self):
        print("C2 it is")
        
class C3(C2):
    def __init__(self):
        pass

print(C3.mro())  

#[<class '__main__.C3'>, <class '__main__.C2'>, <class '__main__.C1'>, <class 'object'>]
```
so doing mro() on the C3 class tells us it will look for methods following this order:
[<class '__main__.**C3**'>, <class '__main__.**C2**'>, <class '__main__.**C1**'>, <class 'object'>]



You can override methods but not properties