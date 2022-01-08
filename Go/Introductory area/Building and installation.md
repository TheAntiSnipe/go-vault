# Personal notes

> NOTE: People running Linux machines are free to refer to [Compile and install the application - The Go Programming Language](https://go.dev/doc/tutorial/compile-install) for the way to do these steps on Linux, and update these notes if they feel like it. Cheers!

This last bit is quite easy. We're just gonna install our application as a binary. A `.exe` file.

The process of just creating a `.exe` file is easy if you just want a local file:
A
```cmd
go build
```
command will do the trick. Remember to navigate to the `hello` folder in this context though. Once you do that, you can run it by typing in
```cmd
hello.exe
```

But this is just a local file. If you move the exe to, say, Desktop, it won't work.

To properly install a file, a bit of setup is necessary. First, we need to locate the place where Go will put the binaries when they are installed. For this, we type in
```cmd
go list -f '{{.Target}}'
```
This shows us where Go's target path for installed binaries is. This is the place where, if you pull off a `go install`, your `.exe` will be saved. But if it's saved there, your PC won't be able to find it. So you need to update the path. As an output for the above command, let's say you get this:
```path
C:\Users\username\go\bin\hello.exe
```
You need to add 
```path
C:\Users\username\go\bin
```
to your path with the command
```cmd
set PATH=%PATH%;C:\Users\username\go\bin
```
Or, heck, you can just go set path variables.

Once you do this, the rest is easy: Go to the `hello` directory again, and type in
```cmd
go install
```
and boom! You're done. Now you can just go to any ol' command line and type in
```cmd
hello
```
and it'll give you the outputs you want!

Tags: #go , #go-cmd , #go-install, #go-build, #backend 