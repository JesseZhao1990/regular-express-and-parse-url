# 总结正则表达式和解析url
## 解析url
#### 将url的查询参数解析为字典对象？
解决方案一般是拆字符串和用正则匹配。比价推荐的方法是正则匹配。因为url允许用户随意输入，如果用拆字符串的方法，有任何一个地方没考虑到容错，
就会导致程序出错。而正则就没有这个问题。它只匹配出正确的配对。非法的全部过滤掉。不过这里把两种方式都写一下。

**拆分字符串的方式**

```
function getUrlParms(){
     var args=new Object(); 
     var query=location.search.substring(1);//获取查询串 
     var pairs=query.split("&");    // 以&分割
     
     for(var i=0;i<pairs.length;i++){ 
         var pos=pairs[i].indexOf('=');         //查找name=value 
         if(pos==-1) continue;                  //如果没有找到就跳过 
         var argname=pairs[i].substring(0,pos); //提取name 
         var value=pairs[i].substring(pos+1);   //提取value 
         args[argname]=decodeURIComponent(value); //存为属性 
     }
     return args;
}
```

**正则的方式**

```
function getQueryObject(url){
  var urlStr=url==null?window.location.href:url;    //校验传进来的url。如果为null 则重新从window对象中取得
  var search = urlStr.substring(urlStr.lastIndexOf("?")+1);  //获取到问号后边的部分
  var obj={};
  var reg = /([^?&=]+)=([^?&=]*)/g;
  search.replace(reg,function(rs,$1,$2){
      var name = decodeURIComponent($1);
      var val = decodeURIComponent($2);
      obj[name]=String(val);
      return rs;
  });
  return obj;
}
```
明天解释正则~~
----

# 正则表达式 #
## 概述 ##
RegExp 构造函数可创建一个正则表达式对象，用特定的模式匹配文本。比如说你想验证一个字符串是不是手机号码？是不是应该判断是不是11位的数字呢？
## 语法 ##

```	

	RegExp(pattern [, flags])
	
	/pattern/flags


```
## 参数 ##

pattern

正则表达式文本

flags

该参数可以是下面几个值的任意组合：

g

全局匹配

i

忽略大小写

m

让开始和结束字符（^ 和 $）工作在多行模式（也就是，^ 和 $ 可以匹配字符串中每一行的开始和结束（行是由 \n 或 \r 分割的），而不只是整个输入字符串的最开始和最末尾处。

### 创建 ###
有两种方式可以创建正则对象：

1.构造函数

2.字面量

要表示字符串，字面量形式不使用引号，而传递给构造函数的参数使用引号。下面表达式创建相同的正则表达式：

```
	/ab+c/i;

	new RegExp('ab+c', 'i');

	new RegExp(/ab+c/, 'i');

```

## 正则表达式中的特殊字符 ##
正则表达式中的特殊字符包括：字符类别（Character Classes）、字符集合（Character Sets）、边界（Boundaries）、分组（grouping）、反向引用（back references）、数量词（Quantifiers）。

### 字符类别 ###
**字符：.**

含义：（点号，小数点）匹配任意单个字符，但是换行符除外，包括：\n \r \u2028 或 \u2029。

需要注意的是，m 多行（multiline）标志不会改变点号的表现。因此为了匹配多行中的字符集，可使用[^] （当然你不是打算用在旧版本 IE 中），它将会匹配任意字符，包括换行符。

例如，/.y/ 匹配 "yes make my day" 中的 "my" 和 "ay"，但是不匹配 "yes"。

**字符：\d**

含义：匹配基本拉丁字母表（basic Latin alphabet）中的一个数字字符。等价于[0-9]。

例如，/\d/ 或 /[0-9]/ 匹配 "B2 is the suite number." 中的 '2'。 


**字符：\D**

含义：匹配任意一个不是基本拉丁字母表中数字的字符。等价于[^0-9]。

例如，/\D/ 或 /[^0-9]/ 匹配 "B2 is the suite number." 中的 'B'。


**字符：\w**

含义：匹配任意来自基本拉丁字母表中的字母数字字符，还包括下划线。等价于 [A-Za-z0-9_]。

