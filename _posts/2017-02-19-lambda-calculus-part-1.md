---
layout: post
title: Lambda Calculus Pt. 1
---

Lambda calculus is a language `to express function application`, which enables us to

* Define functions (anonymous)
* Apply functions
 
 
It is also a source of functional programming, and consists of **Syntax** (grammar) and **Symantics** (meaing) as other languages.

## Syntax
There are only `4 syntactic expressions` in lambda calculus (E for expression)

* Rule 1: E -> ID e.g. x
* Rule 2: E -> &lambda;ID.E e.g. &lambda;x.x
* Rule 3: E -> E E e.g. foo &lambda; bar . (foo (bar baz))
* Rule 4: E -> (E) 

But, there is ***ambiguous syntax*** in such as &lambda;x.xy since it can be parsed in either direction e.g. Rule 2, 3 and Rule 3, 2
<img src="../public/image/2017-02-19-ambiguos-syntax.jpg" style="width:70%"/> This can resolved with *disambiguation rules*

* E -> E E is `left associative` e.g. xyz is (xy)z and wxyz is ((wx)y)z
* &lambda;ID.E `extends as far to the right` e.g. &lambda;x.xy is &lambda;x.(xy) and &lambda;x.&lambda;x.x is &lambda;x.(&lambda;x.(x))

Examples

* (&lambda;x.y)x != &lambda;x.yx (== &lambda;x.(yx))
* &lambda;x.(x)y == &lambda;x.((x)y)
* &lambda;a.&lambda;b.&lambda;c.abc == &lambda;a.(&lambda;b.(&lambda;c.((ab)c)))

## Semantics
Every ID in lambda calculus is called as "variable"

* E -> &lambda;ID.E is "abstraction" 
    * ID is the variable of the abstraction
    * E is the body of abstration
* E -> E E is "application" (Calling a function)

* Semantic 1: E -> &lambda;ID.E defines a new anonymous function
    * That's why anonymous function is called "lambda expressions" in programming language
    * ID is the parameter of the function
    * E is the body of the function
* Semantic 2: E -> E1 E2, is similar to calling function E1 and setting its parameter to be E2

Examples
* Semantic 1: &lambda;x.+ x 1 (Taking x as parameter and summing up x and 1)
* Semantic 2: (&lambda;x.+ x 1)2 (Calling function E1 setting its parameter as 2 i.e. + 2 1 = 3)

How can + function be defined if abractions only accept 1 parameter? *Currying*...
- A function that takes multiple arguments into a seuqence of functions that each take a single arguments

* &lambda;x.&lambda;y.((+ x) y) 
* (&lambda;x.&lambda;y.((+ x) y))1 = &lambda;y.((+ 1) y)
* (&lambda;x.&lambda;y.((+ x) y)) 10 20 = (&lambda;y.((+ 10) y))20 = ((+ 10) 20) = 30


## Free variable
It doesn't appear within the body of the abstraction with a metavariable of the same name (e.g. x in &lambda;x)

* x free in &lambda;x.xyz ? no
* y free in &lambda;x.xyz ? yes
* x free in (&lambda;x.(+ x 1)) x ? yes
* z free in &lambda;x.&lambda;y.&lambda;z.zyx ? no
* x free in (&lambda;x.z foo)(&lambda;y.y x) ? yes


See <a href=" https://www.youtube.com/watch?v=_kYGDJSm0gE&t=1746s">CSE 340 11-23-15 Lecture: "Lambda Calculus Pt. 1"</a>
