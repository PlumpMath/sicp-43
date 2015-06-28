* Installed DrRacket with SICP support from http://www.neilvandyke.org/racket-sicp/
	* There is also MIT Scheme, but people recommend nowadays DrRacket with the
	  Neil Van Dyke's SICP module.
* To use SICP support just add `#lang planet neil/sicp` to the beginning of the file
* Learned that I can open the REPL from terminal with `neil/sicp` support(for
  more info http://crash.net.nz/) :

		racket  -i -p neil/sicp -l xrepl

  I liked this better because it let me use `readline`, so all bash (emacs)
  shortcuts (like jumping to the beginning) works. Don't know if DrRacket
  support it though.

# Chapter 1

* Looking at evaluation strategies: https://en.wikipedia.org/wiki/Evaluation_strategy
* LISP useses applicative-order evaluation (also called call-by-value). There
  is also normal-order evaluation(called call-by-name). Applicative-order
  evaluation is more efficient, because it avoids multiple evaluations of
  expressions. It evaluates the arguments first and applies the resulting
  procedure to the resulting arguments.
* `predicate`: an expression whose value is interpreted as either true or
  false. In Scheme these are the constants #t and #f. If the interpreter checks
  a predicate's value, it interprets #f as false.
* conditional expression: The name `cond` is used, consisting of predicates. It
  continues until a predicate's value is true. If  none of the predicate's is
  true, the value of `cond` is undefined
* There are some primitive predicates, such as `>`, `<`, `=`, etc..
* It's awesome we can create predicates our self. An example from the book:

		(define (>= x y) (or (> x y) (= x y)))
