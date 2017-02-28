# Hyffix

Prefix, infix and postfix notation macros for [Hy](https://github.com/hylang/hy)

## Main macro: deffix

Macro will find out if formula is given either in infix, postfix or prefix format. Internally all affixes will be translated to prefix version to meet Hy and Python language specification. Here infix notation is as an example.

Deffix macro can be used in various ways. Arguments can be given with or without parentheses. For example:

```
(deffix 1 + 2 * 3) ; 9
```

On infix notation the order of evaluation starts from the left side of the formula by default. With parentheses the order of evaluation can be forced:

```
(deffix 1 + (2 * 3)) ; 7
```

## Precedence macro: defprecedence

To define the order of precedence can be done by the specific macro called: defprecedence. To imitate the parenthesized formula without parentheses can be done for example:

```
(defprecedence * -)
(deffix 1 + 2 * 3) ; 7
```

One can reset precedence order with an empty call: `(defprecedence)`

On prefix and postfix notation the order of precedence is not applicable.

## Reader macro shorthand

Hy language support shorthands for macros. One implemented for deffix is $ that can be used like:

```
#$(1 + 2 * 3)
```

Contra to deffix macro, one needs to use parentheses to make evaluation work right way.

## Prefix, infix and postfix mix

It is possible to use any of the three affixes in the same formula as long as they are correctly separated by parentheses:

```
#$(+ 1 (1 + (1 +))) ; 3
```

## Multiple expressions

Use useful way to evaluate many formulas is to use deffix-n macro. Each of the formulas needs to be inside parentheses:

```
(deffix-n (0 + 1) (1 + 2) (2 + 3) (3 + 4)) ; [1 3 5 7]
```

## Custom operators

All basic native operations are supported by deffix without any custom configuration. For example these are valid expressions:

```

```