# JSFudge
A mathematical, possibly Turing-complete subset of JavaScript.

## The subset
Only a 8 characters are freely allowed in JSFudge: `()?:f1->`

In addition, the arrow function "keyword" `=>` is allowed (note that `=` is only allowed as a part of `=>`).

## How it works
At the heart of JSFudge is the self-application function:

    f=>f(f)

This allows you to recurse a function without assigning it to a variable, by making it an argument to itself:

    (f=>f(f))(ff=>f=>f>1?ff(ff)(f-1-1):f)  // Function that calculates (n % 2) on non-negative numbers

Confused? Let's add in `n` for simplicity's sake:

    (f=>f(f))(f=>n=>n>1?f(f)(n-1-1):n)
    
The main function `f=>n=>n>1?f(f)(n-1-1):n` accepts a function and a number. It returns the number if it happens to be 1 or less; otherwise, it calls the function on itself, then the number - 2. In this way, once the function is called with itself once, it always has access to itself through `f`, even though it hasn't been assigned to a variable.

## Turing-completeness
I believe it can be shown that any (integer, integer, ...) -> integer function (with a fixed number of inputs) can be implemented with this subset, although I haven't yet taken the time to do this. However, in order to be Turing-complete, a language must have a way to manipulate arbitrary amounts of memory, if given adequate hardware. Arbitrary output could be achieved with the addition of a function that takes a char-code and prints it to STDOUT, but I am unsure if there is an adequate technique for taking an arbitrary input.

If/when the [BigInt proposal](https://github.com/tc39/proposal-bigint) is passed and officially becomes a part of JavaScript, it would easily be possible to represent any string of bytes by converting it from base-256 into a huge integer, thus fulfilling the requirement for theoretically infinite memory. But until this happens, JSFudge may not be Turing-complete.

## Examples

Adding two numbers:

    f=>ff=>f-(-ff)

Non-negative mod 2:

    (f=>f(f))(ff=>f=>f>1?ff(ff)(f-1-1):f)

Non-negative mod `n` (hardcoded):

    (f=>f(f))(ff=>f=>f>n-1?ff(ff)(f-n):f)

Non-negative mod `n` (as input):

    (f=>f(f))(ff=>f1=>f=>f1>f-1?ff(ff)(f1-f)(f):f1)

Multiplication:

    (f=>f(f))(ff=>f=>f1=>--f1?ff(ff)(f)(f1)-(-f):f)

more examples and interpreter to come very soon...

## Credits

This idea was not mine originally&mdash;I got the idea from [this knowledge-bending website](https://alf.nu/ReturnTrue), where this subset is used to solve "fibfudge" (Season 2, 66th level). But since this page is so obscure, and quite difficult to access, I decided to expand on this idea and share it with the world. I do not claim any ownership to the idea, hence why I have licensed this repo under the MIT License.
