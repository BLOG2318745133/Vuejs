
# 《JavaScript正则表达式迷你书》

## 第一章 正则表达式  字符匹配

> 正则表达式是匹配模式，要么匹配字符，要么匹配位置！

- 横向匹配  ``/ab[2,5]/c/g ``  匹配 ``abc, abbc,abbbc,abbbbc,abbbbbc``，数字连续出现 2 到 5 次，会匹配 2 位、3 位、4 位、5 位连续数字

- 纵向匹配 ``/a[1,2,3]c/`` 匹配 ``a1c,a2c,a3c``

- 范围表示法  ``[1-3a-dE-G]``  匹配  ``[123abdcEFG]``

- 排除字符组 ``[^abc]`` 匹配 除``a`` 、``b``、``c`` 之外的任意一个字符

#### 1.1 **常见简写形式**

| 字符组 | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| ``\d`` | 表示 ``[0-9]` ，digit（数字）的首字母 d，匹配数字            |
| ``\D`` | 表示``[^0-9]`` , 匹配 除数字以外的任意字符                   |
| ``\w`` | 表示``[0-9a-zA-Z]`` ，word（单词）的首字母，匹配 数字、大小写字母、下划线 |
| ``\W`` | 表示``[^0-9a-zA-Z]`` ， 匹配非单词字符                       |
| ``\s`` | 表示``[\t\v\n\r\f]`` ， 匹配空白符、空格、制表位、换行符、回车符、换页符 |
| ``\S`` | 表示``[^\t\v\n\r\f]`` , 匹配非空白符                         |
| ``^``  | 表示开头，如果单独使用，表示 **非**                          |
| ``$``  | 表示结尾，例 ``/^([01][0-9]|[2][0-3]):[0-5][0-9]$/``         |

| 量词     | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| ``{m,}`` | **至少**出现 ``m`` 次                                        |
| ``{m}``  | 等价于``{m,m}`` , 出现 ``m`` 次                              |
| ``?``    | 等价于``{0,1}`` ， 出现或者不出现（您吃了吗？吃了/没吃）     |
| ``+``    | 等价于 ``{1,}`` , **至少**出现 1 次（每次只+1）              |
| ``*``    | 等价于``{0,}`` , 出现任意次，也有可能不出现（*星星 可能出现很多，也可能不出现） |

🌰例子1️⃣：要匹配以下👇**16进制颜色**🎨

```CSS
#ffbbad
#Fc01DF
#FFF
#ffE
```

```javascript
// 就需要这样写
var regex = '/#([0-9a-zA-Z]{6} | [0-9a-zA-Z]{3})/g';
var string = "#ffbbad #Fc01DF #FFF #ffE";
console.log(string.match(regex));
// ["#ffbbad #Fc01DF #FFF #ffE"]
```

🌰例子2️⃣：要匹配以下👇**时间**⏳

```css
23:59
02:08
```

```javascript
// 需要这样写
var regex = /^([01][0-9]|[2][0-3]):[0-5][0-9]$/;
// 第一位数字可以为 [0-2],当第 1 位为 "2" 时，第 2 位可以为 [0-3]，
// 其他情况时，第 2 位为 [0-9], 第3位数字为[0-5], 第 3 位数字为 [0-5]，第4位为 [0-9]。
console.log(regex.text("23:59")); // true
console.log(regex.text("02:08")); // true
```

🌰例子3️⃣：匹配以下👇**年月日**⏱️

```css
2021-08-05
```

```javascript
var regex = /^[0-9]{4}-(0[1-9]|1[0-2])-(0[1-9]|[12][0-9]|3[01])$/;
console.log( regex.test("2021-08-05"));//true
```

🌰例子4️⃣：匹配以下👇**文件路径**📂

```css
F:\study\javascript\regex\regular expression.pdf
F:\study\javascript\regex\
F:\study\javascript
F:\
```

```javascript
var regex = /^[a-zA-Z]:\\([^\\:*<>|"?\r\n/]+\\)*([^\\:*<>|"?\r\n/]+)?$/;
/* 匹配 ‘F:\’，需要使用 [a-zA-Z]:\\ 
   匹配 ‘文件名’， [^\\:*<>|"?\r\n/] 表示合法字符
   匹配 ‘文件夹\’，可用 [^\\:*<>|"?\r\n/]+\\
   匹配 多个 ‘文件夹\’，可用 ([^\\:*<>|"?\r\n/]+\\)*
*/
console.log( regex.test("F:\\study\\javascript\\regex\\regular expression.pdf") );
console.log( regex.test("F:\\study\\javascript\\regex\\") );
console.log( regex.test("F:\\study\\javascript") );
console.log( regex.test("F:\\") );
// => true
// => true
// => true
// => true
```

🌰例子5️⃣：匹配👇  **id**  🆔

```html
<div id="container" class="main"></div>
```

```javascript
var regex = /id="."?/;
var string = '<div id="container" class="main"></div>';
console.log(string.match(regex)[0]); // id="container"
```



## 第二章  正则表达式  位置匹配

> 正则表达式是匹配模式，要么匹配字符，要么匹配位置！

#### 2.1 what is position?（什么是位置？）

> 位置又被称为（锚），是相连字符之间的位置。如下所示👇

![position](https://pic2.zhimg.com/80/v2-c91bf328ebaf0519c953a3b3d53b6876_720w.png)



#### 2.2 How to match positions？(怎么匹配位置？)

> ES5 中的 6 个锚：

| 锚        | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| ``^``     | 脱字符，匹配开头                                             |
| ``$``     | 美元字符，匹配结尾                                           |
| ``\b``    | 单词边界， ``^``与 ``\w`` 、``\w`` 与 ``\W``  、``\W``与``$``之间的位置 |
| ``\B``    | 非单词边界                                                   |
| ``(?=p)`` | 子模式，p 前面的位置，比如 ``(?=l)``，表示 "l" 字符前面的位置，``he#l#lo`` |
| ``(?!p)`` | 子模式反面，``\#h#ell#o#``                                   |



#### 3.3 位置的特性

> 可以理解为空字符 ``""``，字符之间可以写成多个。

```javascript
// "hello"可以写成："hello" == "" + "h" + "" + "e" + "" + "l" + "" + "l" + "" + "o" + "";
var result = /^^hello$$$/.test("hello");
console.log(result);//true
```

🌰例子1️⃣：数字千位分隔符

```javascript
// 把 123456 变成  123,456,789 
var result = "123456".replace(/(?=\d{3}+$)/g,','));
consle.log(result);// 123,456.789
```

🌰例子2️⃣：货币格式化

```javascript
// 把1888 格式化成 $1888
function foemat(num){
	return num.toFixed(2).replace(/\B(?=(\d{3})+\b)/g,',').replace(/^/,"$$");
};
console.log(format(1888));// "&1,888,00"
```

🌰例子3️⃣：验证密码

```javascript
// 至少包括两种字符
var regex = /^[0-9A-Za-z]{6,12}$/;
```

![image-20210806153955569](https://pic3.zhimg.com/80/v2-5de1912ec6ff62f99a6a830c05413216_720w.png)

```javascript
// 包含某一种字符
var regex = /(?=.*[0-9])^[0-9A-Za-z]{6,12}$/;
```

![image-20210806155004047](https://pic3.zhimg.com/80/v2-b81b6495a8c6f54a8937e2dda5da574a_720w.png)

```javascript
// 同时包含数字和小写字母，可以用 (?=.*[0-9])(?=.*[a-z]) 
var regex = /(?=.*[0-9])(?=.*[a-z])^[0-9A-Za-z]{6,12}$/;
```

![image-20210806155447144](https://pic1.zhimg.com/80/v2-320a8defd0f6258ba18892102bb1d87a_720w.png)

```javascript
// 合并以上集中情况
var regex = /((?=.*[0-9])(?=.*[a-z])|(?=.*[0-9])(?=.*[A-Z])|(?=.*[a-z])(?=.*[A-Z]))^[0-9A-Za-z]{6,12}$/;
```

![image-20210806160255546](https://pic3.zhimg.com/80/v2-2485a9e666b4e27cc7b6b536fe2f5c97_720w.png)





## 第三章 正则表达式  括号的作用

| 模式              | 说明                                                        |
| ----------------- | ----------------------------------------------------------- |
| ``(ab)``          | 把ab看作一个整体，比如``(ab)+``表示``"ab"``至少连续出现一次 |
| ``(?:ab)``        | 非捕获型分组。与 (ab) 的区别是，它不捕获数据                |
| ``(good|nice)``   | 匹配``"good"``或``"nice"``                                  |
| ``(?:good|nice)`` | 非捕获型分支结构。与 (good\|nice) 的区别是，它不捕获数据。  |
| ``\num``          | 反向引用。比如 \2，表示引用的是第二个括号里的捕获的数据。   |



> 提取年月日，这里用到了 match  返回数组，["整体效果","分组1","分组2","分组3","下标"]

```javascript
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2021-08-06";
console.log(string.match(regex));
//["2021-08-06", "2021", "08", "06", index: 0, input: "2021-08-06"]
```

![image-20210806161634881](https://pic3.zhimg.com/80/v2-e0038ae55f883f95897c00b7ab088f2f_720w.png)



> 替换年月日，yyyy-mm-dd 替换成 mm/dd/yyyy

```javascript
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2021-08-06";
// replace 中的，第二个参数里用 $1、$2、$3 指代相应的分组,等价于 month + "/" + day + "/" + year
var result = string.replace(regex, "$2/$3/$1/");
console.log(result);//"06/08/2021"
```



#### 3.5 字符串 trim 方法

> ``trim ``方法用于去掉字符串开头和结尾的空白符

- **非惰性匹配**，效率高

```javascript
function trim(str){
	return str.replace(/^\s+|\s+$/g,'');
}
console.log(trim("  kuishou  ")); // "kuishou"
```

![image-20210806171343982](https://pica.zhimg.com/80/v2-1857233c81446dc4fde6b23c5c337eea_720w.png)

- **惰性匹配**

```javascript
function trim(str){
	return str.replace(/^\s*(.*?)\s*$/g,"$1");
}
console.log(trim("  kuishou  "));// "kuishou"
```

![image-20210806171458894](https://pica.zhimg.com/80/v2-a99fd90f7891421dc7513082ac1004ad_720w.png)

🌰例子：匹配 HTML成对标签

```html
<title>Hello kuishou</title>
<p>How are you!</p>
```

> 开标签，可以使用正则 ``<[^>]+>``
>
> 闭标签，可以使用正则``<\/[^>]+>``

```javascript
var regex = /<([^>]+)>[\d\D]*<\/\1>/;
var string1 = "<title>Hello kuishou</title>";
var string2 = "<p>How are you!</p>";
console.log(regex.test(string1));//true
console.log(regex.test(string2));//true
```

![image-20210806174258997](https://pica.zhimg.com/80/v2-336ebfbe7e4712f1ba090259f44a4a9e_720w.png)



## 第四章 正则表达式的 拆分

> 操作符优先级

| 操作符                                                       | 描述         | 优先级 |
| ------------------------------------------------------------ | ------------ | ------ |
| ``\``                                                        | 转义符       | 1      |
| ``(···)``、``(?:···)``、``(?=···)``、``(?!···)``、``[····]`` | 括号、方括号 | 2      |
| ``{m}``、``{m,n}``、``{m,}``、``?``、``*``、``+``            | 两次限定符   | 3      |
| ``^``、``$``、``\元字符``、``一般字符``                      | 位置和序列   | 4      |
| ``|``                                                        | 管道符       | 5      |

