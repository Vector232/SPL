# Задания по СЯП

## [Основы Языка Лисп](#1)
## [Функции высших порядков](#2)
## [Haskell. Списки.](#3)

<a name="1"></a>
# Основы Языка Лисп
## 2. Определите функцию, возвращающую последний элемент списка.
```
(DEFUN PRINTLASTELEMENT (x)
       (IF (NULL (CDR x))
           (CAR x)
           (PRINTLASTELEMENT (CDR x))))

(print (PRINTLASTELEMENT '(1 2 3 4 5 6 7 8)))
(print (PRINTLASTELEMENT '(1 2 3 4 5 6 7 (8 9 10 11))))
```
## 4. Определите функцию, порождающую по заданному натуральному числу N список, состоящий из натуральных чисел от 1 до N.
```
(DEFUN PrintFrom1ToN(n)
      (IF (ATOM n)
          (PrintFrom1ToN (CONS n '()))
          (IF (EQL (CAR n) 1)
              n
              (PrintFrom1ToN (CONS (- (CAR n) 1) n))
              )
          )
       )
       
(print (PrintFrom1ToN '(1)))
(print (PrintFrom1ToN '(10)))
```
## 18. Определите предикат, проверяющий, является ли аргумент одноуровневым списком.
```
(DEFUN SIMPLLIST (n)
       (IF (NULL n)
           T
           (IF (ATOM (CAR n))
               (SIMPLLIST (CDR n))
               NIL
               )
           )
       )

(print (SIMPLLIST '(1 2 3 4 5)))
(print (SIMPLLIST '(1 2 (1 2 3 4) 4 5)))
(print (SIMPLLIST '(1 2 (1 2 (1 2 3 4) 4) 4 5)))
```
## 23. Определите функции, преобразующие список (a b с) к виду (а (b (с))) и наоборот.
```
(DEFUN DeFractalithation  (x &optional y) 
  (COND ((NULL x) y)
        ((ATOM x) (CONS x y))
        ((DeFractalithation (CAR x) (DeFractalithation (CDR x) y)))))

(print (DeFractalithation '(1 (2 (3 (4 (5)))))))

(DEFUN Fractalithation (x)
  (IF (NULL (CDR x))
      x
      (REVERSE (LIST (Fractalithation (CDR x)) (CAR x)))
      )
  )
 
(print (Fractalithation '(1 2 3 4 5)))
```
## 25. Определите функцию, удаляющую из списка каждый четный элемент
```
(defun DeleteEven (x &optional n y)
    (cond ( (EQ n -1) y )
        ( (EQ n nil) (DeleteEven x (- (list-length x) 1)) )
        ( (EQ (MOD n 2) 0) (DeleteEven x (- n 1) (CONS (nth n x) y)) )
        ( (DeleteEven x (- n 1) y) )
        )
    )

(print (DeleteEven '(1 2 3 4 5 6 7 8 9)))
(print (DeleteEven '(a b c d e f g h k)))
```
## 27. Определите функцию, которая, чередуя элементы списков (a b...) и (1 2...), образует новый список (a 1 b 2 ...)
```
(defun f(x y &optional i j z)
    (COND ( (EQ i nil) (f (REVERSE  x) (REVERSE  y) (- (list-length x) 1) (- (list-length y) 1)) )
          ( (AND (EQ i -1) (EQ j -1)) (REVERSE z) )
          ( (AND (EQ i -1) (> j -1)) (f x y -1 (- j 1) (CONS (nth j y) z)) )
          ( (AND (EQ j -1) (> i -1)) (f x y (- i 1) -1 (CONS (nth i x) z)) )
          ( t (f x y (- i 1) (- j 1) (CONS (nth i x) (CONS (nth j y) z))) )
          )
    )

(print (f '(a b c d) '(1 2 3 4 5)))
(print (f '(a b c d e f g h) '(1 2 3 4 5)))
```
## 37. Определите функцию ПЕРЕСЕЧЕНИЕ, формирующую пересечение двух множеств, т.е. множество из их общих элементов.
```
(defun IntersectionXAndY (x y &optional i j z)
    (COND ( (EQ i nil) (IntersectionXAndY x y (- (list-length x) 1) (- (list-length y) 1) '()) ) ;start
          ( (EQ i -1) z ) ;exit
          ( (EQ j -1)  (IntersectionXAndY x y (- i 1) (- (list-length y) 1) z) )
          ( (EQ (nth i x) (nth j y)) (IntersectionXAndY x y i (- j 1) (CONS (nth i x) z)) ) ;add element
          ( t (IntersectionXAndY x y i (- j 1) z) )
          )
    )

(print (IntersectionXAndY '(1 2 3 4 5) '(a d c d e)))
(print (IntersectionXAndY '(1 2 3 4 5) '(a 1 c d 2)))
```
## 40. Определите функцию РАЗНОСТЬ, формирующую разность двух множеств, т.е. удаляющую из первого множества все общие со вторым множеством элементы.
```
(defun f (x y &optional i j z)
    (COND ( (EQ i nil) (f x y (- (list-length x) 1) (- (list-length y) 1)) ) ;start
          ( (EQ i -1) z ) ;exit
          ( (EQ j -1)  (f x y (- i 1) (- (list-length y) 1) (CONS (nth i x) z)) ) ;add element
          ( (EQUAL (nth i x) (nth j y)) (f x y (- i 1) (- (list-length y) 1) z) ) 
          ( t (f x y i (- j 1) z) )
          )
    )

(print (f '(a b (a c) d e) '(r (a c) y b s e)))
```

