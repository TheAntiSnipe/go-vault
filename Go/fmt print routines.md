# Personal notes

So this is where it gets funky. Let's start with the three major types of print, and I get the feeling I'll be updating this at some point, so hang a (WIP) on this.

You've got:
- `Fprint`
	- `Fprint` writes to `w`. `w` is any `io.Writer` object. Pythonic style of writing, but remember that `w` is the first argument. If you assign the value of `Fprint` to something, it's actually two things: `n` and `error`. 
	- `n` is the number of bytes that were just written.
	- `error` is `nil` if there were no issues, a default construct (noted by the `%v` verb in this case)

- `Sprint`
	- The one we just saw in the `greetings.go` code in [[creating a Go module]]. 
	- Faithfully returns a string. There's no risk of failing anyway.
	- Has a C-like format for writing within the brackets.
- `Print`
	- `Fprint`'s bro.
	- Writes directly to os.Stdout, no questions asked.
	- Has the same Python-like formatting other than that.

Tags: #go, #print, #os-Stdout