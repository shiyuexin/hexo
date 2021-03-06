---
title: html/javascript转译字符串
date: 2017-01-11
comments: true
categories: html
description: HTML中<，>，&等有特殊含义（<，>，用于链接签，&用于转义），不能直接使用。这些符号是不显示在我们最终看到的网页里的，那如果我们希望在网页中显示这些符号，该怎么办呢？这就要说到HTML转义字符串（Escape Sequence）了。
---

### 【前言】

>转义字符（Escape Sequence）也称字符实体(Character Entity)。在HTML中，定义转义字符串的原因有两个：第一个原因是像“<”和“>”这类符号已经用来表示HTML标签，因此就不能直接当作文本中的符号来使用。为了在HTML文档中使用这些符号，就需要定义它的转义字符串。当解释程序遇到这类字符串时就把它解释为真实的字符。在输入转义字符串时，要严格遵守字母大小写的规则。第二个原因是，有些字符在ASCII字符集中没有定义，因此需要使用转义字符串来表示。

### 【转义字符串的组成】

转义字符串（Escape Sequence），即字符实体（Character Entity）分成三部分：第一部分是一个&符号，英文叫ampersand；第二部分是实体（Entity）名字或者是#加上实体（Entity）编号；第三部分是一个分号。

比如，要显示小于号（<），就可以写 &lt; 或者 &#60; 。

用实体（Entity）名字的好处是比较好理解，一看lt，大概就猜出是less than的意思，但是其劣势在于并不是所有的浏览器都支持最新的Entity名字。而实体(Entity)编号，各种浏览器都能处理。

>提示：实体名称（Entity）是区分大小写的。
>备注：同一个符号，可以用“实体名称”和“实体编号”两种方式引用，“实体名称”的优势在于便于记忆，但不能保证所有的浏览器都能顺利识别它，而“实体编号”则没有这种担忧，但它实在不方便记忆。

### 【转译符】

#### HTML常用转义字符列表

|显示	|说明	        |实体名称	   |实体编号   |
|:-----:|:-------------:|:--------:|:-------:|
| 	    |半方大的空白	    |`&ensp;`  |`&#8194;`|
| 	    |全方大的空白	    |`&emsp;`  |`&#8195;`|
| 	    |不断行的空白格	|`&nbsp;`  |`&#160;` |
|<	    |小于	        |`&lt;`	   |`&#60;`  |
|>	    |大于	        |`&gt;`	   |`&#62;`  |
|&	    |&符号	        |`&amp;`   |`&#38;`  |
|"	    |双引号	        |`&quot;`  |`&#34;`  |
|©	    |版权	        |`&copy;`  |`&#169;` |
|®	    |已注册商标	    |`&reg;`   |`&#174;` |
|?	    |商标（美国）	    |`?`	   |`&#8482;`|
|×	    |乘号	        |`&times;` |`&#215;` |
|÷	    |除号	        |`&divide;`|`&#247;` |

#### ISO 8859-1 (Latin-1)字符集
HTML 4.01 支持 ISO 8859-1 (Latin-1) 字符集。

