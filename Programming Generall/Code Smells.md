#oop #general #clean_code
We have severall different type of <font color="#31859b">Code Smells</font>.
* Bloaters
* Object-Orientation Abusers
* Change Preventers
* Dispensaables
* Couplers

> <font color="#ffff00">These are not always the case</font> and removing these smells in some conditions may have worst results or consequences.

## <font color="#00b0f0">Bloaters</font>
Code, methods and classes that are <u>overly large and difficult to manage</u>.
* <font color="#00b0f0">Long Method</font>
		A method contains too many lines of code. Generally, any method longer than ten lines should make you start asking questions.

* <font color="#00b0f0">Large Class</font>
		A class contains many fields/methods/lines of code.

* <font color="#00b0f0"> Primitive Obsession</font>
	-   Use of primitives instead of small objects for simple tasks (such as currency, ranges, special strings for phone numbers, etc.)
	-   Use of constants for coding information (such as a constant `USER_ADMIN_ROLE = 1` for referring to users with administrator rights.)
	-   Use of string constants as field names for use in data arrays.

* <font color="#00b0f0"> Long Parameter List</font>
		More than three or four parameters for a method.
		Long parameter lists may also be the byproduct of efforts to make classes more independent of each other.

* <font color="#00b0f0">Data Clumps</font>
		Sometimes different parts of the code contain identical **groups of variables** (such as parameters for connecting to a database). These clumps should be turned into their own classes, becauses they are mostly with each other.

## <font color="#e36c09">Object-Orientation Abusers</font>

* <font color="#e36c09"> Switch Statements</font>
		You have a complex `switch` operator or sequence of `if` statements.
		As a rule of thumb, when you see `switch` you should think of polymorphism.

* <font color="#e36c09">Temporary Field</font>
		Temporary fields get their values (and thus are needed by objects) only under certain circumstances. Outside of these circumstances, they’re empty.

* <font color="#e36c09">Refused Bequest</font>
		If a subclass uses only some of the methods and properties inherited from its parents, the hierarchy is off-kilter. The unneeded methods may simply go unused or be redefined and give off exceptions.

* <font color="#e36c09">Alternative Classes with Different Interfaces</font>
		Two classes perform identical functions but have different method names.
		The programmer who created one of the classes probably didn’t know that a functionally equivalent class already existed.

## <font color="#00b050">Change Preventers</font>
These smells mean that if you need to change something in one place in your code, you have to make many changes in other places too. Program development becomes much more complicated and expensive as a result.

* <font color="#00b050">Divergent Change</font>
		You find yourself having to change many unrelated methods when you make changes to <font color="#00b050">a class.</font> For example, when adding a new product type you have to change the methods for finding, displaying, and ordering products.

* <font color="#00b050">Shotgun Surgery</font>
		Making any modifications requires that you make many small changes to many different classes.

* <font color="#00b050">Parallel Inheritance Hierarchies</font>
		Whenever you create a subclass for a class, you find yourself needing to create a subclass for another class.


## <font color="#953734">Dispensables</font>
A dispensable is something pointless and unneeded whose absence would make the code cleaner, more efficient and easier to understand.

* <font color="#953734">Comments</font>
		A method is filled with explanatory comments.

* <font color="#953734">Duplicate Code </font>
		Two code fragments look almost identical.

* <font color="#953734">Lazy Class</font> 
		Understanding and maintaining classes always costs time and money. So if a class doesn’t do enough to earn your attention, it should be deleted.
		Perhaps a class was designed to be fully functional but after some of the refactoring it has become ridiculously small.

* <font color="#953734">Data Class</font> 
		A data class refers to a class that contains only fields and crude methods for accessing them (getters and setters). These are simply containers for data used by other classes. 
		It’s a normal thing when a newly created class contains only a few public fields (and maybe even a handful of getters/setters). But the true power of objects is that they can contain behavior types or operations on their data.

* <font color="#953734">Dead Code </font>
		A variable, parameter, field, method or class is no longer used (usually because it’s obsolete).

* <font color="#953734">Speculative Generality</font>
		There’s an unused class, method, field or parameter.
		Sometimes code is created “just in case” to support anticipated future features that never get implemented. As a result, code becomes hard to understand and support.

## <font color="#ffff00">Couplers</font>

* <font color="#ffff00">Feature Envy</font>
		A method accesses the data of another object more than its own data.

* <font color="#ffff00">Inappropriate Intimacy</font>
		One class uses the internal fields and methods of another class.
		Good classes should know as little about each other as possible.

* <font color="#ffff00">Message Chains</font>
		In code you see a series of calls resembling `$a->b()->c()->d()`
		A message chain occurs when a client requests another object, that object requests yet another one, and so on.

* <font color="#ffff00">Middle Man</font>
		If a class performs only one action, delegating work to another class, why does it exist at all?