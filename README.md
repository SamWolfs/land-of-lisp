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

## Loops

### Loop n times

```lisp
(loop repeat n
    collect 1) ;; returns (1 1 1 1 1 ...) or n times 1
```

### Loop with index

```lisp
(loop for i from 1 to 10
    collect i) ;; returns (1 2 3 ... 10)
```

As is normal in Lisp, we tend to prefer returning values from our functions. In a loop, the return value is defined by `collect`.

## Arrays

Use the `make-array` command to make an array. Array are different from lists in that they're a fixed size and they can fetch the data at a certain position in a set amount of time; in case of a list, you have to iterate through all of the cons cells to get to your requested position. (Notice that an array is prefixed with a **#**)

```lisp
(make-array 3) ;; #(nil nil nil)
```

Lisp also supports *generic setters* through a special sublanguage called *generalized reference*. Because of this, the `setf` command can modify values based on their getters.

```lisp
(setf foo '(a b c)) ;; (A B C)
(second foo) ;; B
(setf (second foo) 'z)
(foo) ;; (A Z C)
```

`setf` uses the getter to look for *where* the data came from, and modifies it with the newly provided value. While it's possibly to use a wide range of getters, not everything is support.

```lisp
(setf foo (make-array 4)) ;; #(nil nil nil nil)
(setf (aref foo 2) '(x y z)) ;; #(nil nil (X Y Z) nil)
(setf (car (aref foo 2)) (make-hash-table))
(setf (gethash 'zoink (car (aref foo 2))) 5) ;; (nil nil (#S(HASH-TABLE (ZOINK . 5)) Y Z nil))
```

Furthermore, the `aref` command can be used to get the value at a certain position in an array.

## Hash table

While arrays are similar to lists, hash tables are more similar to alists in that they allow for storage of key-value-pairs and lookup of the stored items.

```lisp
(defparameter x (make-hash-table)) ;; #S(HASH-TABLE)
(gethash 'yup x) ;; nil; nil
(setf (gethash 'yup x) '25)
(gethash 'yup x) ;; 25; T
```

The `gethash` command has two return variables: the value associated with the key and whether or not a value was associated with the provided key. You can return multiple values by using the `values` command.

```lisp
(defun one-and-two ()
    (values 1 2))
(one-and-two) ;; 1; 2
```

Normally, only the first returned value will be used, but by using the `multiple-value-bind` command we can bind both return values.

```lisp
(multiple-value-bind (x y) (one-and-two)
    (+ x y)) ;; 3
```

## Structures

Structures acn be used to represent objects in an OOP way.

```lisp
(defstruct person
           name
           age
           waist-size
           favourite-colour)
```

This example defines a `person` structure with four properties (also called *slots*): `name`, `age`, `waist-size` and `favourite-colour`.
The `defstruct` command also creates a special function that allows creation of an instance.

```lisp
(defparameter *sam* (make-person :name "Sam"
                                 :age 26
                                 :waist-size 30
                                 :favourite-color "blue"));
*sam* ;; #S(PERSON :NAME "Sam" :AGE 26 :WAIST-SIZE 30 :FAVOURITE-COLOUR "blue")
```

Special getters are also created for every *slot* of the *structure*. (These getters can also be used with `setf` or `push` or any other command allowing generic setters)

```lisp
(person-name *sam*) ;; "Sam"
```

The values printed by the REPL (e.g. `#S(PERSON :NAME "Sam" :AGE 26 :WAIST-SIZE 30 :FAVOURITE-COLOUR "blue")`) can also be used to create a structure, the type will be inferred automatically. (JSON, say hello to LISP Object Notation?)

