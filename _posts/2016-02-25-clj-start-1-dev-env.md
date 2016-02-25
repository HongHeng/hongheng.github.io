---
layout: post
categories: 程序小狼
excerpt: clojure 入门
title: clojure入门（1）——开发环境
---

#clojure入门（1）——开发环境

##软件及工具

1. 通用IDE：IntelliJ IDEA
2. IDEA中clojure插件：cursive
3. clojure项目管理工具：leiningen (可用Homebrew安装，方便)

##新建项目
1. File -> new Project -> leiningen -> ...
2. terminal:`lein new`

##REPL

1. Run -> Edit Configuration -> add -> Clojure REPL -> Local
2. Run -> Run "it"
3. 所见即所得的交互编程试验田诞生！

##实时测试

1. 使用midje.在project.clj中添加
```
  :profiles {:dev {:dependencies [[midje "1.6.3"]]  
                   :plugins [[lein-midje "3.1.3"]
                             [lein-aws-maven "0.1.0" :exclusions [joda-time]]]}}
```
2. View -> tool windows -> terminal
3. run `lein midje :autotest`
4. 写完测试用例即可实时测试，随写随测！
