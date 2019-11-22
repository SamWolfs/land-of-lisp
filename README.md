# Interesting LISP things

## CAR & CDR

**car** takes the first item of a cons and can be compared to the idea of **HEAD** in most languages. 
**cdr** takes the last item of a cons, which can in turn be a sequence of conses and can be compared to the idea of **TAIL** in most languages.

Both commands can be combined as follows:

```lisp
(cadr '(1 2 3)) ;; (car (cdr (1 2 3))) => returns 2
(cadar '((1 5) 2 3)) ;; (car (cdr (car ((1 5) 2 3))) => returns 5 
```

## FLIP & FLOP (quasiquoting)

Both ' (single quote) and ` (backtick) enable **data mode**, but only the *backtick* can be used to return to **code mode**.
To return to code mode when using *backtick data mode*, use the , (comma) before a lisp expression.

```lisp
;;data--------code-------data---code--------data-------    
;;flip        flop              flop
`(there is a ,(caddr edge) going ,(cadr edge) from here)
```

## Association list (alist)

Association lists allow the developer to look up key-value pairs and can be compared to a *map* in other programming languages. In lisp we can use the `assoc` function together with a key and alist to query for the key-value pair.

A cool trick we can use with `alist` and `assoc` is 'replacing' values in the list by adding new key-value pairs to the start of the alist. Since `assoc` only returns the first associated value, this will also return the most recently added value. (as a cool feature, it also maintains a sort of history).

## Higher-order functions

- mapcar: applies a given function to every member of a list
- #' is the function operator and is shorthand for the following: `(function function-name)`
- the *p* suffix is used for functions that return true/false/nil, the p stands for *predicates*
