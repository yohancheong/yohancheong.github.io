---
layout: post
title: Lambda Calculus Pt. 2
---

## Example of Free Variables

* x is free in E if:
    * Rule 1 (E = x) ? yes, since there is no meta variable for x 
    * Rule 2 (E = &lambda;y.E1) ? yes, if y != x
    * Rule 3 (E = E1 E2) ? yes, if either E1 or E2 is free
        * x free in x&lambda;x.x ? Yes, x(&lambda;x.x) where x is free in E1
        * x free in (&lambda;x.xy)x ? Yes, since E2 is free
        * x free in &lambda;x.yx ? No, &lambda;x.(yx)

## Combinators
An expression is a combinator if it doesn't have any free variables

* &lambda;x.&lambda;y.xyx combinator ? yes (The function takes x and y as parameters and do xyx)
* &lambda;x.x ? yes
* &lambda;z.&lambda;y.xyz ? no, since y is free

## Bound Variables

* If a variable is not free then it is Bound
    * Bound Rule 1: If x is free in E then it is bound by &lambda;x in &lambda;x.E
    * Bound Rule 2: If x is bound by &lambda;x in E then x is bound by same *inner most* &lambda;x in &lambda;z.E
        * Even if z == x
        * &lambda;x.&lambda;x.x
            * Which lambda expression binds x? inner most &lambda;x
    * Bound Rule 3: If x is bound by &lambda;x in E1 then E1 is tied by same abstraction &lambda;x in E1 E2 and E2 E1. Think it easy as it said.

## Examples

* (&lambda;x.x(&lambda;y.xyzy)x)xy
    * (&lambda;**x**.**x**(&lambda;*y*.**x***y*<u>z</u>*y*)**x**)<u>x</u><u>y</u>
* (&lambda;x.&lambda;y.xy)(&lambda;z.xz)
    * (&lambda;**x**.&lambda;*y*.**x***y*)(&lambda;*z*.<u>x</u>*z*)
* (&lambda;x.x&lambda;x.zx)
    * (&lambda;**x**.**x**&lambda;**x**.<u>z</u>**x**)

## Equivalence

* What does it mean for two functions to be equivalent?
    * &lambda;y.y = &lambda;x.x ? Same functions
    * &lambda;x.xy = &lambda;y.yx ? 
    * &lambda;x.x = &lambda;x.x ? Same functions

## &alpha;-Equivalence
When two functions vary only by names of bound variables
E1 = &alpha;E2
* &lambda;x.x&lambda;y.xyz
    * Can we rename x to foo? yes
    * Can we rename y to bar? yes
    * Can we rename y to x? no, since it creates bound variable & changes the existing semantics
    * Can we rename z to x? no, since it creates bound variable & changes the existing semantics

## Renaming Operation
E{y/x} = renaming x to y
* x{y/x} = y
* z{y/x} = z, if x != z
* (E1 E2) {y/x} = (E1{y/x})(E2{y/x})
* (&lambda;x.E){y/x} = (&lambda;y.E{y/x})
* (&lambda;z.E){y/x} = (&lambda;z.E{y/x}), if x != z

## Examples
* (&lambda;x.x){foo/x}
= (&lambda;foo.(x){foo/x})
= (&lambda;foo.(foo))

* ((&lambda;x.x(&lambda;y.xyzy)x)xy){bar/x}
= (&lambda;x.x(&lambda;y.xyzy)x){bar/x}(x){bar/x}(y){bar/x}
...
= (&lambda;bar.(x(&lambda;y.xyzy)x){bar/x}) bar y
= (&lambda;bar.(bar(&lambda;y.(bar y z y))bar)) bar y

## &alpha;-Equivalence
* For all expressions E and all variables y that do not occur in E
    * &lambda;x.E = &alpha;y.(E{y/x})
* &lambda;y.y = &lambda;x.x ? yes
* ((&lambda;x.x(&lambda;y.xyzy)x)xy) = ((&lambda;y.y(&lambda;z.yzwz)y)yx) ? no 
* (&lambda;x.x(&lambda;y.xyzy)x) = (&lambda;y.y(&lambda;z.yzwz)y) ? no, bound variables can be different name, but you shouldn't change free variable such as z as it will change semantics

## Substituion
(&lambda;x.+x 1)x -> (+ 1 2)
* Can we use renaming? no
* We need another operator, called substitution, to replace a variable by a lambda expression
    * E[x->N], where E and N are lambda expressions and x is a name
* (+ x 1)[x->2] = (+ 2 1)
* (&lambda;x.+ x 1)[x->2] = (&lambda;x.+ x 1) Nothing to change since the varialbe is bound
* (&lambda;x.yx)[y->&lambda;z.xz] != (&lambda;x.(&lambda;z.xz)x)
 = (&lambda;w.(&lambda;z.xz)w)
See <a href="https://www.youtube.com/watch?v=zg0UgCg7tZQ&t=3583s">CSE 340 11-25-15 Lecture: "Lambda Calculus Pt. 2"</a>
