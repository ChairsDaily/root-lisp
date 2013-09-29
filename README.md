## Root Lisp

Root Lisp is a Lisp, as originally described by John McCarthy, implemented in Python. It is based on the description in [Recursive Functions of Symbolic Expressions and Their Computation by Machine, Part 1][rec-func], but also highly inspired and influenced by the explanation in [The Roots of Lisp][roots] by Paul Graham.

[rec-func]: http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.111.8833&rep=rep1&type=pdf
[roots]: http://www.paulgraham.com/rootsoflisp.html

Just like the original this is a small, although neat, language with a lot of bells a whistles missing. It has no side effects (no IO), no types other than atoms (e.g. no numbers, strings, etc), no error handling (programs are expeted to be expressed correctly), and dynamic rather than lexical scoping.

### Differences from McCarthys original

This language differs from the original Lisp in some minor regards.

- Atoms can be written in lower case letters and with special characters (except single quote `'` and parentheses), in addition to the originals CAPS ONLY atoms. 
- Atoms cannot contain spaces, thus elminating the need for dotted pair notations or commas within lists.
- Quoting can be done using single quote tick (e.g. `'(a quoted list)` instead of `(quote a quoted list)`) in order to improve readability of programs a bit.

In addition, I could not figure out how to define and refer to functions outside of `label` expressions, and thus introduced a `defun` construct like the one described in "The Roots of Lisp". If anyone have inputs on this, I'd be delighted for a message or pull-request on the issue.

The implementation itself also differ a lot, obviously, being written in Python rather than for the [IBM 704](http://en.wikipedia.org/wiki/IBM_704), but I tried to keep the semantic regaring the computational model as close to the original as possible.

### What's what?

The core of the language resides in `rootlisp/core.py` and consists of the `eval` function and some auxillary functions.

The parser in `rootlisp/parser.py` handles converting strings into the ASTs used by `eval`.

Finally, `rootlisp/lisp.py` contains a simple REPL and functions for interpreting Lisp strings and files.

### Play

Try out the Lisp using the REPL:

```lisp
$ ./lisp 
> (cons (cons 'a '()) (cons 'b (cons 'c '(d e f))))                            
((a) b c d e f)
> (atom 'yep)
t
> (defun not (x) (cond (x 'f) ('t 't)))
not
> (not 't)
f
> (not 'f)
t
> (defun null (x) (eq x '()))
null
> (null '())
t
> (not (null '(not empty)))
t
> 
```

Or interpret a file by providing an argument

```
$ ./lisp examples.lisp
(a x b y c z d)
```

### Run the tests

In order to run the test suite, you'll need to install `nose`.

```
$ pip install nose
```

You can then run the tests using `nosetests` from the project's root directory.

```
$ nosetests
................................................
----------------------------------------------------------------------
Ran 48 tests in 0.033s

OK
```
