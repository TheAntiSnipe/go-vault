# Personal notes

I decided to handle this problem statement by myself first, going in blind. The goal is as follows:
>In the last changes you'll make to your module's code, you'll add support for getting greetings for multiple people in one request. In other words, you'll handle a multiple-value input, then pair values in that input with a multiple-value output. To do this, you'll need to pass a set of names to a function that can return a greeting for each of them.

Right, so first off, I need to write a new function, and I need to run a loop over all the names and run the Hello function over each. So I designed the code like this:

``