|显示	|名称	|编号	|显示	|名称	|编号	     |
|:-----:|:-----:|:-----:|:-----:|:-----:|:----------:|
| 	|`&nbsp;`	|`&#160;`	|¡	|`&iexcl;`	|`&#161;`|
|¥	|`&yen;`	|`&#165;`	|¦	|`&brvbar;`	|`&#166;`|	
|ª	|`&ordf;`	|`&#170;`	|«	|`&laquo;`	|`&#171;`|		
|¯	|`&macr;`	|`&#175;`	|°	|`&deg;`	|`&#176;`|		
|´	|`&acute;`	|`&#180;`	|µ	|`&micro;`	|`&#181;`|		
|¹	|`&sup1;`	|`&#185;`	|º	|`&ordm;`	|`&#186;`|		
|¾	|`&frac34;`	|`&#190;`	|¿	|`&iquest;`	|`&#191;`|		
|Ã	|`&Atilde;`	|`&#195;`	|Ä	|`&Auml;`	|`&#196;`|		
|È	|`&Egrave;`	|`&#200;`	|?	|`&Eacute;`	|`&#201;`|		
|?	|`&Iacute;`	|`&#205;`	|Î	|`&Icirc;`	|`&#206;`|		
|Ò	|`&Ograve;`	|`&#210;`	|?	|`&Oacute;`	|`&#211;`|		
|×	|`&times;`	|`&#215;`	|Ø	|`&Oslash;`	|`&#216;`|		
|Ü	|`&Uuml;`	|`&#220;`	|?	|`&Yacute;`	|`&#221;`|		
|á	|`&aacute;`	|`&#225;`	|â	|`&acirc;`	|`&#226;`|		
|æ	|`&aelig;`	|`&#230;`	|ç	|`&ccedil;`	|`&#231;`|		
|ë	|`&euml;`	|`&#235;`	|ì	|`&igrave;`	|`&#236;`|		
|ð	|`&eth;`	|`&#240;`	|ñ	|`&ntilde;`	|`&#241;`|		
|õ	|`&otilde;`	|`&#245;`	|ö	|`&ouml;`	|`&#246;`|		
|ú	|`&uacute;`	|`&#250;`	|û	|`&ucirc;`	|`&#251;`|		
|¢	|`&cent;`	|`&#162;`	|£	|`&pound;`	|`&#163;`|	
|§	|`&sect;`	|`&#167;`	|¨	|`&uml;`	|`&#168;`|	
|	|`&shy;`	|`&#173;`	|®	|`&reg;`	|`&#174;`|
|ÿ	|`&yuml;`	|`&#255;`   |¬	|`&not;`	|`&#172;`|
|²	|`&sup2;`	|`&#178;`	|³	|`&sup3;`	|`&#179;`|
|·	|`&middot;`	|`&#183;`	|¸	|`&cedil;`	|`&#184;`|
|¼	|`&frac14;`	|`&#188;`	|½	|`&frac12;`	|`&#189;`|
|?	|`&Aacute;`	|`&#193;`	|Â	|`&Acirc;`	|`&#194;`|
|Æ	|`&AElig;`	|`&#198;`	|Ç	|`&Ccedil;`	|`&#199;`|
|Ë	|`&Euml;`	|`&#203;`	|Ì	|`&Igrave;`	|`&#204;`|
|Ð	|`&ETH;`	|`&#208;`	|Ñ	|`&Ntilde;`	|`&#209;`|
|Õ	|`&Otilde;`	|`&#213;`	|Ö	|`&Ouml;`	|`&#214;`|
|?	|`&Uacute;`	|`&#218;`	|Û	|`&Ucirc;`	|`&#219;`|
|ß	|`&szlig;`	|`&#223;`	|à	|`&agrave;`	|`&#224;`|
|ä	|`&auml;`	|`&#228;`	|å	|`&aring;`	|`&#229;`|
|é	|`&eacute;`	|`&#233;`	|ê	|`&ecirc;`	|`&#234;`|
|î	|`&icirc;`	|`&#238;`	|ï	|`&iuml;`	|`&#239;`|
|ó	|`&oacute;`	|`&#243;`	|ô	|`&ocirc;`	|`&#244;`|
|ø	|`&oslash;`	|`&#248;`	|ù	|`&ugrave;`	|`&#249;`|
|?	|`&yacute;`	|`&#253;`	|þ	|`&thorn;`	|`&#254;`|
|±	|`&plusmn;`	|`&#177;`   |¶	|`&para;`	|`&#182;`|
|»	|`&raquo;`	|`&#187;`   |À	|`&Agrave;`	|`&#192;`|
|Å	|`&Aring;`	|`&#197;`   |Ê	|`&Ecirc;`	|`&#202;`|
|Ï	|`&Iuml;`	|`&#207;`   |Ô	|`&Ocirc;`	|`&#212;`|
|Ù	|`&Ugrave;`	|`&#217;`   |Þ	|`&THORN;`	|`&#222;`|
|ã	|`&atilde;`	|`&#227;`   |è	|`&egrave;`	|`&#232;`|
|í	|`&iacute;`	|`&#237;`   |ò	|`&ograve;`	|`&#242;`|
|÷	|`&divide;`	|`&#247;`   |ü	|`&uuml;`	|`&#252;`|
|¤	|`&curren;`	|`&#164;`   |©	|`&copy;`	|`&#169;`|

#### JavaScript转义符

|转义序列	|字符|
|-------|---|
|\b	|退格|
|\f	|走纸换页|
|\n	|换行|
|\r	|回车|
|\t	|横向跳格 (Ctrl-I)|
|\'	|单引号|
|\"	|双引号|
|`\\`	|反斜杠|

#### JavaScript编码转换

[escape()](http://www.w3school.com.cn/jsref/jsref_escape.asp),
[encodeURI()](http://www.w3school.com.cn/jsref/jsref_encodeuri.asp),
[encodeURIComponent()](http://www.w3school.com.cn/jsref/jsref_encodeURIComponent.asp)
>通过对三个函数的分析，我们可以知道：escape()除了 ASCII 字母、数字和特定的符号外，对传进来的字符串全部进行转义编码，因此如果想对URL编码，最好不要使用此方法。而encodeURI() 用于编码整个URI,因为URI中的合法字符都不会被编码转换。encodeURIComponent方法在编码单个URIComponent（指请求参 数）应当是最常用的，它可以讲参数中的中文、特殊字符进行转义，而不会影响整个URL。
