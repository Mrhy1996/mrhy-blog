---
title: java编译器和懒加载
toc: true
date: 2021-10-25 14:33:41
tags: jvm
url: jvm-jit
---

1. 针对java是一种编译性语言还是解释性语言做出解释
2. 针对java的懒加载做出解释

## 解释器

bytecode  intepreter

JIT

Just In- Time compiler

混合模式

- 混合使用解释器+热点代码编译

- 起始阶段采用解释执行

- 热点代码检测

  多次被调用的方法（方法计数器：监测方法的执行频率）

  多次被调用的循环（循环计数器：检测循环执行频率）

  进行编译

可以在启动中设置解释器的模式

- -Xmixed 默认为混合模式，开始解释执行，启动速度较快，对热点代码实行检测和编译
- -Xint 使用解释模式，启动很快，执行稍慢
- -Xcomp 使用纯编译模式，启动很慢，执行很快。

## 懒加载

- JVM规范并没有规定何时加载
- 但是严格规定了什么时候必须初始化
  - new  getstatic  putstatic  invokestatic指令，访问final变量除外
  - java.lang.reflect对类进行反射调用时
  - 初始化子类时，父类首先初始化
  - 虚拟机启动时，被执行的主类必须初始化
  - 动态语言支持 java.lang.invoke.MethodHandle解析的结果为REF_getstatic REF_putstatic REF_invokestatic的方法句柄时，该类必须初始化。
