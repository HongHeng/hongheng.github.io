---
layout: post
categories: 程序小狼
excerpt: clojure 入门
title: clojure入门（5）——并发与引用类型
---

##clojure入门（5）——并发与引用类型

```

;--------------------------------------
; delay future promise
;--------------------------------------

(let [d (delay :ok)]
  (map prn [(realized? d)
            (deref d)
            (realized? d)]))

(let [f (future (Thread/sleep 1000) :ok)]
  (map prn [(deref f 100 :unok)
            @f]))

(let [p (promise)]
  (map prn [(realized? p)
            (deref p 100 :unok)
            (deliver p :ok)
            (realized? p)
            @p
            (deliver p :again)
            @p]))

;--------------------------------------
; atom : 缓存
;--------------------------------------

(let [at (atom 1)
      m {:a 1}]
  (map prn [(swap! at inc)
            (reset! at m)
            (compare-and-set! at {:a 1} "")
            @at
            (compare-and-set! at m "")
            @at]))

;--------------------------------------
; watcher
;--------------------------------------

(let [watch (fn [key identity old new]
              (prn key identity old new))
      at (atom 1)]
  (add-watch at :x watch)
  (swap! at inc)
  (add-watch at :y watch)
  (reset! at {:a 1})
  (remove-watch at :x)
  (reset! at {:a 1}))

;--------------------------------------
; validator
;--------------------------------------

(let [at (atom 1 :validator pos?)]
  (->> [(swap! at inc)
        (reset! at 8)]
       (map prn)
       doall)
  (try (reset! at -1)
       (catch Exception e (prn "Exception:" (.getMessage e))))
  (set-validator! at (complement zero?))
  (reset! at -1)
  (prn @at)
  (try (reset! at 0)
       (catch Exception e (prn "Exception:" (.getMessage e)))))

;--------------------------------------
; ref : STM软件事务内存
;--------------------------------------
(let [player (fn [name item] {:name name :item (set item)})
      get-item (fn [from to]
                 (dosync ;事务内应该是纯函数!
                   (when-let [;item (-> from ensure :item first) ;write skew
                              item (-> from deref :item first)]
                     (commute to update :item conj item)
                     (alter from update :item disj item))))
      cc (ref (player "cc" (range 50)))
      lb (ref (player "lb" []))
      sq (ref (player "sq" []))]
  (future (while (get-item cc lb)))
  (future (while (get-item cc sq)))
  (Thread/sleep 100)
  (map prn [@cc
            @lb
            @sq
            (map (comp count :item deref) [cc lb sq])
            (filter (:item @lb) (:item @sq))
            (dosync (ref-set cc (player "cc" (range 50))))]))

;--------------------------------------
; agent : io操作 & STM感知
;--------------------------------------

(let [ag (agent 0)]
  (map prn [(send ag range 3) ; send-off
            @ag
            (await ag)
            @ag]))

(let [ag (agent 3 :error-mode :fail)]
  (map prn [(send ag / 0)
            (agent-error ag)
            (do (Thread/sleep 100) (agent-error ag))
            (restart-agent ag 0)
            (agent-error ag)]))

(let [ag (agent 3 :error-mode :continue :error-handler prn)]
  (map prn [(send ag / 0)
            (agent-error ag)
            (do (Thread/sleep 100) (agent-error ag))]))

```
