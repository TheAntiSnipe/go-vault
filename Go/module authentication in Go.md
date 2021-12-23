# Personal notes

**Why do we auth modules?**
Basically, when the go command downloads a zip file or a go.mod file into its module cache, it computes this hash that allows it to detect tampering compared to when the file was first downloaded. 

**What is the module cache anyway?**
The module cache is this directory where Go stores downloaded module files. It's located in `$GOPATH/pkg/mod`, where GOPATH is the path where Go is stored on the machine or server.
This cache can be shared between multiple projects living in the same machine. Supports multi-access, so no worries about running multiple projects on the same machine at the same time.

If deleting from the Go cache, remember that `rm -rf` <mark style="background: undefined;">will not work</mark> . Instead, use `go clean -modcache`. You can use `go clean -modcacherw` to instead make these cache items rewritable, and thus be able to mod them. However, be careful: <mark style="background: undefined;">This leaves these programs with no protection from tampering.</mark> 
To check for tampering, we can use the `go mod verify` command. This checks modifications to dependencies of the main module and checks if they match the parameters in `go.sum`.
Atharva: Yo
