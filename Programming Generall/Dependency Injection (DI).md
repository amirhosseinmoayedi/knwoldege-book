#general #oop 
# What is <font color="#31859b">Dependency Injection</font> 
Dependency Injection is a technique for creating more flexible and maintainable code<u> by removing hardcoded dependencies between components</u>, allowing them to be easily swapped out or managed centrally.
> It's decouples the class construction from it construction of dependecies.

## What is <font color="#e36c09">Dependency Inversion Principle</font>
Dependency Inversion Principle (DIP) is a principle of software design stating that **high-level modules should not depend on low-level modules**, **but both should depend on abstractions**. This promotes separation of concerns and encourages the use of loosely-coupled, reusable code. **The principle is often used in conjunction with Dependency Injection to implement inversion of control in software design**.
dependency inversion is concept of [[SOLID]].

Example:
```python
# Abstractions
class Engine:
    def start(self):
        pass

class ElectricEngine(Engine):
    def start(self):
        print("Starting electric engine")

class GasEngine(Engine):
    def start(self):
        print("Starting gas engine")

# High-level module
class Car:
    def __init__(self, engine: Engine):
        self.engine = engine

    def start(self):
        self.engine.start()

# Client code
electric_engine = ElectricEngine()
car = Car(electric_engine)
car.start()
# Output: Starting electric engine

gas_engine = GasEngine()
car = Car(gas_engine)
car.start()
# Output: Starting gas engine
```

In this example, the `Car` class is the **high-level module** and it depends on the abstraction of an `Engine`, not on a specific implementation (i.e. `ElectricEngine` or `GasEngine`). This is an example of <font color="#e36c09">Dependency Inversion</font>, as the high-level module depends on an abstraction, not a concrete implementation. Additionally, the `Car` class uses <font color="#31859b">Dependency Injection</font> to receive an instance of the `Engine` abstraction, allowing it to be easily swapped out at runtime.

## <font color="#92d050">container</font>
The term “<font color="#92d050">container</font>” is often used in <font color="#ffff00">DI frameworks</font> to describe the thing into which you add “<font color="#00b0f0">providers</font>” and out of which you ask for **fully-build objects**.
A <u>provider is a mechanism for registering dependencies with a DI container</u>. It tells the container how to create an instance of a particular service. 