---
layout: post
categories: 程序小狼
excerpt: 排序 函数 统计与重组
title: clojure入门（4）——常用函数
---

##clojure入门（4）——常用函数

###排序

```
(sort-by :a [{:a 3} {:a 2} {:a 4}])
;=> ({:a 2} {:a 3} {:a 4})

(sort-by (comp - :a) [{:a 3} {:a 2} {:a 4}])
;=> ({:a 4} {:a 3} {:a 2})

(sort #(- (:a %1) (:a %2)) [{:a 3} {:a 2} {:a 4}])
;=> ({:a 2} {:a 3} {:a 4})
```

###函数

```
comp
partial
complement
fnil
```

###统计与重组

```
frequencies
group-by
partition-by
flatten
```