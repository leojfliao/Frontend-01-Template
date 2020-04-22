# 第 02 周学习笔记

## 一. 语言按语法分类

1. 非形式语言
   - 中文，英文
2. 形式语言（乔姆斯基谱系）
   - **0 型**：无限制文法
   - **1 型**：上下文相关文法
   - **2 型**：上下文无关文法
   - **3 型**：正则文法

### 1.1 产生式（BNF）

> 作用：掌握 BNF 产生式有助于阅读和理解任何一门计算机语言的官方文档提供的语法定义。

BNF（Backus-Naur Form 巴克斯-诺尔范式）是一种用于表示上下文无关文法的语言，可以用来描述**编程语言的文法**。BNF 规定是推导规则（产生式）的集合，写法如下：

```ini
符号 ::= <使用符号的表达式>
```

说明：

1. <符号> 是**非终结符**，而**表达式**由一个符号序列，或用指示选择的竖杠 '|' 分隔的多个符号序列构成；
2. 每个符号序列整体都是左端的符号的一种可能的替代；
3. 从未在左端出现的符号叫做终结符。

概念：

- **终结符**：终结符是一个形式语言的基本符号；
- **非终结符**：非终结符是可以被取代的符号。

#### BNF 范式的语法

```ini
< >     : 内包含的为必选项，表示语法结构名（可以是`终结符`或`非终结符`）。
[ ]     : 内包含的为可选项。
{ }     : 内包含的为可重复 0 至无数次的项。
|       : 表示在其左右两边任选一项，相当于 "OR" 的意思。
::=     : 是“被定义为”的意思
"..."   : 术语符号（终结符）
[...]   : 选项，最多出现一次
{...}   : 重复项，任意次数，包括 0 次
(...)   : 分组
|       : 并列选项，只能选一个
*       : 表示重复多次
+       : 表示至少一次
```

#### BNF 练习

- **练习 1**：定义一个语言由若干字母 `a` 或 `b` 任意组成

  ```ini
  <Program> ::= ("a"+ | "b"+)+
  ```

- **练习 2**：定义数字、十进制数，以及加法表达式

  ```ini
  # 数字
  <Number> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

  # 十进制数
  <DecimalNumber> ::= "0" |
    ("1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9") <Number>*)

  # 加法
  <Expression> ::= <DecimalNumber> |
    <Expression> "+" <DecimalNumber>
  ```

- **练习 3**：带括号的四则运算

  ```ini
  # 数字
  <Number> ::= "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"

  # 十进制数
  <DecimalNumber> ::= "0" |
    ("1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9") <Number>*)

  # 括号
  <PrimaryExpression> ::= <DecimalNumber> |
    "(" <LogicalExpression> ")"

  # 乘法 / 除法
  <MultiplicativeExpression> ::= <PrimaryExpression> |
    <MultiplicativeExpression> "*" <PrimaryExpression> |
    <MultiplicativeExpression> "/" <PrimaryExpression>

  # 加法 / 减法
  <AdditiveExpression> ::= <MultiplicativeExpression> |
    <AdditiveExpression> "+" <MultiplicativeExpression> |
    <AdditiveExpression> "-" <MultiplicativeExpression>

  # 逻辑运算
  <LogicalExpression> ::= <AdditiveExpression> |
    <LogicalExpression> "||" <AdditiveExpression> |
    <LogicalExpression> "&&" <AdditiveExpression>
  ```

### 1.2 通过产生式理解乔姆斯基谱系

- **0 型**：无限制文法

  - `? ::= ?`
  - 例：`<a> <b> ::= "c" <d>`

- **1 型**：上下文相关文法

  - `?<A>? ::= ?<B>?`
  - 例：`"a" <b> "c" ::= "a" "x" "c"`

- **2 型**：上下文无关文法

  - `<A> ::= ?`
  - 提示：JavaScript 的大部分语法都符合 2 型文法

- **3 型**：正则文法
  - `<A> ::= <A>?`
  - 只允许左递归
  - 提示：JavaScript 的大部分表达式都符合 3 型文法

#### 正则文法练习

> 小经验

