---
layout: post
categories: 程序小狼
excerpt: 顺序 条件 循环 判断
title: clojure入门（3）——控制与判断
---

##clojure入门（3）——控制与判断

###控制

* 顺序

```
(do (prn 1) (prn 2))
;1 2

```

* 条件

```

(if true (prn 1) (prn 2))
;1

(when true (prn 1) (prn 2))
;1 2

;if-let, if-not, if-some
;when-not, when-let, when-some

(case 2
  1 "a"
  2 "b"
  :else "c")
;=> "b"

(cond
  false "a"
  true "b"
  :else "c")
;=> "b"

(condp #(%1 %2) :foo
  string? "it's a string"
  keyword? "it's a keyword"
  symbol? "it's a symbol"
  fn? "it's a function"
  "something else!")
;;=> "it's a keyword"

```

* 循环

```
;--------------------------------------
; 简单循环
;--------------------------------------

(let [i (atom 3)]
  (while (pos? @i)
    (prn @i)
    (swap! i dec)))
;3 2 1
;=> nil

(dotimes [i 2] (prn i))
;0 1
;=> nil

(doseq [i (range 2)] (prn i))
;0 1
;=> nil

(for [i (range 2)] (do (prn i) i))
;0 1
;=> (0 1)

(repeatedly 2 #(do (prn 1) 1))
;1 1
;=> (1 1)

(repeat 2 3)
;=> (3 3)

(range 3 7)
;=> (3 4 5 6)

;--------------------------------------
; mapcat(常用)
;--------------------------------------

(reduce + [1 2 3])
;=> 6

(map inc [1 2 3])
;=> (2 3 4)

(filter pos? [1 -1 2 -2 3 -3])
;=> (1 2 3)

(remove pos? [1 -1 2 -2 3 -3])
;=> (-1 -2 -3)

(take-while pos? [1 -1 2 -2 3 -3])
;=> (1)

(drop-while pos? [1 -1 2 -2 3 -3])
;=> (-1 2 -2 3 -3)

(split-with pos? [1 -1 2 -2 3 -3])
;=> [(1) (-1 2 -2 3 -3)]

;--------------------------------------
; 递归
;--------------------------------------

(loop [i 1]
  (prn i)
  (when (< i 2)
    (recur (inc i))))
;1 2
```

###判断

* 相等：对象`identical?` 引用`＝` 数字`==`
* 基本逻辑 `and or not not=`
* 数据类型判断 `seq? list? vector? coll? string? keyword? symbol? fn?`
* 空判定 `nil? some? empty? seq`
* 数值判断 `even? odd? pos? neg? zero?`
* 多重判定 `every? some not-every? not-any?`