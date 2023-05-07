#Solid #oop #general 
### <font color="#92cddc">Cohesion</font>
<font color="#548dd4">Cohesion</font> refers <font color="#548dd4">to the degree to which elements within a module work together to fulfill a single, well-defined purpose</font>. High cohesion means that elements are closely related and focused on a single purpose, while<u> low cohesion means that elements are loosely related and serve multiple purposes</u>.


## <font color="#00b0f0">S.O.L.I.D</font> consitst of five principles:
1. Single Responsibility
2. Open-Closed
3. Liskov Substitution
4. Interface Segregation
5. Dependency Inversion

The SOLID principles provide a <u>set of guidelines for writing maintainable and scalable software</u>. By following these principles, developers can write code that is:

1.  **Easy to understand and modify**
2.  **Flexible** enough to accommodate changing requirements
3.  **Easier to test and debug**
4.  More likely to be **free of bugs** and other defects

By adhering to SOLID principles, <font color="#ffff00">developers can reduce the cost and effort required to maintain and evolve software over time</font>, leading to higher quality, more robust, and more reliable systems.

## Single Responsibility Principle
**A class should have only one job**.
If a class has **more than one responsibility**, it becomes <font color="#e36c09">coupled</font>.
```python 
class Animal: # has on responsibilty to create a animal and get it name
	def __init__(self, name: str):
		self.name = name

	def get_name(self):
		pass

class AnimalDB: # responsibilty to get the animal from date base or save the change (database interactions)
	def get_animal(self) -> Animal:
		pass

	def save(self, animal: Animal):
		pass

"""
```

## Open-Closed Principle
<font color="#e36c09">Software entities(Classes, modules, functions)</font> should be open for extension, not modification.
```python 
class Animal:
	def __init__(self, name: str):
		self.name = name

	def get_name(self) -> str:
		pass
		
	def make_sound(self):
		pass

class Lion(Animal):
	def make_sound(self):
		return 'roar'

class Mouse(Animal):
	def make_sound(self):
		return 'squeak'

class Snake(Animal):
	def make_sound(self):
		return 'hiss'

def animal_sound(animals: list):
	for animal in animals:
		print(animal.make_sound())

animal_sound(animals)
```

## Liskov Substitution Principle
<u>A sub-class must be substitutable for its super-class</u>.
```python 
class Animal:
	def leg_count(self):
		pass

class Lion(Animal):
	def leg_count(self):
		pass

class kangrow(Animal):
	def leg_count(self):
		pass

def animal_leg_count(animals: list):
	for animal in animals:
		print(animal.leg_count())

animal_leg_count(animals)
```
> this principle is not violited if inherited wrongly

## Interface Segregation Principle
<u>Make fine grained interfaces that are client specific</u>, clients should not be forced to depend upon interfaces that they do not use.
This is<font color="#ff0000"> wrong example</font> :
```python 
class IShape:
	def draw_square(self):
		raise NotImplementedError

	def draw_rectangle(self):
		raise NotImplementedError

	def draw_circle(self):
		raise NotImplementedError

class Circle(IShape):
	def draw_square(self):
		pass

	def draw_rectangle(self):
		pass

	def draw_circle(self):
		pass

class Square(IShape):
	def draw_square(self):
		pass

	def draw_rectangle(self):
		pass

	def draw_circle(self):
		pass
```

>This principle deals with the disadvantages of implementing big interfaces.

## Dependency Inversion Principle
Usally comes with [[Dependency Injection (DI)]]
Dependency should be on abstractions not concretions
1. <font color="#e36c09"> High-level modules should not depend upon low-level modules</font>. Both should depend upon abstractions.
2. Abstractions should not depend on details. Details should depend upon abstractions.
```python
class AbstractModule:
    def do_something(self):
        pass

class LowLevelModule(AbstractModule):
    def do_something(self):
        print("Low-level module is doing something")

class HighLevelModule:
    def __init__(self, module: AbstractModule):
        self.module = module

    def do_something(self):
        self.module.do_something()

```