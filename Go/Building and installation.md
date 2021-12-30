# Personal notes

This last bit is quite easy. We're just gonna install our application as a binary. A `.exe` file.

The process of just creating a `.exe` file is easy if you just want a local file:
A
```cmd
go build
```
command will do the trick. Remember to navigate to the `hello` folder in this context though.

But this is just a local file. If you move the exe to, say, Desktop, it won't work.
To properly install a file, a bit of setup is necessary. First, we need to locate the place where Go will put the binaries when they are installed. For this, we type in
```cmd
go list -f '{{.Target}}'
```
This shows us where Go's target path for installed binaries is. This is the place where, if you pull off a `go install`