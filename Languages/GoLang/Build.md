#go #golang 
The **Go tooling** has a command that can print a list of the possible platforms that Go can build on.
`go tool dist list`
result example:
```
linux/ppc64le
linux/riscv64
linux/s390x
windows/386
windows/amd64
windows/arm
windows/arm64
```
the first part is <font color="#ffc000">OS</font>, second part is <font color="#ffc000">architecture</font>.
<font color="#92cddc">GOOS</font> (**Go Operating System**), <font color="#92cddc">GOARCH</font> (**Go Architecture**).

When you run a command like `go build`, Go uses the <u>current platform’s</u> `GOOS` and `GOARCH` <u>to determine how to build the binary</u>. To find out what combination your platform is, you can use the `go env` command and pass `GOOS` and `GOARCH` as arguments:
`go env GOOS GOARCH`
result:
```
windows
amd64
```

> Some of libraries or packages are os dependent have behave differently, like `path/filepath` in unix and windows  becasue of `/` and `\`.

we can instruct go builder to behave differently on each package base on the os with the <font color="#fac08f">BuildTags</font>.

for specifying the the env before the build:
`GOOS=linux GOARCH=ppc64 go build`
