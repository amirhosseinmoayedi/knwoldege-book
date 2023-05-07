#go #golang #commands 
| Command | Description |
| --- | --- |
| run | execute the package |
| build | build the binray version |
| fmt | gofmt (reformat) package sources |
|mod init| create module |
|intall | compile and install package and dependency |
| env |  portably set the default value for an environment variable for future `go` commands |
|test | run every file with name `*_test.go` |
| mod tidy | update the go.sum file |
| vet | starts where the compiler ends by identifying subtle issues in your code. It’s good at catching things where your code is technically valid but probably not working as intended. |
|goimports| add the imports pakcage and remove unused and group them|

If `GOPATH` is set, binaries are installed to the `bin` subdirectory of the first directory in the `GOPATH` list.

![[Pasted image 20230311220142.png]]