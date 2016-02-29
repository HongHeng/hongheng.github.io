---
layout: post
categories: 程序小狼
excerpt: 协议 记录 多重方法
title: clojure入门（6）——协议与多重方法
---

##clojure入门（6）——协议与多重方法

```
;--------------------------------------
; 协议
;--------------------------------------

(defprotocol my-p "desc"
  (method-first [this] "desc")
  (method-second [this b] "desc"))

(defrecord my-r [x]
  my-p
  (method-first [this]
    (prn x this))
  (method-second [this b]
    (prn x this b)))

(comment
  (let [r (->my-r 1)]
    (method-first r)
    (method-second r 2)))

;--------------------------------------
; 多重方法
;--------------------------------------

(defmulti build-r "desc" :type)

(defmethod build-r :type1
  [{:keys [type data]}]
  (->my-r data))

(comment
  (let [r (build-r {:type :type1 :data 1})]
    (method-first r)
    (method-second r 2)))
```
