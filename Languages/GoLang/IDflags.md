#go #golang #build #tool 
When deploying applications into a production environment, **building binaries with version information and other metadata will improve your monitoring, [[Logging]], and debugging processes by adding identifying information to help track your builds over time**.

in Go is to use `-ldflags` with the `go build` command <u>to insert dynamic information into the binary at build time</u>, without the need for source code modification.

### <font color="#b2a2c7">what is ID</font>
In this flag, `ld` stands for <font color="#e36c09">linker</font>, <font color="#e36c09">the program that links together the different pieces of the compiled source code into the final binary</font>. `ldflags`, then, stands for _linker flags_. It is called this because it passes a flag to the underlying Go toolchain linker, `cmd/link`, that allows you to change the values of imported packages at build time from the command line.

```bash
go build -ldflags="-flag"
```

we will use the `-X` flag to <u>write information into the variable at link time</u>, followed by the package path to the variable and its new value:
```bash
go build -ldflags="-X 'package_path.variable_name=new_value'"

go build -ldflags="-X 'main.Version=v1.0.0'"
```
The `.` character **separates the package path and the variable name**, and **single quotes are used to avoid breaking characters** in the key-value pair.

In order to use `ldflags`, the <u>value you want to change must exist and be a package level variable of type</u> `string`.This variable can<u> be either exported or unexported</u>. The value cannot be a `const` or have its value set by the result of a function call.

to set variable in subdirectory:
```bash
go build -v -ldflags="-X 'main.Version=v1.0.0' -X 'app/build.User=$(id -u -n)' -X 'app/build.Time=$(date)'"
```
Here you passed in the `id -u -n` Bash command to list the current user, and the `date` command to list the current date.