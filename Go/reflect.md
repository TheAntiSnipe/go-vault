# Personal notes

This is very chaotic, I asked this question on the Go official Discord server and got answers I'm not quite qualified to interpret yet. 
>Anything using struct tags for data population is done via `reflect`. `reflect` allows you to map field names or field-struct-tags to incoming values.

>To give a better explanation `reflect` is the only way to arbitrarily assign data into an unknown struct type. But `reflect` can only access exported fields (i.e. Fields With Capitalized Names). Most JSON or other data formats don't capitalize the names so `struct tags` allow lower case key-names in the incoming data to be mapped to the Exported Struct Fields that `reflect` has access to.

> Mostly you'll encounter packages that tell you what struct tags they look for and how they should be formatted. You yourself will write very little or no code that actually uses reflect directly.

> `reflect` is basically Go's skeleton in the closet. It's required to implement most "general purpose" libraries or packages and 99% of Gophers like to pretend it doesn't exist and would hide it from the authorities if they could.

\- From @superfiend#4211 from the Discord server for Go.

Tags: #go, #backend, #reflect, #wip, #esoterics