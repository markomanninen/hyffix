# Hyffix

Prefix, infix and postfix notation macros for [Hy](https://github.com/hylang/hy)

## Main macro: deffix

Deffix -macro will find out if given formula is either in infix, postfix, or prefix format. Internally all affixes will be translated to the prefix format to meet Hy and Python language specification. Here infix notation is as an example at first.

Deffix macro can be used in various ways. Arguments can be given with or without parentheses. Without parentheses example:

```
(deffix 1 + 2 * 3) ; 9
```

On infix notation the order of evaluation starts from the left side of the formula by default. With parentheses the order of evaluation can be forced. Note how resulting values changes from 9 to 7:

```
(deffix 1 + (2 * 3)) ; 7
```

## Precedence macro: defprecedence

To define the order of precedence one can use the specific macro called: `defprecedence`. To imitate the parenthesized formula without parentheses:

```
(defprecedence * -)
(deffix 1 + 2 * 3) ; 7
```

One can reset the order precedence with an empty call: `(defprecedence)`

On prefix and postfix notation the order of precedence is not applicable same way than on infix notation. Instead the order is defined by parentheses:

```
(deffix (* 2 2 (+ 2 2))) ; 16
```

## Reader macro shorthand

Hy language support shorthands for macros. Contra to deffix macro, one needs to use initial parentheses to make evaluation work right way. One implemented for deffix is $ which can be used something like:

```
#$(1 + 2 * 3) ; = (deffix 1 + 2 * 3) = (deffix (1 + 2 * 3)) = 7
```

## Prefix, infix and postfix mix

It is possible to use any of the three affixes in the same formula as long as they are correctly separated by parentheses:

```
#$(+ 1 1 (1 + 1 + (1 1 +))) ; 6
```

## Multiple expressions

Use useful way to evaluate many formulas is to use deffix-n macro. Each of the formulas needs to be inside parentheses along these lines:

```
(deffix-n (0 + 1) (1 + 2) (2 + 3) (3 + 4)) ; [1 3 5 7]
```

## Custom operators

All basic native math operations (* / + - = !=) from Python core are supported by deffix without any custom configuration. For example these are valid expressions:

```
(print
 (deffix 1 + 2 - 3 * 4 / 5) ; -1.8
 [(deffix (1 + 1) = 2) (deffix (1 + 1) != 2)] ; [True False] 
)
```

To define your own operators you can use `defoperator` macro. For example, custom modulo function as an operator:

```
(defoperator mod [x y] (% x y))
(deffix 3 mod 2) ; 1
```

Basicly all operators with binary behaviour are good. So you can create a function that takes 1 or two arguments. Then macro will pass left and rigth side of the operator to the function.

```
(defoperator op [&rest args] (sum args))
(deffix 2 op 2) ; 4
```

```
(defoperator and? [&rest args] (all args))
(deffix 1 and? 1 and? 0) ; False
```

```
(defoperator x^y [x y] (pow x y))
(deffix 2 x^y 3) ; 8
```

## Custom operands

Operands can be any numeric, boolean and string value. To use variable as a operand you need to initialize it with `defoperand` macro:

```
(defoperand x 1 y 2 z 3)
(, operands) ; ({'z': 3, 'x': 1, 'y': 2},)
```

Then it is possible to use them on the formula:

```
(deffix 1 - (x + (* y (z /)))) ; -0.6666666666666665
```

If undefined operator is used on formula, it will raise a `NameError` exception:

```
(try 
 (eval '(deffix 1+1))
 (except [e [Exception]]
    (print "NameError: name '1+1' is not defined")))
```

One more complex function that calculates a truangular number value is given here:


```
(require [hy.contrib.loop [loop]])

; triangular number function
; if start is 1 and limit is 4, then result is 1 + 2 + 3 + 4 = 10
; https://en.wikipedia.org/wiki/Triangular_number
(defoperator Tn [limit &optional [start 1]]
  (loop [[result start] [start start] [limit limit]]
    (if (< start limit)
        (recur (+ result (inc start)) (inc start) limit)
        result)))

(deffix 4 Tn 1 Tn 1) ; 55
```

To find out more extensive way of using hyffix module, please see [hylogic](https://github.com/markomanninen/hylogic) module on GitHub.

### The [MIT](https://choosealicense.com/licenses/mit/) License

Copyright © 2017 Marko Manninen