例如，/\w/ 匹配 "apple" 中的 'a'，"$5.28" 中的 '5' 和 "3D" 中的 '3'。

**字符：\W**

匹配任意不是基本拉丁字母表中单词（字母数字下划线）字符的字符。等价于 [^A-Za-z0-9_]。

例如，/\W/ 或 /[^A-Za-z0-9_]/ 匹配 "50%" 中的 '%'。

**字符：\S**

匹配一个非空白符。等价于 [^ \f\n\r\t\v​\u00a0\u1680​\u180e\u2000​\u2001\u2002​\u2003\u2004​ \u2005\u2006​\u2007\u2008​\u2009\u200a​\u2028\u2029​\u202f\u205f​\u3000]。

例如，/\S\w*/ 匹配 "foo bar" 中的 'foo'。

**字符：\t**

匹配一个水平制表符（tab）

**字符：\r**

匹配一个回车符（carriage return）

**字符：\n**

匹配一个换行符（linefeed）

**字符：\v**

匹配一个垂直制表符（vertical tab）

**字符：\f**

匹配一个换页符（form-feed）

**字符：\f**

匹配一个换页符（form-feed）

**字符：[\b]**

匹配一个退格符（backspace）（不要与 \b 混淆）


**字符：[\b]**

匹配一个退格符（backspace）（不要与 \b 混淆）

**字符：\xhh**

匹配编码为 hh （两个十六进制数字）的字符。

**字符：\uhhhh**

匹配 Unicode 值为 hhhh （四个十六进制数字）的字符。

**字符：\**

对于那些通常被认为字面意义的字符来说，表示下一个字符具有特殊用处，并且不会被按照字面意义解释。

例如 /b/ 匹配字符 'b'。在 b 前面加上一个反斜杠，即使用 /\b/，则该字符变得特殊，以为这匹配一个单词边界。

或

对于那些通常特殊对待的字符，表示下一个字符不具有特殊用途，会被按照字面意义解释。

例如，* 是一个特殊字符，表示匹配某个字符 0 或多次，如 /a*/ 意味着 0 或多个 "a"。 为了匹配字面意义上的 * ，在它前面加上一个反斜杠，例如，/a\*/匹配 'a*'。

### 字符集合 ###

**字符集合：[xyz]**

一个字符集合，也叫字符组。匹配集合中的任意一个字符。你可以使用连字符'-'指定一个范围。

例如，[abcd] 等价于 [a-d]，匹配"brisket"中的'b'和"chop"中的'c'。

**字符集合：[^xyz]**

一个反义或补充字符集，也叫反义字符组。也就是说，它匹配任意不在括号内的字符。你也可以通过使用连字符 '-' 指定一个范围内的字符。

例如，[^abc] 等价于 [^a-c]。 第一个匹配的是 "bacon" 中的'o' 和 "chop" 中的 'h'。

### 边界 ###

**边界：^**

匹配输入/字符串的开始。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符后的开始处。

例如，/^A/ 不匹配 "an A" 中的 "A"，但匹配 "An A" 中的 "A"。

**边界：$**

匹配输入/字符串的结尾。如果多行（multiline）标志被设为 true，该字符也会匹配一个断行（line break）符的前的结尾处。

例如，/t$/ 不匹配 "eater" 中的 "t"，但匹配 "eat" 中的 "t"。

**边界：\b**

匹配一个零宽单词边界（zero-width word boundary），如一个字母与一个空格之间。 （不要和 [\b] 混淆）

例如，/\bno/ 匹配 "at noon" 中的 "no"，/ly\b/ 匹配 "possibly yesterday." 中的 "ly"。

**边界：\B**

匹配一个零宽非单词边界（zero-width non-word boundary），如两个字母之间或两个空格之间。

例如，/\Bon/ 匹配 "at noon" 中的 "on"，/ye\B/ 匹配 "possibly yesterday." 中的 "ye"。

### 分组 ###

重复单字符我们可以使用限定符，如果重复字符串，用什么呢？ 对！用小括号，小括号里包裹指定字表达式（子串），这就是分组。之后就可以限定这个子表示式的重复次数了。