## 44. Определите функцию, проверяющую, является ли одно дерево поддеревом второго.
```
(defun XSubtreeOfY (x y &optional n ans)
    (COND ( (EQ n nil) (COND ( (EQUAL x y) t )
                             ( t (XSubtreeOfY x y (- (list-length y) 1)) )
                             ) 
                       )
          ( (EQ ans t) t )
          ( (EQ n -1) nil )
          ( (IF (ATOM (nth n y))
                (XSubtreeOfY x y (- n 1))
                (IF (EQUAL x (nth n y))
                    t
                    (XSubtreeOfY x y (- n 1) (XSubtreeOfY x (nth n y)))
                ) )
           )
          )
    )
(print (XSubtreeOfY '(a b c d) '(a b c d)))
(print (XSubtreeOfY '(k l (u y (w w))) '(a (d c) (a b c d (a b c d (a b c d (k l (u y (w w)))))) (w b c d)) ))
(print (XSubtreeOfY '(l (u y (w w))) '(a (d c) (a b c d (a b c d (a b c d (k l (u y (w w)))))) (w b c d)) )) ;без первого эл. у поддерева.
```
## 47. Определите функцию УДАЛИТЬ-ВСЕ-СВОЙСТВА, которая удаляет все свойства символа.
```
(setf (get 'a '1) '(a))
(setf (get 'a '2) '(b))
(setf (get 'a '3) '(c))
(setf (get 'a '4) '(d e f))
(print "Было:")
(print (symbol-plist 'a))


(defun DeleteAllProperties (x)
    (COND ( (NULL (symbol-plist x)) (symbol-plist x) ) ;вывод списка свойств
          ( t (remprop x (CAR (symbol-plist x)))
              (DeleteAllProperties x))
          )
    )
(print "Стало:")
(print (DeleteAllProperties 'a))
```

<a name="2"></a>
# Функции высших порядков
## 1. Определите FUNCALL через функционал APPLY.

```
(defun -funcall (function &rest args) ;минусик для ранее определенных функций
    (apply function args)
    )

(print (funcall '+ 1 2 3 4 5))
```
## 3. Определите функционал (APL-APPLY f x), который применяет каждую функцию fi списка