1. 用正则表达式对代码扫描就是词法分析，然后根据产生式建立抽象语法树，最后基于语法树做语法分析；
2. JS 的正则效率不高，由于支持回溯，因此 JS 的正则不和字符串的长度成正比，有可能会是平方关系。

**练习要求**：使用正则文法重写 BNF 练习的四则运算。

```ini
# 十进制数
<DecimalNumber> ::= /0|[1-9][0-9]*/

# 括号
<PrimaryExpression> ::= <DecimalNumber> |
"(" <LogicalExpression> ")"

# 乘法 / 除法
<MultiplicativeExpression> ::= <PrimaryExpression> |
<MultiplicativeExpression> "*" <PrimaryExpression> |
<MultiplicativeExpression> "/" <PrimaryExpression>

# 加法 / 减法
<AdditiveExpression> ::= <MultiplicativeExpression> |
<AdditiveExpression> "+" <MultiplicativeExpression> |
<AdditiveExpression> "-" <MultiplicativeExpression>

# 逻辑运算
<LogicalExpression> ::= <AdditiveExpression> |
<LogicalExpression> "||" <AdditiveExpression> |
<LogicalExpression> "&&" <AdditiveExpression>
```

### 1.3 其他产生式

> 提示：JavaScript 的语法定义并不是 `BNF`，在官方文档中使用**粗体**表示**终结符**。

- EBNF
- ABNF
- Customized

#### 现代语言的特例

- C++ 中，`*` 可能表示乘号或者指针，具体是哪个，取决于星号前面的标识符是或否被声明为类型；
- VB 中，`<` 可能是小于号，也可能是 XML 直接量的开始，取决于当前位置是否可以接受 XML 直接量；
- Python 中，行首的 `tab` 符号和空格会根据上一行的行首空白以一定规则被处理成虚拟终结符 `indent` 或者 `dedent`；
- JavaScript 中，`/` 可能是除号，也可能是正则表达式的开头，处理方式类似于 VB，字符串模板中也需要特殊处理，还有自动插入分号规则。

> 练习：尽可能寻找你知道的计算机语言，尝试把它们分类。

## 二. 语言扩展

### 2.1 图灵完备性

#### 图灵机

- 凡事可计算的都能计算出来（世界上不是每一件事情都是可计算的）；
- 可以被看作是等价于任何有限逻辑数学过程的终极强大逻辑机器。

#### 图灵完备性

- 命令式 —— 图灵机

  - `goto`
  - `if` 和 `while`

- 声名式 —— `lambda`
  - 递归

### 2.2 动态与静态

#### 动态语言

- 在用户的设备 / 在线服务器上
- 产品实际运行时
- Runtime

#### 静态语言

- 在程序员的设备上
- 产品开发时
- Compiletime

### 2.3 类型系统

- 动态类型系统和静态类型系统
- 强类型与弱类型
  - 弱类型有隐式转换，例如：`String + Number` / `String == Boolean`
- 复合类型
  - 结构体 / 函数签名
- 子类型
  - 逆变 / 协变

### 2.4 一般命令式编程语言

- Atom
  - Identifier / Literal
- Expression
  - Atom / Operator / Punctuator
- Statement
  - Expression / Keyword / Punctuator
- Structure
  - Function / Class / Process / Namespace / ...
- Program
  - Program / Module / Package / Library

## 三. JavaScript

- Atom
- Expression
- Statement
- Structure
- Program / Module

### 3.1 源代码

```ini
SourceCharacter :: any Unicode code point
```

> 提示：JavaScript 的字符集使用的是 `UCS` 字符集（`U+0000` ~ `U+FFFF`），超出 `U+FFFF` 范围的例如 `emoji`，会被当做两个字符。

#### [Blocks](https://www.fileformat.info/info/unicode/block/index.htm) 编码组

- `0 ~ U+007F`：常用拉丁字符
- `U+4E00 ~ U+9FFF`：CJK（Chinese + Japanese + Korean 三合一）
  - 有一些增补区域（extension）
