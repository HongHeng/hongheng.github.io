---
layout: post
categories: 程序小狼
excerpt: 编程 范式
title: clojure入门（8）——编程相关
---

##clojure入门（8）——编程相关

```
;--------------------------------------
; 函数式编程
;--------------------------------------

;1 将函数视为一种数据
;2 尽量扩大纯函数的范围,缩小非纯函数的影响

(let [my-f (fn [a b f] (f a b))]
  (map prn [(my-f 1 2 +)
            (my-f 1 2 *)
            (my-f 1 2 prn)]))

;--------------------------------------
; 流式编程
;--------------------------------------

(->> (range 9)
     (map inc)
     (reduce +)
     (prn "45 = "))
   
; -> ->> as-> some->

(cond->
  9
  true (* 2)
  false (inc)
  true (prn " = 18"))
  
;--------------------------------------
; 面向测试
;--------------------------------------

; 1.先写测试用例,再写逻辑;
; 2.集成测试最重要,单元测试辅助之；

```

###其他
1. 逻辑函数尽量划分为功能唯一并简单的小函数
2. 函数内聚性要高，推敲参数表，最好不超过3个
3. 纯函数编程中应避免过多分支和if嵌套，多用流式编程
4. 逻辑函数尽量不做防御式处理，而通过断言`:pre`实现

