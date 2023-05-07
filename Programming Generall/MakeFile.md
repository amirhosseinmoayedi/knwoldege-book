#general #tool #makefile
<font color="#e36c09">Makefile</font> is a **file that is usally contains bash(or any shell) commands** for compling and running the project there are many tools that do it but usually for <u>C, C++, Golang</u> we use it.

The standard one is <font color="#953734">GNU Make</font>.

The syntax is like this:
```Makefile
name: [dependecy]
indent|command
```

Sample:
```makefile
# the first one is the default one so if you run `make` without args `hello` will be runned 

# vars
name := ali

hello:
	echo "hello world"


goodbye: hello
	# using the variable
	echo "bye bye ${name}"

clean:
	clear
```

you can use **conditions** and **functions** in makefile.

Add an `@` before a command <font color="#31859b">to stop it from being printed</font>.
You can also run make with `-s` to add an `@` before each line.


###  Make <font color="#31859b">clean</font>
`clean` is often used as a target that **removes the output of other targets**, but it <u>is not a special word in Make</u>. You can run `make` and `make clean` on this to create and delete `some_file`.

### <font color="#fac08f">strings</font>
**Single or double quotes have no meaning to Make**. They are simply characters that are assigned to the variable

### <font color="#b2a2c7">Wild Cards</font>
it's got `%` and `*` but with total different meaning and usage.

### <font color="#76923c">Default Shell</font>
The default shell is `/bin/sh`. You can change this by changing the variable SHELL:
```makefile
SHELL=/bin/bash

cool:
	echo "Hello from bash"
```

### Error handling with `-k`, `-i`, and `-`
Add `-k` when running make to continue running **even in the face of errors**. Helpful if you want to see all the errors of Make at once.  
Add a `-` before a command to **suppress the error**  
Add `-i` to make to have this happen for **every command**.
```makefile
one:
	# This error will be printed but ignored, and make will continue to run
	-false
	touch one
```