```
(f1 f2 ... fn)
к соответствующему элементу списка
x = (x1 x2 ... xn)
и возвращает список, сформированный из результатов.

(defun f1 (x)
    (+ x 25)
    )

(defun f2 (x)
    (apply '+ x)
    )

(defun APL-APPLY (f x)
    (IF (NULL f)
        nil
        (CONS (funcall (car f) (car x)) (APL-APPLY (CDR f) (CDR x)) )
        )
    )

(print (APL-APPLY '(f1 f1 f1 f2 f2) '(1 2 3 (1 2 3) (1 2 3 4))))
```
## 5. Определите функциональный предикат (НЕКОТОРЫй пред список), который истинен, когда, являющейся функциональным аргументом предикат пред истинен хотя бы для одного элемента списка список.
```
(defun f1 (x) ;если 0 то истина иначе ложь
    (if (EQ x 0)
        t
        nil
        )
    )

(defun -Some (f x)
    (if (f (CAR x))
        t
        (Some f (CDR x)))
    )
;если есть ноль то истина иначе ложь
(print (Some 'f1 '(3 1 2 3 6 5 1 7 8 9 3)))
(print (Some 'f1 '(0 1 2 3 6 5 1 7 8 9 3)))
(print (Some 'f1 '(3 1 2 3 6 5 1 7 8 9 0)))
(print (Some 'f1 '(3 1 2 0 6 5 0 7 8 9 3)))
```
## 7. Определите фильтр (УДАЛИТь-ЕСЛИ-НЕ пред список), удаляющий из списка список все элементы, которые не обладают свойством, наличие которого проверяет предикат пред.
```
(setf (get 'a '2) "lol")
(setf (get 'b '2) "kek")
(setf (get 'c '3) "cheburek")
(setf (get 'e '2) "cheburek")

(defun f1 (x)
    (IF (EQ (GET x '2) nil)
        nil
        t)
    )

(defun DeleteIfNot (f x) 
    (COND ( (NULL x) nil )
          ( (funcall f (CAR x)) (CONS (CAR x) (DeleteIfNot f (CDR x))) )
          (t (DeleteIfNot f (CDR x)) )
          )
    )

(print (DeleteIfNot 'f1 '(a b c e)) )
```
## 9. Напишите генератор порождения чисел Фибоначчи: 0, 1, 1, 2, 3, 5,
```
(let ( (x 1) (y 0) )
     (defun GenerateFibonachy () (let ((temp x)) (setq x (+ x y) y temp)))
     )


(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
(print (funcall 'GenerateFibonachy))
```
## 11. Определите фукнционал МНОГОФУН, который использует функции, являющиеся аргументами, по следующей схеме: (МНОГОФУН ’(f g ... h) x) ? (LIST (f x) (g x) ... (h x)).
```
(defun f1 (x)
    (sin x)
    )
(defun f2 (x)
    (cos x)
    )

(defun Multifun (f x)
      (COND ( (NULL f) nil )
            ( t (CONS (funcall (CAR f) x) (Multifun (CDR f) x)) )
             )
    )

(print (Multifun '(cos sin cos sin f1 f2) '1) )
```
## 13. Определите функцию, которая возвращает в качестве значения свое определение (лямбда-выражение).
```
(defun Request () (
                   (lambda (x)  (list 'defun 'Request () (list x (list x))))
                   '(lambda (x)  (list 'defun 'Request () (list x (list x))))
                   )
    )

(print (Request))
```

```
(defun mb () 
    (SYMBOL-FUNCTION 'mb)
    )

(print  (mb))
```
<a name="3"></a>
# Haskell. Списки.
## 9. Определите функцию, которая обращает список (а b с) и разбивает его на уровни (((с) b) а).
```
fractalithation [] = ""
fractalithation (x:')':y) = x:')':y
fractalithation (' ':t) = "" ++ fractalithation t
fractalithation (')':t) = " )" ++ t
fractalithation ('(':t) = " (" ++ fractalithation t
fractalithation (h:t) = h : mod_tail
    where mod_tail = " (" ++ (fractalithation t) ++ ")"

main = print.fractalithation $ "(a b c d)"
```
