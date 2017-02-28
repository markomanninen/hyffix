# Hyffix

Prefix, infix and postfix notation macros for Hy

<code>
; reader macro shorthand
#$(+ 1 (1 + (1 +)))
</code>

<code>
; reset and use default left to right precedence
(defprecedence)
; this time use normal deffix macro. answer should be 9
(print (deffix 1 + 2 * 3))
; alter precedence so that first multiplication, then addition is made
(defprecedence * +)
; print again, answe should be 7
(print (deffix 1 + 2 * 3))
</code>

<code>
; multiexpression
(print (deffix-n (0 + 1) (1 + 2) (2 + 3) (3 + 4)))
</code>
