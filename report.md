# 编译原理PA1-B实验报告

## 实验内容

基于 LL(1) 的语法分析与错误恢复。

1. 修改框架文件，将 PA1-A 中所述的所有新特性对应的文法改写为 LL(1)。
2. 使用一种介于二者之间的错误恢复方法，完成错误恢复。

## 实验过程

### LL（1）文法

#### abstract关键字

按照PA-1的代码，直接引入abstract关键字仍使得文法为LL(1)。不需要特殊的处理。

#### var关键字

在`SimpleStmt`的产生式添加`var`关键字。`var`的引入不需要特殊的文法上的处理。

#### First-class Functions

此处是本次实验的核心，在文法方面的处理相当多。

首先完成函数调用。

添加`ExprT8->'(' ExprList ')' ExprT8`，完成对函数调用的解析，将解析到的参数列表放在`thunkList`中。

随后，在`Expr8`和`AfterLParen`处，从`thunkList`中，完成函数调用的解析。

随后完成`Lambda`表达式。

这部分搬运PA1-A代码即可。需要注意的是，一共有两种形式的`Lambda`表达式，他们具有相同的前缀，因此需要提取公因子，完成解析。

最后完成函数类型。

此部分最为麻烦，我如下设计产生式：

```
Type            ->   AtomType ArrayType TLambdas
TLambdas -> '(' TypeList ')' ArrayType TLambdas|/*empty*/
```

如此设计在于`Lambda`表达式的小括号和数组的中括号可以交替出现。为了解决这个问题，在分析过程中，将其分解为一组一组可选的中括号和小括号，分别作为数组和`Lambda`表达式的部分作为分析，并将临时结果放在`thunkList`中。

### 错误恢复

## 思考题