- `U+0000 - U+FFFF`：[BMP - Unicode 字符平面映射](https://zh.wikipedia.org/wiki/Unicode%E5%AD%97%E7%AC%A6%E5%B9%B3%E9%9D%A2%E6%98%A0%E5%B0%84)

#### 相关方法

- `String.fromCharCode(code)` 返回由指定的 UTF-16 编码创建的字符串
- `str.codePointAt(0)` 返回一个 Unicode 编码点值的非负整数

示例代码：

```javascript
// 把“中”对应的编码数值，转成十六进制字符串：4e2d
'中'.charCodeAt(0).toString(16)

// 输出中文的“中”
console.log('\u4e2d')
```

### 3.2 输入元素

```ini
InputElement ::
  WhiteSpace
  LineTerminator
  Comment
  Token

Token ::
  Punctuator
  IdentifierName ::
    Keywords
    Identifier
    Future reserved Keyword: enum
  Literal ::
    Number
    String
    Boolean
    Object
    Null
    Undefined
    Symbol
```

#### [WhiteSpace](https://www.fileformat.info/info/unicode/category/Zs/list.htm)

- `nbsp`：No break space（无分词效果）`\u00a0`
- `space`: 空格 `\u0020`

#### IdentifierName

- 标识符，可以以**字母**、`_` 或者 `$` 开头，代码中用来标识**[变量](https://developer.mozilla.org/en-US/docs/Glossary/variable)、[函数](https://developer.mozilla.org/en-US/docs/Glossary/function)、或[属性](https://developer.mozilla.org/en-US/docs/Glossary/property)**的字符序列
  - 变量名：不能用 `Keywords`
  - 属性名：可以用 `Keywords`

### 3.3 Type

#### Number

##### [IEEE 754 双精度浮点数](https://zh.wikipedia.org/wiki/IEEE_754)

1. `Sign` 符号位（`1`）
2. `Exponent` 指数字（`11`）
3. `Fraction` 有效数字（`52`）

##### 各种进制的写法

- 二进制：`0b011`
- 八进制：`0o10`
- 十六进制：`0xFF`
- 十进制：`0`，`0.`，`.2`，`1e3`

> 小经验

1. 查看一个数字对应的二进制值：`97 .toString(2)` （**注意**，`97` 后面要有一个空格）
2. 比较浮点是否相等：`Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON`
3. 最大安全整数：`Number.MAX_SAFE_INTEGER.toString(16)`
4. 在开发**银行**系统时，==货币不要使用浮点数==，**货币值**要以**分**为单位，并且**都使用整数**
   - 如果存在一分钱的误差可能，需要对用户公示

#### String

##### 字符编码方案

- ASCII
- Unicode
- USC `U+0000 ~ U+FFFF`
- GB
  - GB2312
  - GBK（GB13000）
  - GB18030
- ISO-8859
- BIG5

> 提示：JavaScript 的字符集使用的是 `UCS` 字符集（`U+0000` ~ `U+FFFF`），超出 `U+FFFF` 范围的例如 `emoji`，会被当做两个字符。

##### 字符串语法

- ''
- ""
- [\`模板字符串\`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/template_strings) **推荐使用**

## 参考资料

- 静态和动态语言：<https://www.cnblogs.com/raind/p/8551791.html>
- 协变与逆变：<https://jkchao.github.io/typescript-book-chinese/tips/covarianceAndContravariance.html>
- Yacc 与 Lex 快速入门：<https://www.ibm.com/developerworks/cn/linux/sdk/lex/index.html>
- 关于元编程： <https://www.zhihu.com/question/23856985>
- 编程语言的自举：<https://www.cnblogs.com/lidyan/p/6727184.html>
- Unicode：<https://www.fileformat.info/info/unicode/>
- 正则表达式：<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions>
- 揭秘 0.1 + 0.2 != 0.3：<https://www.barretlee.com/blog/2016/09/28/ieee754-operation-in-js/>
- ASCII，Unicode 和 UTF-8：<http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html>

## 作业

1. 写一个能匹配所有 Number 直接量的正则
2. 写一个 UTF8_Encoding 方法
3. 写一个正则表达式来匹配字符串

```javascript
function UTF8_Encoding() {
  // return new  Buffer();
}
```
