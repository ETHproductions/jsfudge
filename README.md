# JSLambda
A mathematical, possibly Turing-complete subset of JavaScript.

## The subset
Only a 8 characters are freely allowed in JSLambda: `()?:f1->`

In addition, the arrow function "keyword" `=>` is allowed (note that `=` is only allowed as a part of `=>`).

## How it works
At the heart of JSLambda is the self-application function:

    f=>f(f)

This allows you to recurse a function without assigning it to a variable, by making it an argument to itself:

    (f=>f(f))(ff=>f=>f>1?ff(ff)(f-1-1):f)  // Function that calculates (n % 2) on non-negative numbers

Confused? Let's add in `n` for simplicity's sake:

    (f=>f(f))(f=>n=>n>1?f(f)(n-1-1):n)
    
The main function `f=>n=>n>1?f(f)(n-1-1):n` accepts a function and a number. It returns the number if it happens to be 1 or less; otherwise, it calls the function on itself, then the number - 2. In this way, once the function is called with itself once, it always has access to itself through `f`, even though it hasn't been assigned to a variable.

## Turing-completeness
It can be shown that any (integer, integer, ...) -> integer function (with a known number of inputs) can be implemented with this subset, although I haven't yet taken the time to do this. However, in order to be Turing-complete, a language must have a way to and manipulate infinite memory, if given adequate hardware. Infinite output could be achieved with the addition of a function that takes a char-code and prints it to STDOUT, but I am unsure if there is an adequate technique for taking an unknown number of inputs.

If/when the [BigInt proposal](https://github.com/tc39/proposal-bigint) is passed and officially becomes a part of JavaScript, it would easily be possible to represent any string of bytes by converting it from base-256 into a huge integer, thus fulfilling the requirement for theoretically infinite memory. But until this happens, JSLambda may not be Turing-complete.

## Examples

Adding two numbers:

    f=>ff=>f-(-ff)
    
Number (non-negative) mod 2:

    (f=>f(f))(ff=>f=>f>1?ff(ff)(f-1-1):f)

more examples and interpreter to come very soon...