- 匹配身份证

![image-20210806223159285](https://pic1.zhimg.com/80/v2-18d7c5fd8fd008a5952c648b67c60133_720w.png)

- 匹配IP地址

```javascript
/^((0{0,2}\d|0?\d{2}|1\d{2}|2[0-4]\d|25[0-5])\.){3}(0{0,2}\d|0?\d{2}|1\d{2}|2[0-4]\d|25[0-5])$/
```

这个正则，看起来非常吓人。但是熟悉优先级后，会立马得出如下的结构：`` ((…)\.){3}(…) ``

其中，两个 ``(…) ``是一样的结构。表示匹配的是 3 位数字。因此整个结构是 

``3位数.3位数.3位数.3位数 ``

然后再来分析 (…)：`` (0{0,2}\d|0?\d{2}|1\d{2}|2[0-4]\d|25[0-5]) ``

它是一个多选结构，分成5个部分： 

- ``0{0,2}\d``，匹配一位数，包括 "0" 补齐的。比如，"9"、"09"、"009"； 
- ``0?\d{2}``，匹配两位数，包括 "0" 补齐的，也包括一位数； 
- ``1\d{2}``，匹配 "100" 到 "199"; 2[0-4]\d，匹配 "200" 到 "249"；
- ``25[0-5]``，匹配 "250" 到 "255"。

![image-20210806225356990](https://pica.zhimg.com/80/v2-6f6a0624c9a732b3b357d3a841c1ee77_720w.png)
