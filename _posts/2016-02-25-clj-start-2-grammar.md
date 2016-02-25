---
layout: post
categories: 程序小狼
excerpt: clojure 入门
title: clojure入门（2）——语法与数据结构
---

#clojure入门（2）——语法与数据结构

##特点

1. 动态类型
2. 前缀表达式
3. 小括号定义代码范围
5. 注释 `;`
6. 逗号＝空格

##数据

1. 空 `nil`
2. 数字
2. 字符 `\c`
2. 字符串 `"abc"`
3. 关键字 `:a :1`
3. 列表 list `'(1 2 3)`
4. 向量 vector `[1 2 3]`
5. 表 map `{:ab 12 :12 "ab"}`
6. 集合 set `#{1 2 3}`
4. 正则表达式 `#"pattern[0-1]"`

##绑定（赋值）

- 全局变量 `(def a 1) (def x {:a 1 :b 2})`
- 局部变量 `(let [a 1 x {:a 1 :b 2}] (prn a x))`
- 解构 
```
(let [{:keys [a b c] :or {a 1 c 1}}  {:a 1 :b 2}
             [_ y z] [1 2 3]]
  (prn a b c y z))
```

##函数
```
(def my-prn0 (fn [a] (prn a)))
(defn my-prn1 [a] (prn a))
(defn my-prn2 [a & other] (prn a other)) ;可变参数
(defn my-prn3 [[_ a b]] (prn a b)) ;可直接对入参解构
```
