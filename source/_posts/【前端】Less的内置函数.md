---
title: Less的内置函数
description: '-'
tags:
  - WEB开发
  - Web前端
  - Css预处理
  - Less
abbrlink: fef42d6e
date: 2021-01-20 00:15:24
---



# [Less的内置函数](https://www.cnblogs.com/waibo/p/7918454.html)

**转自：https://www.cnblogs.com/waibo/p/7918454.html**

## 杂项函数

### [color](http://www.waibo.wang/bible/less/html/3/3.html)

> 解析颜色，将代表颜色的字符串转换为颜色值.

参数：

- `string`: 代表颜色值的字符串。

返回值： `color`

案例： `color("#aaa");`

输出： `#aaa`

### convert

> 将数字从一种单位转换到另一种单位。

第一个参数为带单位的数值，第二个参数为单位。如果两个参数的单位是兼容的，则数字的单位被转换。如果两个参数的单位不兼容，则原样返回第一个参数。

兼容的单位是:

- 长度： `m`, `cm`, `mm`, `in`, `pt` and `pc`,
- 时间： `s` and `ms`,
- 角度： `rad`, `deg`, `grad` and `turn`.

参数：

- `number`: 带单位浮点数。
- `identifier`, `string` or `escaped value`: units

返回值： `number`

案例：

```less
convert(9s, "ms")
convert(14cm, mm)
convert(8, mm) // incompatible unit types
```

输出：

```
9000ms
140mm
8
```

### data-uri

> 将资源内联进样式表，如果开启了 ieCompat 选项并且资源太大，或者此函数的运行环境为浏览器，则会回退到直接使用 `url()` 。如果没有指定 MIME，则 node 将使用 mime 包来决定正确的 mime 类型。

参数：

- `mimetype`: (可选) MIME 字符串。
- `url`: 需要内嵌的文件的 URL 。

案例： `data-uri('../data/image.jpg');`

输出： `url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');`

浏览器端输出： `url('../data/image.jpg');`

案例： `data-uri('image/jpeg;base64', '../data/image.jpg');`

输出： `url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==');`

### default

> Available only inside guard conditions and returns `true` only if no other mixin matches, `false` otherwise.

案例：

```less
.mixin(1)                   {x: 11}
.mixin(2)                   {y: 22}
.mixin(@x) when (default()) {z: @x}

div {
  .mixin(3);
}

div.special {
  .mixin(1);
}
```

输出：

```css
div {
  z: 3;
}
div.special {
  x: 11;
}
```

It is possible to use the value returned by `default` with guard operators. For example `.mixin() when not(default()) {}` will match only if there's at least one more mixin definition that matches`.mixin()` call:

```less
.mixin(@value) when (ispixel(@value)) {width: @value}
.mixin(@value) when not(default())    {padding: (@value / 5)}

div-1 {
  .mixin(100px);
}

div-2 {
  /* ... */
  .mixin(100%);
}
```

输出：

```css
div-1 {
  width: 100px;
  padding: 20px;
}
div-2 {
  /* ... */
}
```

It is allowed to make multiple `default()` calls in the same guard condition or in a different conditions of a mixins with the same name:

```less
div {
  .m(@x) when (default()), not(default())    {always: @x}
  .m(@x) when (default()) and not(default()) {never:  @x}

  .m(1); // OK
}
```

However Less will throw a error if it detects a *potential* conflict between multiple mixin definitions using `default()`:

```less
div {
  .m(@x) when (default())    {}
  .m(@x) when not(default()) {}

  .m(1); // Error
}
```

In above example it is impossible to determine what value each `default()` call should return since they recursively depend on each other.

Advanced multiple `default()` usage:

```less
.x {
  .m(red)                                    {case-1: darkred}
  .m(blue)                                   {case-2: darkblue}
  .m(@x) when (iscolor(@x)) and (default())  {default-color: @x}
  .m('foo')                                  {case-1: I am 'foo'}
  .m('bar')                                  {case-2: I am 'bar'}
  .m(@x) when (isstring(@x)) and (default()) {default-string: and I am the default}

  &-blue  {.m(blue)}
  &-green {.m(green)}
  &-foo   {.m('foo')}
  &-baz   {.m('baz')}
}
```

输出：

```css
.x-blue {
  case-2: #00008b;
}
.x-green {
  default-color: #008000;
}
.x-foo {
  case-1: I am 'foo';
}
.x-baz {
  default-string: and I am the default;
}
```

The `default` function is available as a Less built-in function *only inside guard expressions*. If used outside of a mixin guard condition it is interpreted as a regular CSS value:

案例：

```less
div {
  foo: default();
  bar: default(42);
}
```

输出：

```css
div {
  foo: default();
  bar: default(42);
}
```

### unit

> 删除或更换单位。

参数：

- `dimension`: 带单位或不带单位的数字。
- `unit`: (可选) 目标单位，如果省略此参数，则删除单位。

See [convert](http://www.waibo.wang/bible/less/html/3/3.html#misc-functions-convert) for changing the unit without conversion.

案例： `unit(5, px)`

输出： `5px`

案例： `unit(5em)`

输出： `5`

## 字符串函数

### escape

> 对字符串中的特殊字符做 [URL-encoding](http://en.wikipedia.org/wiki/Percent-encoding) 编码。

- 这些字符不会被编码：`,`, `/`, `?`, `@`, `&`, `+`, `'`, `~`, `!` and `$`。
- 被编码的字符是：`\<space\>`, `#`, `^`, `(`, `)`, `{`, `}`, `|`, `:`, `>`, `<`, `;`, `]`, `[` and `=`。

参数：`string`: 需要转义的字符串。

返回值：转义后不带引号的 `string` 字符串。

案例：

```less
escape('a=1')
```

输出：

```css
a%3D1
```

注意：如果参数不是字符串的话，函数行为是不可预知的。目前传入颜色值的话会返回 `undefined` ，其它的值会原样返回。写代码时不应该依赖这个特性，而且这个特性在未来有可能改变。

### e

> 用于对 CSS 的转义，已经由 `~"value"` 语法代替。

它接受一个字符串作为参数，并原样返回内容，不含引号。它可用于输出一些不合法的 CSS 语法，或者是使用 LESS 不能识别的属性。

参数：`string` - 需要转义的字符串。

返回值： `string` - 转义后的字符串，不含引号

案例：

```less
filter: e("ms:alwaysHasItsOwnSyntax.For.Stuff()");
```

输出：

```css
filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
```

注意：也接受经 `~""` 转义的值或者是数字作为参数。任何其它的值将产生错误。

### % 格式化

> 此函数 `%(string, arguments ...)` 用于格式化字符串。

第一个参数是一个包含占位符的字符串。占位符以百分号 `%` 开头，后面跟着字母 `s`、`S`、`d`、`D`、`a` 或 `A`。后续的参数用于替换这些占位符。如果你需要输出百分号，可以多用一个百分号来转义 `%%`。

使用大写的占位符可以将特殊字符按照 UTF-8 编码进行转义。 此函数将会对所有的特殊字符进行转义，除了 `()'~!` 。空格会被转义为 `%20` 。小写的占位符将原样输出特殊字符，不进行转义。

占位符说明：

- `d`, `D`, `a`, `A` - 以被任意类型的参数替换 (颜色、数字、转义后的字符串、表达式等)。如果将它们和字符串一起使用，则整个字符串都会被使用，包括引号。然而，引号将会按原样放在字符串中，不会用 "/" 或类似的方式转义。
- `s`, `S` - 可以被除了颜色的之外的任何类型参数替换。如果你将它们和字符串一起使用，则只有字符串的值会被使用，引号会被忽略。

参数：

- `string`: 有占位符的格式化字符串。
- `anything`* : 用于替换占位符的值。

返回值： 格式化后的 `字符串（string）`.

案例：

```less
format-a-d: %("repetitions: %a file: %d", 1 + 2, "directory/file.less");
format-a-d-upper: %('repetitions: %A file: %D', 1 + 2, "directory/file.less");
format-s: %("repetitions: %s file: %s", 1 + 2, "directory/file.less");
format-s-upper: %('repetitions: %S file: %S', 1 + 2, "directory/file.less");
```

输出：

```css
format-a-d: "repetitions: 3 file: "directory/file.less"";
format-a-d-upper: "repetitions: 3 file: %22directory%2Ffile.less%22";
format-s: "repetitions: 3 file: directory/file.less";
format-s-upper: "repetitions: 3 file: directory%2Ffile.less";
```

### replace

> 用一个字符串替换一段文本。

参数：

- `string`: 用于搜索、替换操作的字符串。
- `pattern`: 用于搜索匹配的字符串或正则表达式。
- `replacement`: 用于替换匹配项的字符串。
- `flags`: (可选) 正则表达式参数。

返回值： 被替换之后的字符串。

案例：

```less
replace("Hello, Mars?", "Mars\?", "Earth!");
replace("One + one = 4", "one", "2", "gi");
replace('This is a string.', "(string)\.$", "new $1.");
replace(~"bar-1", '1', '2');
```

输出：

```css
"Hello, Earth!";
"2 + 2 = 4";
'This is a new string.';
bar-2;
```

## 列表函数

### length

> 返回列表中元素的个数。

参数：

- `list` - 由逗号或空格分隔的元素列表。

返回值：一个代表元素个数的数字。

案例： `length(1px solid #0080ff);`

输出：`3`

案例：

```less
@list: "banana", "tomato", "potato", "peach";
n: length(@list);
```

输出：

```
n: 4;
```

### extract

> 返回列表中指定位置的元素。

参数：

- `list` - 逗号或空格分隔的元素列表。
- `index` - 指定列表中元素位置的数字。

返回值：列表中指定位置的元素。

案例： `extract(8px dotted red, 2);`

输出： `dotted`

案例：

```less
@list: apple, pear, coconut, orange;
value: extract(@list, 3);
```

输出：

```
value: coconut;
```

## 数学函数

### ceil

> 向上取整。

参数： `number` - 浮点数。

返回值： `整数（integer）`

案例： `ceil(2.4)`

输出： `3`

### floor

> 向下取整。

参数： `number` - 浮点数

返回值： `整数（integer）`

案例： `floor(2.6)`

输出： `2`

### percentage

> 将浮点数转换为百分比字符串。

参数： `number` - 浮点数。

返回值： `字符串（string）`

案例： `percentage(0.5)`

输出： `50%`

### round

> 四舍五入取整。

参数：

- `number`: 浮点数
- `decimalPlaces`: 可选：四舍五入取整的小数点位置。默认值为0。

返回值： `数字（number）`

案例： `round(1.67)`

输出： `2`

案例： `round(1.67, 1)`

输出： `1.7`

### sqrt

> 计算一个数的平方根，并原样保持单位。

参数： `number` - 浮点数

返回值： 数字（number）

案例：

```less
sqrt(25cm)
```

输出：

```css
5cm
```

案例：

```less
sqrt(18.6%)
```

输出：

```js
4.312771730569565%;
```

### abs

> 计算数字的绝对值，并原样保持单位。

参数： `number` - 浮点数。

返回值： 数字（number）

Example #1: `abs(25cm)`

输出： `25cm`

Example #2: `abs(-18.6%)`

输出： `18.6%;`

### sin

> 正弦函数。

处理时会将没有单位的数字认为是弧度值。

参数： `number` - 浮点数。

返回值： 数字（number）

案例：

```less
sin(1); // sine of 1 radian
sin(1deg); // sine of 1 degree
sin(1grad); // sine of 1 gradian
```

输出：

```
0.8414709848078965; // sine of 1 radian
0.01745240643728351; // sine of 1 degree
0.015707317311820675; // sine of 1 gradian
```

### asin

> 反正弦函数。返回以弧度为单位的角度，区间在 -PI/2 到 PI/2之间。

返回以弧度为单位的角度，区间在 `-π/2` 和 `π/2` 之间。

参数： `number` - 取值范围为 `[-1, 1]` 之间的浮点数。

返回值： 数字（number）

案例：

```less
asin(-0.8414709848078965)
asin(0)
asin(2)
```

输出：

```
-1rad
0rad
NaNrad
```

### cos

> 余弦函数。

处理时会将没有单位的数字认为是弧度值。

参数： `number` - 浮点数。

返回值： 数字（number）

案例：

```less
cos(1) // cosine of 1 radian
cos(1deg) // cosine of 1 degree
cos(1grad) // cosine of 1 gradian
```

输出：

```
0.5403023058681398 // cosine of 1 radian
0.9998476951563913 // cosine of 1 degree
0.9998766324816606 // cosine of 1 gradian
```

### acos

> 反余弦函数。，区间在 0 到 PI之间。

返回以弧度为单位的角度，区间在 0 到 π 之间。

参数： `number` - 取值范围为 [-1, 1] 之间的浮点数。

返回值： 数字（number）

案例：

```less
acos(0.5403023058681398)
acos(1)
acos(2)
```

输出：

```
1rad
0rad
NaNrad
```

### tan

> 正切函数。

处理时会将没有单位的数字认为是弧度值。

参数： `number` - 浮点数。

返回值： 数字（number）

案例：

```less
tan(1) // tangent of 1 radian
tan(1deg) // tangent of 1 degree
tan(1grad) // tangent of 1 gradian
```

输出：

```
1.5574077246549023   // tangent of 1 radian
0.017455064928217585 // tangent of 1 degree
0.015709255323664916 // tangent of 1 gradian
```

### atan

> 反正切函数。

返回以弧度为单位的角度，区间在 `-π/2` 到 `π/2` 之间。

参数： `number` - 浮点数。

返回值： 数字（number）

案例：

```less
atan(-1.5574077246549023)
atan(0)
round(atan(22), 6) // arctangent of 22 rounded to 6 decimal places
```

输出：

```
-1rad
0rad
1.525373rad;
```

### pi

> 返回圆周率 π (pi);

参数： `none`

返回值： `number`

案例：

```less
pi()
```

输出：

```
3.141592653589793
```

### pow

> 设第一个参数为A，第二个参数为B，返回A的B次方。

返回值与第一个参数有相同的单位，第二个参数的单位被忽略。

参数：

- `number`: base -浮点数。
- `number`: exponent - 浮点数。

返回值： 数字（number）

案例：

```less
pow(0cm, 0px)
pow(25, -2)
pow(25, 0.5)
pow(-25, 0.5)
pow(-25%, -0.5)
```

输出：

```
1cm
0.0016
5
NaN
NaN%
```

### mod

> 返回第一个参数对第二参数取余的结果。

返回值与第一个参数单位相同，第二个参数单位被忽略。这个函数也可以处理负数和浮点数。

参数：

- `number`: 浮点数。
- `number`: 浮点数。

返回值： 数字（number）

案例：

```less
mod(0cm, 0px)
mod(11cm, 6px);
mod(-26%, -5);
```

输出：

```
NaNcm;
5cm
-1%;
```

### min

> 返回一系列值中最小的那个。

参数： `value1, ..., valueN` - 一个或多个参与比较的值。

返回值： 最小值。

案例： `min(5, 10);`

输出： `5`

案例： `min(3px, 42px, 1px, 16px);`

输出： `1px`

### max

> 返回一系列值中最大的那个。

参数： `value1, ..., valueN` - 一个或多个参与比较的值。

返回值： 最大值。

案例： `max(5, 10);`

输出： `10`

案例： `max(3%, 42%, 1%, 16%);`

输出： `42%`

## 类型函数

### isnumber

> 如果待验证的值为数字则返回 `true` ，否则返回 `false` 。

参数：`value` - 待验证的值或变量。

返回值：如果待验证的值为数字则返回 `true` ，否则返回 `false` 。

案例：

```less
isnumber(#ff0);     // false
isnumber(blue);     // false
isnumber("string"); // false
isnumber(1234);     // true
isnumber(56px);     // true
isnumber(7.8%);     // true
isnumber(keyword);  // false
isnumber(url(...)); // false
```

### isstring

> 如果待验证的值是字符串则返回 `true` ，否则返回 `false` 。

参数：`value` - 待验证的值或变量。

返回值：如果是字符串则返回 `true` ，否则返回 `false` 。

案例：

```less
isstring(#ff0);     // false
isstring(blue);     // false
isstring("string"); // true
isstring(1234);     // false
isstring(56px);     // false
isstring(7.8%);     // false
isstring(keyword);  // false
isstring(url(...)); // false
```

### iscolor

> 如果待验证的值为颜色则返回 `true` ，否则返回 `false` 。

参数：`value` - 待验证的值或变量。

返回值：如果待验证的值为颜色则返回 `true` ，否则返回 `false` 。

案例：

```less
iscolor(#ff0);     // true
iscolor(blue);     // true
iscolor("string"); // false
iscolor(1234);     // false
iscolor(56px);     // false
iscolor(7.8%);     // false
iscolor(keyword);  // false
iscolor(url(...)); // false
```

### iskeyword

> 如果待验证的值为关键字则返回 `true` ，否则返回 `false` 。

参数： `value` - 待验证的值或变量。

返回值：如果待验证的值为关键字则返回 `true` ，否则返回 `false` 。

案例：

```less
iskeyword(#ff0);     // false
iskeyword(blue);     // false
iskeyword("string"); // false
iskeyword(1234);     // false
iskeyword(56px);     // false
iskeyword(7.8%);     // false
iskeyword(keyword);  // true
iskeyword(url(...)); // false
```

### isurl

> 如果待验证的值为 url 则返回 `true` ，否则返回 `false` 。

参数： `value` - 待验证的值或变量。

返回值：如果待验证的值为 url 则返回 `true` ，否则返回 `false` 。

案例：

```less
isurl(#ff0);     // false
isurl(blue);     // false
isurl("string"); // false
isurl(1234);     // false
isurl(56px);     // false
isurl(7.8%);     // false
isurl(keyword);  // false
isurl(url(...)); // true
```

### ispixel

> 如果待验证的值为像素数则返回 `true` ，否则返回 `false` 。

参数： `value` - 待验证的值或变量。

返回值：如果待验证的值为像素数则返回 `true` ，否则返回 `false` 。

案例：

```less
ispixel(#ff0);     // false
ispixel(blue);     // false
ispixel("string"); // false
ispixel(1234);     // false
ispixel(56px);     // true
ispixel(7.8%);     // false
ispixel(keyword);  // false
ispixel(url(...)); // false
```

### isem

> 如果待验证的值的单位是 em 则返回 `true` ，否则返回 `false` 。

参数： `value` - 待验证的值或变量。

返回值：如果待验证的值的单位是 em 则返回 `true` ，否则返回 `false` 。

案例：

```less
isem(#ff0);     // false
isem(blue);     // false
isem("string"); // false
isem(1234);     // false
isem(56px);     // false
isem(7.8em);    // true
isem(keyword);  // false
isem(url(...)); // false
```

### ispercentage

> 如果待验证的值为百分比则返回 `true` ，否则返回 `false` 。

参数： `value` - 待验证的值或变量。

返回值：如果待验证的值为百分比则返回 `true` ，否则返回 `false` 。

案例：

```less
ispercentage(#ff0);     // false
ispercentage(blue);     // false
ispercentage("string"); // false
ispercentage(1234);     // false
ispercentage(56px);     // false
ispercentage(7.8%);     // true
ispercentage(keyword);  // false
ispercentage(url(...)); // false
```

### isunit

> 如果待验证的值为指定单位的数字则返回 `true` ，否则返回 `false` 。

参数：

- `value` - 待验证的值或变量。
- `unit` - 单位标示符 (可加引号) 。

返回值：如果待验证的值为指定单位的数字则返回 `true` ，否则返回 `false` 。

案例：

```less
isunit(11px, px);  // true
isunit(2.2%, px);  // false
isunit(33px, rem); // false
isunit(4rem, rem); // true
isunit(56px, "%"); // false
isunit(7.8%, '%'); // true
isunit(1234, em);  // false
isunit(#ff0, pt);  // false
isunit("mm", mm);  // false
```

## 颜色定义函数

### rgb

> 通过十进制红色、绿色、蓝色三种值 (RGB) 创建不透明的颜色对象。

在 HTML/CSS 中也会用文本颜色值 (literal color values) 定义颜色值，例如 `#ff0000`。

参数：

- `red`: 0-255 的整数或 0-100% 的百分比数。
- `green`: 0-255 的整数或 0-100% 的百分比数。
- `blue`: 0-255 的整数或 0-100% 的百分比数。

返回值： `color`

案例： `rgb(90, 129, 32)`

输出： `#5a8120`

### rgba

> 通过十进制红色、绿色、蓝色，以及 alpha 四种值 (RGBA) 创建带alpha透明的颜色对象。

参数：

- `red`: 0-255 的整数或 0-100% 的百分比数。
- `green`: 0-255 的整数或 0-100% 的百分比数。
- `blue`: 0-255 的整数或 0-100% 的百分比数。
- `alpha`: 0-1 的整数或 0-100% 的百分比数。

返回值： `color`

案例： `rgba(90, 129, 32, 0.5)`

输出： `rgba(90, 129, 32, 0.5)`

### argb

> 创建格式为 `#AARRGGBB` 的十六进制颜色值 (**注意不是** `#RRGGBBAA`！)。

这种格式被用于 IE 、.NET 和 Android 的开发中。

参数： `color`, 颜色对象。

返回值： `string`

案例： `argb(rgba(90, 23, 148, 0.5));`

输出： `#805a1794`

### hsl

> 通过色相 (hue)，饱和度 (saturation)，亮度 (lightness) 三种值 (HSL) 创建不透明的颜色对象。

参数：

- `hue`: 0-360 的整数，用于表示度数。
- `saturation`: 0-100% 的百分比数或 0-1 的整数。
- `lightness`: 0-100% 的百分比数或 0-1 的整数。

返回值： `color`

案例： `hsl(90, 100%, 50%)`

输出： `#80ff00`

当你想基于一种颜色的通道来创建另一种颜色时很方便，例如： `@new: hsl(hue(@old), 45%, 90%);` `@new` 将拥有 `@old` 的 *hue*，以及它自身的饱和度与亮度。

### hsla

> 通过色相 (hue)，饱和度 (saturation)，亮度 (lightness)，以及 alpha 四种值 (HSLA) 创建透明的颜色对象。

参数：

- `hue`: 0-360 的整数，用于表示度数。
- `saturation`: 0-100% 的百分比数或 0-1 的整数。
- `lightness`: 0-100% 的百分比数或 0-1 的整数。
- `alpha`: 0-100% 的百分比数或 0-1 的整数。

返回值： `color`

案例： `hsl(90, 100%, 50%, 0.5)`

输出： `rgba(128, 255, 0, 0.5)`

### hsv

> 通过色相 (hue)、饱和度 (saturation)、色调 (value) 三种值 (HSV) 创建不透明的颜色对象。

注意，与 `hsl` 不同，这是另一种在 Photoshop 中可用的色彩空间。

参数：

- `hue`: 0-360 的整数，用于表示度数。
- `saturation`: 0-100% 的百分比数或 0-1 的整数。
- `value`: 0-100% 的百分比数或 0-1 的整数。

返回值： `color`

案例： `hsv(90, 100%, 50%)`

输出： `#408000`

### hsva

> 通过色相 (hue)，饱和度 (saturation)，色调 (value)，以及 alpha 四种值 (HSVA) 创建透明的颜色对象。

注意，与 `hsla` 不同，这是另一种在 Photoshop 中可用的色彩空间。

参数：

- `hue`: 0-360 的整数，用于表示度数。
- `saturation`: 0-100% 的百分比数或 0-1 的整数。
- `value`: 0-100% 的百分比数或 0-1 的整数。
- `alpha`: 0-100% 的百分比数或 0-1 的整数。

返回值： `color`

案例： `hsva(90, 100%, 50%, 0.5)`

输出： `rgba(64, 128, 0, 0.5)`

## 颜色通道函数

### hue

> 从颜色对象的 HSL 颜色空间中提取色色调值。

参数： `color` - 颜色对象。

返回值： `整数（integer）` `0-360`

案例： `hue(hsl(90, 100%, 50%))`

输出： `90`

### saturation

> 从颜色对象的 HSL 色彩空间中提取饱和度值。

参数： `color` - 颜色对象。

返回值： `百分比（percentage）` `0-100`

案例： `saturation(hsl(90, 100%, 50%))`

输出： `100%`

### lightness

> 从颜色对象的 HSL 色彩空间中提取亮度值。

参数： `color` - 颜色对象。

返回值： `百分比（percentage）` `0-100`

案例： `lightness(hsl(90, 100%, 50%))`

输出： `50%`

### hsvhue

> 在颜色对象的 HSV 色彩空间中提取色相值。

参数： `color` - 颜色对象。

返回值： `整数（integer）` `0-360`

案例： `hsvhue(hsv(90, 100%, 50%))`

输出： `90`

### hsvsaturation

> 在颜色对象的 HSV 色彩空间提取饱和度值。

参数： `color` - 颜色对象。

返回值： `百分比（percentage）` 0-100

案例： `hsvsaturation(hsv(90, 100%, 50%))`

输出： `100%`

### hsvvalue

> Extracts the value channel of a color object in the HSV color space.

参数： `color` - 颜色对象。

返回值： `百分比（percentage）` 0-100

案例： `hsvvalue(hsv(90, 100%, 50%))`

输出： `50%`

### red

> 从颜色对象中提取红色通道值。

参数： `color` - 颜色对象。

返回值： `整数（integer）` `0-255`

案例： `red(rgb(10, 20, 30))`

输出： `10`

### green

> 从颜色对象中提取绿色通道值。

参数： `color` - 颜色对象。

返回值： `整数（integer）` 0-255

案例： `green(rgb(10, 20, 30))`

输出： `20`

### blue

> 从颜色对象中提取蓝色通道值。

参数： `color` - 颜色对象。

返回值： `整数（integer）` 0-255

案例： `blue(rgb(10, 20, 30))`

输出： `30`

### alpha

> 从颜色对象中提取 alpha 通道值。

参数： `color` - 颜色对象。

返回值： `浮点数（float）` `0-1`

案例： `alpha(rgba(10, 20, 30, 0.5))`

输出： `0.5`

### luma

> 计算颜色对象的 [luma](http://en.wikipedia.org/wiki/Luma_(video)) (perceptual brightness) 值（亮度的百分比表示法）。

Uses **SMPTE C / Rec. 709** coefficients, as recommended in [WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef). This calculation is also used in the contrast function.

参数： `color` - a颜色对象。

返回值： `百分比（percentage）` 0-100%

案例： `luma(rgb(100, 200, 30))`

输出： `65%`

## 颜色操作函数

Color operations generally take parameters in the same units as the values they are changing, and percentages are handled as absolutes, so increasing a 10% value by 10% results in 20%, not 11%, and values are clamped to their allowed ranges; they do not wrap around. Where return values are shown, we've used formats that make it clear what each function has done, in addition to the hex versions that you will usually be be working with.

### saturate

> Increase the saturation of a color in the HSL color space by an absolute amount.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `saturate(hsl(90, 80%, 50%), 20%)`

输出： `#80ff00 // hsl(90, 100%, 50%)`

![80e619](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZbQ6CMBAF5Rh6Wk6rx1AhYlbSLzUpo4z/sKn7fNO33YRhPB+vBz8YBwaBYFjMQgTC4iEQGA+BCITmAEyPd4hAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEzOZgkZT5cXK+6vAZ7PpbWSf7V9y3qsNf1ebV9PZpsAWRsTn0trLTAms1PGR9NT8JfvctB6QflLIDnzSqB2DaTUJloSkjrt67Yz1whtMNY0Iasj+03LyrW3d0DW7pAUTFvW43TnjI4Gpe6NlvaUMtk7JEw5LcbWJqXSXdCyd5dAaqNmbQzNrX86Stfq9WpXU51Npqyef/DXagkERkwgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDk3ACbeSboVqFeNAAAAABJRU5ErkJggg==) ➜ ![80ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFklEQVRoQ+2Z0Q6DIBAE5c/9c9saaawRlGC4aTK8mRNu3WHFxDQv0zI5MA4kgWBYrEIEwuIhEBgPgQiE5gBMj2eIQGAOwOSYEIHAHIDJMSECgTkAk2NCBAJzACbHhAgE5gBMTlhC5vTrxPs3wHfUavmm1vmt90dxCgGSzckQ9te12hHGHmKt1ttvJByBbEk92xwjQeReIUA+zUuvkKuEHOeta22vu1LNhFxsrV6DjvP37c5qvf1GJiUkIb0GCeThLSKQsqEhCamdIa21O2dI65pnX28P78nicmFARj3gv/URCIyYQAQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyXkBlUDx2X8D7WsAAAAASUVORK5CYII=)

### desaturate

> Decrease the saturation of a color in the HSL color space by an absolute amount.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `desaturate(hsl(90, 80%, 50%), 20%)`

输出： `#80cc33 // hsl(90, 60%, 50%)`

![80e619](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZbQ6CMBAF5Rh6Wk6rx1AhYlbSLzUpo4z/sKn7fNO33YRhPB+vBz8YBwaBYFjMQgTC4iEQGA+BCITmAEyPd4hAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEzOZgkZT5cXK+6vAZ7PpbWSf7V9y3qsNf1ebV9PZpsAWRsTn0trLTAms1PGR9NT8JfvctB6QflLIDnzSqB2DaTUJloSkjrt67Yz1whtMNY0Iasj+03LyrW3d0DW7pAUTFvW43TnjI4Gpe6NlvaUMtk7JEw5LcbWJqXSXdCyd5dAaqNmbQzNrX86Stfq9WpXU51Npqyef/DXagkERkwgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDk3ACbeSboVqFeNAAAAABJRU5ErkJggg==) ➜ ![80cc33](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKUlEQVRoQ+2ZbQ6CMBAF5Ux6QA6oZ/IrwdQmts2C7qjjP7JWnjO8QsI0H/fnnR8MgUkhGBf3IAph+VAIzIdCFEIjAMvjPUQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDhpDZkPpycU19cAj+PWLMrv0+eL5kwRssBZJJTHrVn0T376fNGcae9DsgHV5y8BtmZrQI+uTWnILdyrLWSkIdHtp1xXbpF1nno2CnOL76UIWdOQ0e0t2oK/bMhWQuorchRmVNYWDej9xlc3ZFTImgugB3DreYqQ1j2kN+vNe/emBeC7H7OjotKERAP/+jqFwAwrRCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4F+zuFOivbAR3AAAAAElFTkSuQmCC)

### lighten

> Increase the lightness of a color in the HSL color space by an absolute amount.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `lighten(hsl(90, 80%, 50%), 20%)`

输出： `#b3f075 // hsl(90, 80%, 70%)`

![80e619](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZbQ6CMBAF5Rh6Wk6rx1AhYlbSLzUpo4z/sKn7fNO33YRhPB+vBz8YBwaBYFjMQgTC4iEQGA+BCITmAEyPd4hAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEzOZgkZT5cXK+6vAZ7PpbWSf7V9y3qsNf1ebV9PZpsAWRsTn0trLTAms1PGR9NT8JfvctB6QflLIDnzSqB2DaTUJloSkjrt67Yz1whtMNY0Iasj+03LyrW3d0DW7pAUTFvW43TnjI4Gpe6NlvaUMtk7JEw5LcbWJqXSXdCyd5dAaqNmbQzNrX86Stfq9WpXU51Npqyef/DXagkERkwgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDk3ACbeSboVqFeNAAAAABJRU5ErkJggg==) ➜ ![b3f075](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABTElEQVRoQ+2Z4Q2CMBQGdQMXcxyncBwXcwMVE8xLfa0lBnrG85eBAh93fC3R/eV6uu38YAjsFYJx8QyiEJYPhcB8KEQhNAKwPK4hCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMWxIQqBEYDFGdKQ4+H8wvD4+f8NSdw/7ewdUzuu3D5fcDpva98IV0OETDc6gyhhl9uzcT3b4pja91aOETKG/h9SE1KC6IGfgf1G+CgZCCFx+oggatNaNsXEqWdu3FKR8drZFLmVJMSU1WrLUrBLhfQ0cisZiIZkT3fWlPjU9kjqGZOB7p1K15KEa8gai/oMr0fS3wvJ1pDWa29tDYkL+6d1qfZmVzturTZk5x3WkC1v8peupRCYLYUoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFucOjQIy0MYyC34AAAAASUVORK5CYII=)

### darken

> Decrease the lightness of a color in the HSL color space by an absolute amount.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `darken(hsl(90, 80%, 50%), 20%)`

输出： `#4d8a0f // hsl(90, 80%, 30%)`

![80e619](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZbQ6CMBAF5Rh6Wk6rx1AhYlbSLzUpo4z/sKn7fNO33YRhPB+vBz8YBwaBYFjMQgTC4iEQGA+BCITmAEyPd4hAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEzOZgkZT5cXK+6vAZ7PpbWSf7V9y3qsNf1ebV9PZpsAWRsTn0trLTAms1PGR9NT8JfvctB6QflLIDnzSqB2DaTUJloSkjrt67Yz1whtMNY0Iasj+03LyrW3d0DW7pAUTFvW43TnjI4Gpe6NlvaUMtk7JEw5LcbWJqXSXdCyd5dAaqNmbQzNrX86Stfq9WpXU51Npqyef/DXagkERkwgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDk3ACbeSboVqFeNAAAAABJRU5ErkJggg==)➜![4d8a0f](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABOklEQVRoQ+2Z0Q3CIBRF7QRO5joO4TpO5gRqm5IQhBYMDSd6/CzwcnvPuw+TTpfb+Xnyh3FgEgiGxSJEICweAoHxEIhAaA7A9HiHCATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5AxPyP36WCx5fwb4sCasxevxs9K5nMc152r2HM1vKJCc4ekLx8BSeFsw4zo152pr/SyQ2m4UyNEtsNbfA1JabzkXxuBeQtKaLaOwt11DRtZW188vWFqvNXYG0ZosR9Z6mccdluvob40NdVM4KfCw7++BbBlhQnoPwoZ6pX9apbnecofkUhI/S5sil9aGV+mydcgd0kX5jxYRCAysQAQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyXkBuIgGADAPwaAAAAAASUVORK5CYII=)

### fadein

> Decrease the transparency (or increase the opacity) of a color, making it more opaque.

Has no effect on opaque colors. To fade in the other direction use `fadeout`.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `fadein(hsla(90, 90%, 50%, 0.5), 10%)`

输出： `rgba(128, 242, 13, 0.6) // hsla(90, 90%, 50%, 0.6)`

### fadeout

> Increase the transparency (or decrease the opacity) of a color, making it less opaque. To fade in the other direction use `fadein`.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `fadeout(hsla(90, 90%, 50%, 0.5), 10%)`

输出： `rgba(128, 242, 13, 0.4) // hsla(90, 90%, 50%, 0.6)`

### fade

> Set the absolute transparency of a color. Can be applied to colors whether they already have an opacity value or not.

参数：

- `color`: 颜色对象。
- `amount`: 百分比 0-100%。

返回值： `color`

案例： `fade(hsl(90, 90%, 50%), 10%)`

输出： `rgba(128, 242, 13, 0.1) //hsla(90, 90%, 50%, 0.1)`

### spin

> Rotate the hue angle of a color in either direction.

While the angle range is 0-360, it applies a mod 360 operation, so you can pass in much larger (or negative) values and they will wrap around e.g. angles of 360 and 720 will produce the same result. Note that colors are passed through an RGB conversion, which doesn't retain hue value for greys (because hue has no meaning when there is no saturation), so make sure you apply functions in a way that preserves hue, for example don't do this:

```less
@c: saturate(spin(#aaaaaa, 10), 10%);
```

Do this instead:

```less
@c: spin(saturate(#aaaaaa, 10%), 10);
```

Colors are always returned as RGB values, so applying `spin` to a grey value will do nothing.

参数：

- `color`: 颜色对象。
- `angle`: A number of degrees to rotate (+ or -).

返回值： `color`

案例：

```less
spin(hsl(10, 90%, 50%), 30)
spin(hsl(10, 90%, 50%), -30)
```

输出：

```css
#f2a60d // hsl(40, 90%, 50%)
#f20d59 // hsl(340, 90%, 50%)
```

![f2330d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZUQ6CMBAF4QB6Ge9/BC+jB1AxaVKbgktI29EMf1BaHm/6dkmYb5fTY/LAODALBMPiLUQgLB4CgfEQiEBoDsD02EMEAnMAJseECATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByhifkfL1/WPL6HTDVrqWbWozV1l50jDiGAknm5i9fXsvPW4yVptc09QTzU0D2mBcFuWfNHmCGASlLz/KyZZlY26353LU5+XpbyYqUwh4g0jOGAVkEbJWHSOmIzv8GZCtNPWEsz0ICicDYA1QgwW0Vaer5Uq2augnJylVueO2TN42nXtHqszfS04L77NBtQ0vWIeV/OlkgMLACEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAm5wlb6RTg64SYfwAAAABJRU5ErkJggg==) ➜ ![f2a60d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABQklEQVRoQ+2ZUQrCMBAF2wPoAT2gB9QDqBUKMaTJ2lIytNMvCcn6+sa3G3B83C+vwQfjwCgQDIuvEIGweAgExkMgAqE5ANPjDBEIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMnpnpDr7fljyefvgKG0FvGtdi5SM90z6ejxdAUyG5C+fL5W2lMyKt3XqlGrGf2+vWAdEkhuVgtQuv+0QPIWMpmSt4klc0qtpVYvAiTS0vZKRVoXl5BZXAtGOmvyz1ONtS3stAnJTfunbdQuAnPKBLIiz5GhXpsHUdNbLatWZ8VrbTrSrWUt9fzSej5fomdLt7fZraV5lbrZ4+rbDcimn9GBDwsEBlcgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkvAEtyiPYGGTCCAAAAABJRU5ErkJggg==)

![f2330d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABNklEQVRoQ+2ZUQ6CMBAF4QB6Ge9/BC+jB1AxaVKbgktI29EMf1BaHm/6dkmYb5fTY/LAODALBMPiLUQgLB4CgfEQiEBoDsD02EMEAnMAJseECATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByhifkfL1/WPL6HTDVrqWbWozV1l50jDiGAknm5i9fXsvPW4yVptc09QTzU0D2mBcFuWfNHmCGASlLz/KyZZlY26353LU5+XpbyYqUwh4g0jOGAVkEbJWHSOmIzv8GZCtNPWEsz0ICicDYA1QgwW0Vaer5Uq2augnJylVueO2TN42nXtHqszfS04L77NBtQ0vWIeV/OlkgMLACEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAm5wlb6RTg64SYfwAAAABJRU5ErkJggg==) ➜ ![f20d59](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABQElEQVRoQ+2ZUQ7CIBAF7QH0fp7Q++kB1Bqb4AYKNhHGdPpZyPJ8w9tu4nQ9nu8HH4wDk0AwLF5CBMLiIRAYD4EIhOYATI/fEIHAHIDJMSECgTkAk2NCBAJzACbHhAgE5gBMjgkRCMwBmJzhCTndLh+WPP8OOOTeLZvW1nJ7cvWWffPa/LTU7MVtKJDFiMWY1JxoVmpsbi0altbOnRPhtdTsAUUg74TuHkhsE/PtS5NSS0tuvdR+1s6K6VlL0y4TUmol3wLa0qZSw+Pl6AFjPgPXsko3/5dAUrNNSGhVW252bQCotaXWAaBHSoYlpNTXc+/T70ttRG2tm5vs4jjcA0A8YxiQET/2H84UCIySQAQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyXkA4gUd0OoiZOkAAAAASUVORK5CYII=)

### mix

> Mix two colors together in variable proportion. Opacity is included in the calculations.

参数：

- `color1`: 颜色对象。
- `color2`: 颜色对象。
- `weight`: Optional, a percentage balance point between the two colors, defaults to 50%.

返回值： `color`

案例：

```less
mix(#ff0000, #0000ff, 50%)
mix(rgba(100,0,0,1.0), rgba(0,100,0,0.5), 50%)
```

输出：

```css
#800080
rgba(75, 25, 0, 0.75)
```

![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) + ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ➜ ![800080](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA50lEQVRoQ+2ZywqAIBQF9curL+8FRS1yIeidcNqJUMcZTgrmOc1r8sEQyArBuDiDKITlQyEwHwpRCI0ALI97iEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxbIhCYARgccIaMq3TC8WSl3tMmuvtK0TIBfyS8ByT5nrLCLsPIUEvZRlGyLHQr99SC1m13xtGSAvoLd6pkH1jbwG29p0KUUjcFS7paFvK0rslIcfe3ov80/cUArOlEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibDRGRhAx+4h2AAAAAElFTkSuQmCC)

### greyscale

> Remove all saturation from a color in the HSL color space; the same as calling `desaturate(@color, 100%)`.

Because the saturation is not affected by hue, the resulting color mapping may be somewhat dull or muddy; [`luma`](http://www.waibo.wang/bible/less/html/3/3.html#color-channel-luma) may provide a better result as it extracts perceptual rather than linear brightness, for example `greyscale('#0000ff')` will return the same value as `greyscale('#00ff00')`, though they appear quite different in brightness to the human eye.

参数： `color`: 颜色对象。

返回值： `color`

案例： `greyscale(hsl(90, 90%, 50%))`

输出： `#808080 // hsl(90, 0%, 50%)`

![80f20d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABLklEQVRoQ+2ZWw6CMAAE5QB6Zs6sB1AgQmrTQiFSRhz/eFiWGbYloWnv1+fFH4ZAoxCMiyGIQlg+FALzoRCF0AjA8riGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW57CGtLfHB4ruM8C0PXdsPCl1zjfHDPPUdHaIkBHceNPh9tyxWEZK4tYx58ZWSEcgBVYhOz4auellqSHx//qI8fRSOsbaaXJHHNPQp5mytrZnEPpez3pBscwaEsJrnEpIDuaaxihk4QlNASrdl2vNmpeKv2hIOE1M0Apfe3NrSGp/uL4svUqXrE015BwyZdW4sV+9hkJg5hSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJwX9P416NgjJOAAAAAASUVORK5CYII=) ➜ ![808080](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA70lEQVRoQ+2ZwQqDMBQE9c/z562KhQqa3PZNcLwUCcXNTPcF6tpa+yxeGAKrQjAujiAKYflQCMyHQhRCIwDL4xmiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVicsoZsf/tfUPzfk9bSvkqE/IDffZLW0jLK3oeQoPeyvEbIvtGnsTQClP5eWooj6zzLnn4ICukASo+6tAzPkE3+aESmpZSMrN4ZQlt7jZD0Rmd5XllDZgGUzqmQNPHB8xSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE/IFbiZXEBOOezsAAAAASUVORK5CYII=)

Notice that the generated grey looks darker than the original green, even though its lightness value is the same.

Compare with using `luma` (usage is different because `luma` returns a single value, not a color):

```less
@c: luma(hsl(90, 90%, 50%));
color: rgb(@c, @c, @c);
```

输出： `#cacaca`

![80f20d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABLklEQVRoQ+2ZWw6CMAAE5QB6Zs6sB1AgQmrTQiFSRhz/eFiWGbYloWnv1+fFH4ZAoxCMiyGIQlg+FALzoRCF0AjA8riGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW57CGtLfHB4ruM8C0PXdsPCl1zjfHDPPUdHaIkBHceNPh9tyxWEZK4tYx58ZWSEcgBVYhOz4auellqSHx//qI8fRSOsbaaXJHHNPQp5mytrZnEPpez3pBscwaEsJrnEpIDuaaxihk4QlNASrdl2vNmpeKv2hIOE1M0Apfe3NrSGp/uL4svUqXrE015BwyZdW4sV+9hkJg5hSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJwX9P416NgjJOAAAAAASUVORK5CYII=) ➜ ![cacaca](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA+UlEQVRoQ+2ZSw6DMBDF4FK5/zqXKgQp7YCKSqVocFWz4jfiYeuRBXOt9TG5YQjMCsG42IIohOVDITAfClEIjQAsj2uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxbIhCYARgcWyIQl4ESik7HOvfy+dxvBbPtxuy5zKd3daQDrXBjvsROOFapoz2LISQdy991oKjvOPs6DmFhE/Sp4acyRg59zdCvlkLOpS+llxdQ0bNZUq57ZOV+ZK/9CyFwGwpRCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4C6ih+7F+ivNMAAAAAElFTkSuQmCC)

This time the grey's lightness looks about the same as the green, though its value is actually higher.

### contrast

> Choose which of two colors provides the greatest contrast with another.

This is useful for ensuring that a color is readable against a background, which is also useful for accessibility compliance. This function works the same way as the [contrast function in Compass for SASS](http://compass-style.org/reference/compass/utilities/color/contrast/). In accordance with [WCAG 2.0](http://www.w3.org/TR/2008/REC-WCAG20-20081211/#relativeluminancedef), colors are compared using their [luma](http://www.waibo.wang/bible/less/html/3/3.html#color-channel-luma) value, not their lightness.

The light and dark parameters can be supplied in either order - the function will calculate their luma values and assign light and dark automatically, which means you can't use this function to select the *least* contrasting color by reversing the order.

参数：

- `color`: A color object to compare against.
- `dark`: optional - A designated dark color (defaults to black).
- `light`: optional - A designated light color (defaults to white).
- `threshold`: optional - A percentage 0-100% specifying where the transition from "dark" to "light" is (defaults to 43%, matching SASS). This is used to bias the contrast one way or another, for example to allow you to decide whether a 50% grey background should result in black or white text. You would generally set this lower for 'lighter' palettes, higher for 'darker' ones..

返回值： `color`

案例：

```less
contrast(#aaaaaa)
contrast(#222222, #101010)
contrast(#222222, #101010, #dddddd)
contrast(hsl(90, 100%, 50%), #000000, #ffffff, 40%);
contrast(hsl(90, 100%, 50%), #000000, #ffffff, 60%);
```

输出：

```
#000000 // black
#ffffff // white
#dddddd
#000000 // black
#ffffff // white
```

These examples use the calculated colors for background and foreground; you can see that you never end up with white-on-white, nor black-on-black, though it's possible to use the threshold to permit lower-contrast outcomes, as in the last 案例：

![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA4klEQVRoQ+3ZQQrCMBgFYXvJHLKX1CpUdGGXySdMd6GLvs7w/gSy7ft+v/UwBLaEMC5eQRJi+UgI5iMhCdEIYHnaQxKCEcDi1JCEYASwODUkIRgBLE4NSQhGAItTQxKCEcDiLGvIGOMLxXEN8F5L72b7WiLkBH5K+FxL72bLWHYfIkG/ypKQY2xJshKSkDUXVFILGlnHHEjI72G45JT1jCMdba+yzN5HlgmZ/aP/8r2EYKYSkhCMABanhiQEI4DFqSEJwQhgcWpIQjACWJwakhCMABanhiQEI4DFqSEJwQhgcR62xnPARPzoKQAAAABJRU5ErkJggg==) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA7ElEQVRoQ+2ZSw6AIBDF9Cpw/yNxFg1GjJ9Ed0MNdUdY8GjzmIVzSmmZ/DAEZoVgXGxBFMLyoRCYD4UohEYAlscZohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYnO4NKaVckOScjzVpL8pbVyEN+FlCuzhpL0pG9/8hJOhvWYYQcn+O6qVbU0h7kTJsyIn28A2pLHyynv1zqO9Mhm8IaU68ZRlqhkRf9g/ndX2y/gAoOqNCool/nKcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQmJAVTxrH8eXYvlcAAAAASUVORK5CYII=) ![dddddd](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA+UlEQVRoQ+2WMQ7DIBAE46/wF17N4xIRCetyhctjYo/bK1hmWPDRWnu//DAEDoVgXHyDKITlQyEwHwpRCI0ALI9viEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxbIhCYARgcbY3ZIxxIum9/+Ahzaq8bRcyN7rAZyG0WYUUhSTKV4fj9kLilTQ3GxtCmlWIWGtsa0g8iflUkmaVMuZaCklv2GOvLFILrrI8piHxDypuer0j+Q2Jb0z1rFLKtiurcpP/tJZCYLYUohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVicD7mA/fFYKXDNAAAAAElFTkSuQmCC) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA30lEQVRoQ+3ZwQqDMBQFUfPn/rm1BUu7qMvkFCY7ycLrDPdFyNiP7dhaDIGREMbFK0hCLB8JwXwkJCEaASxPZ0hCMAJYnBqSEIwAFqeGJAQjgMWpIQnBCGBxakhCMAJYnGUN2cc3ifMa4L2kvdm+lgi5gF8SPp+lvdkylt2HSNDvsiTkHFuSrIQkZM0FldSCRtY5BxLyexgu+ct6xpF+be+yzD5HlgmZ/aH/8r6EYKYSkhCMABanhiQEI4DFqSEJwQhgcWpIQjACWJwakhCMABanhiQEI4DFqSEJwQhgcR6UXwvo8egRpQAAAABJRU5ErkJggg==) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQ6AIBDE4OXqyzEaMUQTvC01lCsHhjbDHshrSSW5MASyQjAuziAKYflQCMyHQhRCIwDL4wxRCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACzO8IYsj++YLeUbEWkvyttQIRV4K6FenLQXJWP4fwgJei/LFEKez9Fx6doU0l6kDBvS0J6+IQcLn6x3/xzqF5PpG0KaE70sU82Q6Mv+4byhT9YfAEVnVEg08Y/zFKIQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcGwITsgMAzcPZnbCkdQAAAABJRU5ErkJggg==)

## 颜色混合函数

> 这些操作和图片编辑器（例如 Photoshop、Fireworks 或 GIMP）中的混合模式很*类似*（虽然不是完全一致），因此，你可以通过这些函数让 CSS 中的颜色与图片中的颜色相匹配。

### multiply

> 分别将两种颜色的红绿蓝 (RGB) 三种值做乘法运算，然后再除以 255，输出结果是更深的颜色。（译注：对应Photoshop中的“变暗/正片叠底”。）

参数：

- `color1`: 颜色对象。
- `color2`: 颜色对象。

返回值： `颜色（color）`

**案例**:

```less
multiply(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=)

```less
multiply(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![331400](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABMUlEQVRoQ+2ZUQ7CIBQEyx30JHr/E3gTD6GpCQYJpZSSdkzGL82jsuy8BZKG22V6TX4wDgSBYFh8hAiExUMgMB4CEQjNAZgezxCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZic0xLyeP6+hrlfw9eaWm0eFOvpM6mvpfqe+Y5kdgqQ3LD0d62Wwpi/l4Ckxsf6nvmOhIF5H1Lr+Fq350CWUiCQxrYqdXN8tKUmkEajtw4bkZCebW8GurZFbl3L3vGnnCG56BFA8vMlzrFmukAKt6Qt3d1zy/IMacht7zV07Uq8dBPrna9hKUOHILasoSv68z8TCAygQAQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyXkD6Av2GeKzFY4AAAAASUVORK5CYII=)

```less
multiply(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![662900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKUlEQVRoQ+2Z4Q3CIBgFyzg6mM7kYq6jwQTTEkuxGDjT85cJah93vo8mDZfT9Jh8YQgEhWBcvIIohOVDITAfClEIjQAsj2eIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxhjXkdl8+hrmewxsNaa23ryFCEvAoYf4+br527ZvPtlzjcELyDefw5ut7we79Xm8Zwx5Q5SMpBkkjq7RWK+dX7TmckFzC1ghLgNZatCbThmz8tVoAlUbaWoNarte7JcMP9ZbxUjp/am8OSrJ6yxh2hswlpE3X3PZ+Gkml86fmN7ey9JYypCG9N/lP11MIzJZCFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAIvzBCRUHGjNnHqXAAAAAElFTkSuQmCC)

```less
multiply(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![993d00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABLUlEQVRoQ+2ZUQ7CIBQEy8H0ON5Jj+PFNDXBIKlIgJRpHP8UeF123kITw/W0PBY/GAeCQDAsXkIEwuIhEBgPgQiE5gBMj3eIQGAOwOSYEIHAHIDJMSECgTkAk2NCBAJzACbHhAgE5gBMzrSEXO6ff8PczuFtTetYLJCuj3V7a+7FbQqQaE5u1vq9dSw3LK0zquYeUA4FpGS6QDrapbdjt46kreNq/W1k6jq2XL10SkJWdfmZHs37NZburBZs7bz02emdVu3mgInTgJSMbR37dm8IpKJTWi7dkrF5d7fU/+uEtL6GltaVjrvW51X01tApiCNr6I4OXkwgMIACEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAm5wmQXCSAu0GH4gAAAABJRU5ErkJggg==)

```less
multiply(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![cc5200](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABGUlEQVRoQ+2Z4QnCMBgFm5m6ic7pJnUmxWIghBKbBMuFnP80Bl7v+r4GGrbb8lr8YAgEhWBc7EEUwvKhEJgPhSiERgCWx2eIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxhmrI+jh+dfO8hx1rvh5/71m72teQQlLQEViUkcv5fG9du1rGcO9DcrApsFbopX3TCakdMUcjq7Yt6fj61Z6phKR3Zs3dXWpFDrt3nE0rJL/w0mg6O6bO/s+R9SVV8zxoudMV0tDv2mdI6fj6ryNxw2V1bRnq2Nt1pYNsVghMlEIUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi/MGNVAFwCKB/IwAAAAASUVORK5CYII=)

```less
multiply(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
multiply(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=)

```less
multiply(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![006600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDUlEQVRoQ+2ZUQ6CMBAF28vqmTwtKlJTG9NsILZDHP9ICTxm+hYTcrqkJfnDEMgKwbhYgyiE5UMhMB8KUQiNACyP7xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOJMa8hy+/wMk6/5jYa0NtrXFCEFeJFQH0fXouc9gR45VyFbcyKyalgt9N5adAOMljHtA9XeHduOsvUBtlEXWYtI7okdIei0I6sdRUclf5M1QkB7D4V0RqRCHuPnFzt97zX/Rkg9bspD+7f3RWLKyJqx885yT4XATClEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDh31uMoAB3khnMAAAAASUVORK5CYII=)

```less
multiply(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=)

### screen

> 与 `multiply` 函数效果相反，输出结果是更亮的颜色。（译注：对应Photoshop中的“变亮/滤色”。）

参数：

- `color1`: 颜色对象。
- `color2`: 颜色对象。

返回值： `颜色（color）`

案例：

```less
screen(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
screen(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![ff8533](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJklEQVRoQ+2WUQ6CMBAF5Ux6Ov/1dN4Jg0ljbUp1Y9NOwvAHheXxpm+XZb2d15MHxoFFIBgWLyECYfEQCIyHQARCcwCmxxkiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTMz8h18enJffL+7y2tnd/eT1VSfWi75kEai6QZFIOIRlRWyuv5eeRWq3nWnUGQDoGkNLICLwBEPJXzANSazF77WVT/K31tOqVqcvr/bI2EMo8INtHRndqq2X1SoEtK9v9uanRGSKQDjnumZDIwHeoV+D1niF5Cyx/ef9Z67DvIiXmzpCI0oPcKxAYaIEIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJPzBF9F4Jl1/aVfAAAAAElFTkSuQmCC)

```less
screen(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![ffa366](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2WQQ4CIRAE3X961jd51n+uWRMikgUHI6Gi5UkFZ5suesZlvZ7Wgy+MA4tAMCweQgTC4iEQGA+BCITmAEyPM0QgMAdgckyIQGAOwOSYEIHAHIDJMSECgTkAk2NCBAJzACZnfkKOl1dLbufn59banpGf1up9zkCIc4EkI3II6bCttRaMVCv/fe39Vqe1NtD4WunfAVKeMGp0L/jBkOYBKdvEdtDydueHr7WyMl153Ui9lo7B5u+VnwekbBetG56vfXLzyxREW5hAMgci8yVtfzeDBBK8WhHTay1p+z560wUSAPKtGZKnxL+9AePd0uXA3KHeJfU/NgsExlkgAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMCAzIHf8l60GiaCJiAAAAAElFTkSuQmCC)

```less
screen(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ffc299](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABUElEQVRoQ+2aUQ6CQAwF5WJexzN5HS+GYoTABpYC7tuHjn+G2pYZWkiwaR/39sLHhkCDEBsX70YQ4uUDIWY+EIIQNwJm/XAPQYgZAbN2mBCEmBEwa4cJQYgZAbN2mBCEmBEwa4cJQYgZAbN2qk9Ic71NkLxeBwzfc8fmOO7NtbVOSYdVhfQgxhL6k80dy8noc41/n+aKHisJfik3Qj4TOifyr4Ska6I7+RTKGMiWVZabiC4nE7Jwqe1ZWTmYKexx2SMXgHJSTrey9khMgX4jRylJPyNk7SEguqbW8pQSMTzM1PrXyZEVMveYOpcvd1/ack8qLWGyWmsJUZ7kmWpVXVlnAqXqFSEq0sE6CAmCUoUhREU6WAchQVCqMISoSAfrICQIShWGEBXpYB2EBEGpwhCiIh2sg5AgKFUYQlSkg3UQEgSlCkOIinSwDkKCoFRhTwZXLiDE9qb6AAAAAElFTkSuQmCC)

```less
screen(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![ffe0cc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKklEQVRoQ+2aUQ6CMBAF7U08hPc/gYfwJiAm1VJLtYrZMRm+ILTk8aZvlwBpupyngxvGgSQQDIubEIGweAgExkMgAqE5ANNjDxEIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMkJT0g6nlaWXD8H3I9751o+jo6HsYh//Z4NLCFkk3rnejDytUbnU+CEJkQgz8sgDEhdXhZp9eou5W6Vsq1EtGDvWR5/lagwIMsNfZKQcs7Wfuva746NLnV/C6ROT23kq+NyfjSElZbInxy+TUjPVIEMFtW9ekiv99RPb/aQQUgO9zcg3BoIbeo4NwCCBAKAgHnKgnmBkGNCEBgeIgQiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTMwN1ryLIIezIfQAAAABJRU5ErkJggg==)

```less
screen(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==)

```less
screen(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
screen(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ffff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA+0lEQVRoQ+2Z0QqEIBQF8/8/2igQLhYiZjbE7FtY7WHmnnXBlPOWNz8YAkkhGBdnEIWwfCgE5kMhCqERgOVxD1EIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALA6gIfVxTAqIZq6NvmutsY+FFEhRQgEwc61+V7xura2VATgPmQm9JVIhHaN1d3JcmjJ7TSEdQo5bbEgNyj3kMhStIemcswe3KUQh9eYbx+mtPST+PJbv6/17/WDcBx79uCEDiX/+iEJgghWiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJwdyY+zsVD29mAAAAAASUVORK5CYII=)

```less
screen(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ff66ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHklEQVRoQ+2ZWxKCMBAEzWU9lJdFsQoqRh5rANNo8ycbwux0JrGK1F277uKFcSAJBMPiKUQgLB4CgfEQiEBoDsD0eIYIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkNE9IuqUXSx6fA8bfU7VPx5d+b33+aH5NgQzm5BCGhqdq+b2yvlRbmjNSOxpCPv9pgcyt/Cm4EdOXFsdfACm3jr7pwcy5Ws0zJYzc3LX3fRPEqLPlJ9wtW1bfQHQLMyHBpSWQd6NOfYaYkODKjwyrPQ9q/7bWvi/Sy55jmiZkz0Z+ZS6BwEgKRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYnDtk2i+wJpWMIQAAAABJRU5ErkJggg==)

### overlay

> 结合 `multiply` 与 `screen` 两个函数的效果，令浅的颜色变得更浅，深的颜色变得更深。（译注：对应 Photoshop 中的“叠加”。）注意：输出结果由第一个颜色参数决定。

参数：

- `color1`: 基准颜色对象。也就是用以确定最终结果是浅些还是深些的参考色。
- `color2`: 颜色对象。

返回值： `颜色（color）`

案例：

```less
overlay(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=)

```less
overlay(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![ff2900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJUlEQVRoQ+2ZUQ6DIBQE5Tjt/c/S69hoYkpeEZUSmTTTzypk2WHQxDQ/pnnyh2kgCQTDYg0iEBYPgcB4CEQgtAZgeXyGCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsznhDXuFzzDN9Kipdu3r/NlvruJuBjQWylZRDiAWWAG3/5ePjXD2u3Qxj/PcQgXwhH2dIPEKWaHHn53GjRTUjlnEa0uD3VUNqx1kOIYJsPc4alvTrkHGGxF0cV7IHqwYxn0NDGvbGVUOOYOxB0JATcFqeIaUxtWfP0Sv0mVfiE0vpecvYI6vnSv5kLoHAQApEILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHHeNLvaQeOS/IQAAAAASUVORK5CYII=)

```less
overlay(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![ff5200](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKUlEQVRoQ+2ZUQ6CMBAF4Yp6Ts+IwaSxIaXsGqSTMP5uW58z7NaEeXlMy+QHQ2BWCMbFJ4hCWD4UAvOhEIXQCMDyeIcoBEYAFscOUQiMACyOHaIQGAFYHDtEITACsDh2iEJgBGBxxnfIa/M65jl/EW1re/DKnsxZve+paxcLGyukAGwBaNUy6+u1233R2sUyxr8PyQBe02bWR6H3ZN1KSGsc7Y2eFcxa6+2p4R1Bjsq6lZDsE9+CEx1rjqzgo5UZQREhe+cp5E9CMmCjI+xovAV/ylnLxv3L+uUOqcdcIdC7d8rd09t3VDuLdPCccUKCAe+2TCEw4wpRCIwALI4dohAYAVgcO0QhMAKwOHaIQmAEYHHsEIXACMDi2CEKgRGAxbFDFAIjAIvzBrtS2KlMlgN8AAAAAElFTkSuQmCC)

```less
overlay(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ff7a00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0Q6CIBhG5bXruue22cZiDNjvzDjO012J+HkOn7iV1seyLn4wBJJCMC4+QRTC8qEQmA+FKIRGAJbHPUQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDjzG/Kq/o55pi+i+lgPXnlOb8ye60TmO0nkXCEZUgtA61j52+jcGlY9djTPnnlPkHItISWAPeAUElg6rcdRbsro2DZ1pFnbuHq+1veRrMBt/HrINRvSE9J7FNmQ4LqJrPTo/pIv2dq8FTJJiA0Jgm8N+8cekq+7tczX3gOybnzq3E39xuB7t64Q2KJQiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsCEzIGz8j2Onx3/5WAAAAAElFTkSuQmCC)

```less
overlay(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![ffa300](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABPElEQVRoQ+2ZUQrCMBAF7T29nd6zUjEYQhPiw0cf7fgbd7uZyWYLXdbHbb3xiyGwICTGxbsQhGT5QEiYD4QgJI1AWD3MEISEEQgrhw5BSBiBsHLoEISEEQgrhw5BSBiBsHLoEISEEQgr5/gOuTefY57LF9FobQ+kmuvX5xglHiukgKgllM2O1kYySq46vs01u2YE30t9HiHtDmehj2RdSkh7TWybb093DaR3lbXdVefd65Yt56ysSwlpwYxOeL02gjn7P4R0jpo6Q2aG8Cx0rqxKjiKkB1od3Aj5CPnXDCl+t3nBa+8BU+/kjzz2tffkcJXtIUShZoxBiBGukhohCjVjDEKMcJXUCFGoGWMQYoSrpEaIQs0YgxAjXCU1QhRqxhiEGOEqqRGiUDPGIMQIV0mNEIWaMQYhRrhK6heEBuNRtJWa2QAAAABJRU5ErkJggg==)

```less
overlay(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ffcc00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABF0lEQVRoQ+2ZWw6DIBQFZU3d/w7qmmxsJCWkEsAHYzL+KTdwOMO5fBiW97RMPhgHgkAwLL5CBMLiIRAYD4EIhOYATI93iEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATM74hLyy3zFz+FlUGms18q51WnVl9WOBRJNSCFFgaax10/lc6XtprHWdE+oFIpDtGOUtZP0ck1IaW+ta248JqcxuT8uqbTc9dSnsf220cltHyp7Xso5CzE23ZSXn5wxz0+O4N58tqyK0d94hPfdOxRauKBnbsq7Y0cPnFAgMoEAEAnMAJseECATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMn5ANS737mlLIuNAAAAAElFTkSuQmCC)

```less
overlay(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=)

```less
overlay(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![ffcc00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABF0lEQVRoQ+2ZWw6DIBQFZU3d/w7qmmxsJCWkEsAHYzL+KTdwOMO5fBiW97RMPhgHgkAwLL5CBMLiIRAYD4EIhOYATI93iEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATM74hLyy3zFz+FlUGms18q51WnVl9WOBRJNSCFFgaax10/lc6XtprHWdE+oFIpDtGOUtZP0ck1IaW+ta248JqcxuT8uqbTc9dSnsf220cltHyp7Xso5CzE23ZSXn5wxz0+O4N58tqyK0d94hPfdOxRauKBnbsq7Y0cPnFAgMoEAEAnMAJseECATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMn5ANS737mlLIuNAAAAAElFTkSuQmCC)

```less
overlay(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=)

### softlight

> 与 `overlay` 函数效果相似，只是当纯黑色或纯白色作为参数时输出结果不会是纯黑色或纯白色。（译注：对应Photoshop中的“柔光”。）

参数：

- `color1`: 混合色（光源）。
- `color2`: 被混合的颜色。

返回值： `颜色（color）`

案例：

```less
softlight(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff2900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJUlEQVRoQ+2ZUQ6DIBQE5Tjt/c/S69hoYkpeEZUSmTTTzypk2WHQxDQ/pnnyh2kgCQTDYg0iEBYPgcB4CEQgtAZgeXyGCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsznhDXuFzzDN9Kipdu3r/NlvruJuBjQWylZRDiAWWAG3/5ePjXD2u3Qxj/PcQgXwhH2dIPEKWaHHn53GjRTUjlnEa0uD3VUNqx1kOIYJsPc4alvTrkHGGxF0cV7IHqwYxn0NDGvbGVUOOYOxB0JATcFqeIaUxtWfP0Sv0mVfiE0vpecvYI6vnSv5kLoHAQApEILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHHeNLvaQeOS/IQAAAAASUVORK5CYII=)

```less
softlight(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![ff4100](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJElEQVRoQ+2ZUQ6CMBAF4Wh6cK+GwdjYNHRZmgijGb9My9Lnm75uE+flNi2TH4wDs0AwLF5CBMLiIRAYD4EIhOYATI89RCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJuf6hDyav2Pu88eizFz9/FpZanrj5e3ZdU4Gdi2QnnmRsfXc+r1n7NZ4GavXbTVEmk6A83tAotT0QEamC+S9zVpj691+ZC5zZAkkme2jR1Z01JQlt94pkC8BaftH1KDtIUkI9WNHE5KpNSEDIPZ2etRD2qMp6j3Zq+3eRWHwJ46UXXvLGlH85zUCgQEWiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZgcEyIQmAMwOU/OtdQBUIKkrQAAAABJRU5ErkJggg==)

```less
softlight(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![ff5a00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABQ0lEQVRoQ+2ZUQ6CMBAF4Zp6Ps+JgdjYNG15JbI8w/hHunTXGXZLwrw8pmXiZ0NgRoiNi60QhHj5QIiZD4QgxI2AWT2cIQgxI2BWDh2CEDMCZuXQIQgxI2BWDh2CEDMCZuXQIQgxI2BWzvUd8io+xzznL6JyrQUvv6cVM5JH2e8kkdcKSZBqAGprvfgeoPK+/Lq3dhL03rYIQcjn+aiNo9QprbXePemxy2PK/WrXCMkadnRklb2ujiI1bt3/6Fj80Xj7r5G1JyQHmmLXrkCI+LiMdsjeeGkd1ggRhBw5Q1odkKdTz5/e6/VtX3sFb3cLufYMuRtt4f8iRIAUGYKQSNpCLoQIkCJDEBJJW8iFEAFSZAhCImkLuRAiQIoMQUgkbSEXQgRIkSEIiaQt5EKIACkyBCGRtIVcCBEgRYYgJJK2kOsNZCPT6UNnrBIAAAAASUVORK5CYII=)

```less
softlight(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ff7200](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHklEQVRoQ+2ZUQ6CMBAF6ZX1HJ4Z0YTYNC12taFjHP5gS3l9w1uakNbLsi4eGAeSQDAsnkIEwuIhEBgPgQiE5gBMj98QgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYnPkJuRW/Y67pZVFZa5m33xOZ6+g5ee1kYHOB7AbWDKjV8mtl/ej809rJMOb/D4kCyQ0SyODXpdaOWq3n8ehai2m1llGwBi+5Z7rfaln7iqLJsmX1vAvbmKix74C05hPIBCARuJHNQedSRg2b17JGf0NaW+Rvt8SjnO6cZx6QToH/NkwgMOICEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAm5w6Sst2pmqb2cAAAAABJRU5ErkJggg==)

```less
softlight(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![ff8a00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABM0lEQVRoQ+2ZUQ6CMBAF5dBeQg+NwUhsKq7i86UvZPgSym6XGbYlcZovp/nEEUNgQkiMi3shCMnygZAwHwhBSBqBsHrYQxASRiCsHDoEIWEEwsqhQxASRiCsHDoEIWEEwsqhQxASRiCsnPEdcu7+jrlOT0RbY9X9Fdy98wwSNVbICqmVsILYGuuvVfEt0Cru15wmYQhByOPV6peQ5fLaKXvG+u5qY/t8W+cIaXr930tWm+/d72X6b8dMy1KV9nhLVvUhQId8eMXokBdA4zpkzz5R7S/VHrI+7nIPn70DFuADTDmuQw4Az/EICHFQFXIiRIDnCEWIg6qQEyECPEcoQhxUhZwIEeA5QhHioCrkRIgAzxGKEAdVISdCBHiOUIQ4qAo5ESLAc4QixEFVyIkQAZ4j9AYBN+VpA6WYCAAAAABJRU5ErkJggg==)

```less
softlight(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ffa100](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2ZUQrCMBAF24t6N71oJWJwCSaEpSVDGT9r0z5n8pKA+/Hcjs0PhsCuEIyLTxCFsHwoBOZDIQqhEYDlcQ9RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACzO+oY8mr9jXvsP0ei7Hsg6Jj6n3Hv2ey4SuVZID14E2IIdgYjQ/4mt1+J72wyjTBdJiI+9l5CeyBF0hXznQ7uElMvtDI5Tp7eU9ZYmG5Loc2bJGi03NiQhIQ7JCJndoG1IQk5GiA1JgJ4ZctYeUt9VGpE92maO1zO/MXHP2lNWIvDdhygEZlghCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMV5A5AF3wFmks+VAAAAAElFTkSuQmCC)

```less
softlight(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff2900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJUlEQVRoQ+2ZUQ6DIBQE5Tjt/c/S69hoYkpeEZUSmTTTzypk2WHQxDQ/pnnyh2kgCQTDYg0iEBYPgcB4CEQgtAZgeXyGCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsznhDXuFzzDN9Kipdu3r/NlvruJuBjQWylZRDiAWWAG3/5ePjXD2u3Qxj/PcQgXwhH2dIPEKWaHHn53GjRTUjlnEa0uD3VUNqx1kOIYJsPc4alvTrkHGGxF0cV7IHqwYxn0NDGvbGVUOOYOxB0JATcFqeIaUxtWfP0Sv0mVfiE0vpecvYI6vnSv5kLoHAQApEILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHHeNLvaQeOS/IQAAAAASUVORK5CYII=)

```less
softlight(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![ffa100](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2ZUQrCMBAF24t6N71oJWJwCSaEpSVDGT9r0z5n8pKA+/Hcjs0PhsCuEIyLTxCFsHwoBOZDIQqhEYDlcQ9RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACzO+oY8mr9jXvsP0ei7Hsg6Jj6n3Hv2ey4SuVZID14E2IIdgYjQ/4mt1+J72wyjTBdJiI+9l5CeyBF0hXznQ7uElMvtDI5Tp7eU9ZYmG5Loc2bJGi03NiQhIQ7JCJndoG1IQk5GiA1JgJ4ZctYeUt9VGpE92maO1zO/MXHP2lNWIvDdhygEZlghCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMV5A5AF3wFmks+VAAAAAElFTkSuQmCC)

```less
softlight(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![ff2900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJUlEQVRoQ+2ZUQ6DIBQE5Tjt/c/S69hoYkpeEZUSmTTTzypk2WHQxDQ/pnnyh2kgCQTDYg0iEBYPgcB4CEQgtAZgeXyGCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsznhDXuFzzDN9Kipdu3r/NlvruJuBjQWylZRDiAWWAG3/5ePjXD2u3Qxj/PcQgXwhH2dIPEKWaHHn53GjRTUjlnEa0uD3VUNqx1kOIYJsPc4alvTrkHGGxF0cV7IHqwYxn0NDGvbGVUOOYOxB0JATcFqeIaUxtWfP0Sv0mVfiE0vpecvYI6vnSv5kLoHAQApEILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHHeNLvaQeOS/IQAAAAASUVORK5CYII=)

### hardlight

> 与 `overlay` 函数效果相似，不过由第二个颜色参数决定输出颜色的亮度或黑度，而不是第一个颜色参数决定。（译注：对应Photoshop中的“强光/亮光/线性光/点光”。）

参数：

- `color1`: 混合色（光源）。
- `color2`: 基准色对象。它决定最终结果是亮些还是暗些。

返回值： `颜色（color）`

案例：

```less
hardlight(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=)

```less
hardlight(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![662900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKUlEQVRoQ+2Z4Q3CIBgFyzg6mM7kYq6jwQTTEkuxGDjT85cJah93vo8mDZfT9Jh8YQgEhWBcvIIohOVDITAfClEIjQAsj2eIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxhjXkdl8+hrmewxsNaa23ryFCEvAoYf4+br527ZvPtlzjcELyDefw5ut7we79Xm8Zwx5Q5SMpBkkjq7RWK+dX7TmckFzC1ghLgNZatCbThmz8tVoAlUbaWoNarte7JcMP9ZbxUjp/am8OSrJ6yxh2hswlpE3X3PZ+Gkml86fmN7ey9JYypCG9N/lP11MIzJZCFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAIvzBCRUHGjNnHqXAAAAAElFTkSuQmCC)

```less
hardlight(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![cc5200](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABGUlEQVRoQ+2Z4QnCMBgFm5m6ic7pJnUmxWIghBKbBMuFnP80Bl7v+r4GGrbb8lr8YAgEhWBc7EEUwvKhEJgPhSiERgCWx2eIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxhmrI+jh+dfO8hx1rvh5/71m72teQQlLQEViUkcv5fG9du1rGcO9DcrApsFbopX3TCakdMUcjq7Yt6fj61Z6phKR3Zs3dXWpFDrt3nE0rJL/w0mg6O6bO/s+R9SVV8zxoudMV0tDv2mdI6fj6ryNxw2V1bRnq2Nt1pYNsVghMlEIUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi/MGNVAFwCKB/IwAAAAASUVORK5CYII=)

```less
hardlight(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![ff8533](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJklEQVRoQ+2WUQ6CMBAF5Ux6Ov/1dN4Jg0ljbUp1Y9NOwvAHheXxpm+XZb2d15MHxoFFIBgWLyECYfEQCIyHQARCcwCmxxkiEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTMz8h18enJffL+7y2tnd/eT1VSfWi75kEai6QZFIOIRlRWyuv5eeRWq3nWnUGQDoGkNLICLwBEPJXzANSazF77WVT/K31tOqVqcvr/bI2EMo8INtHRndqq2X1SoEtK9v9uanRGSKQDjnumZDIwHeoV+D1niF5Cyx/ef9Z67DvIiXmzpCI0oPcKxAYaIEIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJPzBF9F4Jl1/aVfAAAAAElFTkSuQmCC)

```less
hardlight(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![ff2900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJUlEQVRoQ+2ZUQ6DIBQE5Tjt/c/S69hoYkpeEZUSmTTTzypk2WHQxDQ/pnnyh2kgCQTDYg0iEBYPgcB4CEQgtAZgeXyGCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsznhDXuFzzDN9Kipdu3r/NlvruJuBjQWylZRDiAWWAG3/5ePjXD2u3Qxj/PcQgXwhH2dIPEKWaHHn53GjRTUjlnEa0uD3VUNqx1kOIYJsPc4alvTrkHGGxF0cV7IHqwYxn0NDGvbGVUOOYOxB0JATcFqeIaUxtWfP0Sv0mVfiE0vpecvYI6vnSv5kLoHAQApEILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHE0RCCwBmBxNEQgsAZgcTREILAGYHHeNLvaQeOS/IQAAAAASUVORK5CYII=)

```less
hardlight(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==)

```less
hardlight(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=)

```less
hardlight(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==)

```less
hardlight(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC)

### difference

> 从第一个颜色值中减去第二个（分别计算 RGB 三种颜色通道），输出结果是更深的颜色。如果结果为负值则被反转。如果减去的颜色是黑色则不做改变；减去白色将得到颜色反转。（译注：对应Photoshop中的“差值/排除”。）

参数：

- `color1`: 被减的颜色对象。
- `color2`: 减去的颜色对象。

返回值： `颜色（color）`

案例：

```less
difference(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
difference(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![cc3333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2ZQQrDIBQFmzMl9z9BeqaGFFJsFiLF7x/pZCeCvszwNJBlX9fXwwdDYFEIxsU7iEJYPhQC86EQhdAIwPJ4hygERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFme6hqz7/oXwuW2fccTcaF9TCbmAXxLKccTcaBnT/w+5SygBRsyNEJTakB5HTHlkncDKNXvNjRBx7ZEm5NfjJroFtWaNEIMQcn/RVigRx1Lr3lFyphIScXHX1oyCXls3Tcj9vD/HLZ+wPe6d1r3+TkjGC9P3TG0IHU5GPoVkUK/sqRCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCYkAM1zfPRyIPNWgAAAABJRU5ErkJggg==)

```less
difference(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![990066](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABCElEQVRoQ+2ZUQqDMBQEzWXtmexlLWlJSUOhUYhvSsY/jehmxjWCaVvWfXHDEEgKwbh4BlEIy4dCYD4UohAaAVge1xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOKENWTdtw8U93R775PGrvYVIqQALxLq/dFjR65/tYyw/yFHoIyQVYNus0RIqO85VUPqibft/DYWISdESJ5ou07kY78gXd2sqYT0vjZGvLJ6rzmVkF4oZ86rG5hbd7ZZUwopk/az90UibA2JePr+4Z4KgVlSiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBxHiguM+hBakKuAAAAAElFTkSuQmCC)

```less
difference(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![663399](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKklEQVRoQ+2ZYQ6CIABG80x1JjuTnak71azRiDmCnPCcr181GH6+5wduDeN5epz8YAgMCsG4eAVRCMuHQmA+FKIQGgFYHs8QhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDidGvIdB+/UFwvt89v0lhrX12EBOCzhPj7fPOlYzVz11zjcELSG07h5YDk5paKzclqLaPbH1TpljQHCVtWbiwAiufEW13cmqU102v8aujhhKwFVNqQVFQAXfMQtJLT/QypOQtqtrcaWfG6NVvmFpJ2JWTN4fzvmbIF9NyaXYQsbSG+9r41dRPS+snby/UUAjOlEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDiPAGgBCPgI21RngAAAABJRU5ErkJggg==)

```less
difference(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![3366cc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABLElEQVRoQ+2WUQ6CMBAF5Ux6Jj2Td8IzaTCpWRsoxYR2iMMf2dK+vunbMpyv4/Pkg3FgEAiGxVuIQFg8BALjIRCB0ByA6fEOEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyemWkPF+/rLicnt83veowXxflNMFSDI8QYjvtbXacUcBkXR2AZKblJsb6yVYS+OOBiHq7QoktqbYsiaBc7W8lU3j8pTFzZXmjN/m6+W1loC7Akkb/TUhta0uGj5BIre7vwSypWW2TMe0VhcgW05obQpKc+YJEcjMMdvj17Y059o9sfZtq6R0SUirzR1xHYHAqAlEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZicFzMJB9iJ/ATUAAAAAElFTkSuQmCC)

```less
difference(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![0099ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2ZUQ6CMBAF4bLeSS+LgUisBOtLlddHHP9Il+5mpts1YRyu0zTwiyEwIiTGxVIIQrJ8ICTMB0IQkkYgrB5mCELCCISVQ4cgJIxAWDl0CELCCISVQ4cgJIxAWDl0CELCCISV061DpssrifH2fD5ibd29dW+Xty5CViirhPL5iLWtjFK+suaS0e17yBHQa3sq0LfvOyWUuf6mQ7ZX1XIaH9dkbc0t5lRCZjjfgK11AR2yc0LnE6tcPa1XEELe9HvrDCk75JO8PfgIqVzArX8/f/UeM8Q9HU+ar8tQPykrS9kIsWDWkyBEZ2WJRIgFs54EITorSyRCLJj1JAjRWVkiEWLBrCdBiM7KEokQC2Y9CUJ0VpZIhFgw60kQorOyRCLEgllPghCdlSXyDt1d78FmZNbwAAAAAElFTkSuQmCC)

```less
difference(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![006600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDUlEQVRoQ+2ZUQ6CMBAF28vqmTwtKlJTG9NsILZDHP9ICTxm+hYTcrqkJfnDEMgKwbhYgyiE5UMhMB8KUQiNACyP7xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOJMa8hy+/wMk6/5jYa0NtrXFCEFeJFQH0fXouc9gR45VyFbcyKyalgt9N5adAOMljHtA9XeHduOsvUBtlEXWYtI7okdIei0I6sdRUclf5M1QkB7D4V0RqRCHuPnFzt97zX/Rkg9bspD+7f3RWLKyJqx885yT4XATClEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDh31uMoAB3khnMAAAAASUVORK5CYII=)

```less
difference(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![ff9900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDklEQVRoQ+2Z4QrCIBhF58v2TutlFwaS2Ar7cHqI08855e6cXQ2Wjn07Nn8YAkkhGBfPIAph+VAIzIdCFEIjAMvjGaIQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJz1Dbk1n2Pu6YXobOzX+8tq0XmTha0VUiDVElqAZ4LKtXp+u9aIscky1n8PUcib8nUNabeQHK198+u4eeyKFnxb04ZUBD61Z6ZIhXQIqSH1bnnRZimkQ0jksFZIx6sV2XrystG/r9F5HY8y8pZ1h/rIp/ijtRQCk6kQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOI8ACR378E6bVrjAAAAAElFTkSuQmCC)

```less
difference(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![ff66ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHklEQVRoQ+2ZWxKCMBAEzWU9lJdFsQoqRh5rANNo8ycbwux0JrGK1F277uKFcSAJBMPiKUQgLB4CgfEQiEBoDsD0eIYIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkNE9IuqUXSx6fA8bfU7VPx5d+b33+aH5NgQzm5BCGhqdq+b2yvlRbmjNSOxpCPv9pgcyt/Cm4EdOXFsdfACm3jr7pwcy5Ws0zJYzc3LX3fRPEqLPlJ9wtW1bfQHQLMyHBpSWQd6NOfYaYkODKjwyrPQ9q/7bWvi/Sy55jmiZkz0Z+ZS6BwEgKRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYnDtk2i+wJpWMIQAAAABJRU5ErkJggg==)

### exclusion

> 效果与 difference() 函数效果相似，只是输出结果对比度更小 (lower contrast)。（译注：对应Photoshop中的“差值/排除”。）

参数：

- `color1`: 被减的颜色对象。
- `color2`: 减去的颜色对象。

返回值： `颜色（color）`

案例：

```less
exclusion(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
exclusion(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![cc7033](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJklEQVRoQ+2W4Q2CMBgFZSYcTCdxMZxJI0mTpimUtgk9zPkP2pKXO96H0/KYPzd/GAKTQjAu1iAKYflQCMyHQhRCIwDL4zdEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4l2rI/Fqy+N7P+3o/XQ/3e9bO9nVJIT/QAX4qI3e9t7f0HIUcJFACma7Hj21dOxita9vQhvSOmNxI2mpMOrbis6W1LsKVh4cJid/SmpESw6sVEtjYkMxb0gMld7Y0whxZhaqeKaSmgXu5KqdP0/ZhI6vnr+gWtN5vUiCYfl+ayDYeGiqkMfNfH1MITK9CFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAi2NDFAIjAIvzBVqKBmhDlBboAAAAAElFTkSuQmCC)

```less
exclusion(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![997a66](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABUklEQVRoQ+2Z0Q7BQBBF+Vn+gWcfwc8SksratHItuXMbx1t12hnn9O42sT0fdtcNnxgCW4TEuHgMgpAsHwgJ84EQhKQRCJuHPQQhYQTCxiEhCAkjEDYOCUFIGIGwcUgIQsIIhI1DQhASRiBsnLKE7E7nFxSX4/55vHSu/366oL12ie9IvwpXJUImOBPI9lg919e9gzdy/woZZf+HqNDvAy6BHxXSg/7kPg5Jq0rIBOQdxHZp6hPYAlXOOQT0PUqEtE/+CCQlNaPLVHViyoS0IpQnfm7Tn9vM5zbvXyyRrrSUCRl9gtV9ZfT+f5uQb15DlUT1S+E3/VzpKHvLcv7AtfUqW7LWBso1L0JcpMU+CBFBucoQ4iIt9kGICMpVhhAXabEPQkRQrjKEuEiLfRAignKVIcRFWuyDEBGUqwwhLtJiH4SIoFxlCHGRFvsgRATlKrsBNxEi+M4wNPMAAAAASUVORK5CYII=)

```less
exclusion(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![668599](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKElEQVRoQ+2WWw6CMBQFZbO4B/91D7hZDSaY2vAoNbkdZfyCFNrDjKfQ9dfhcfKHIdApBOPiFUQhLB8KgflQiEJoBGB5fIcoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABanWUOGS/+B4ny7v89rxvJ7psmmeWvmbOGqiZAJzggrPR4BlI5tXZvC/GaNaCnNheQPnMOrBVt739r6EXKaCkkfcGlrGa8p2c7mtqx8zrnztfZECMjXaCpkL6A98JaurfkTRIo5jJC1Lax0LELM3wjZak/px8Ih3yHpF1L+eUobi2hFukaThkQ/5C+tpxCYLYUoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFucJYfY2sJIk5s4AAAAASUVORK5CYII=)

```less
exclusion(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![338fcc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABP0lEQVRoQ+2V0Q3CMAwF6UwwGJvAYGUmUJGKQhDtS5Q6T8r1r4obO3d1PJ1v8/PEY0NgQoiNi3chCPHygRAzHwhBiBsBs3qYIQgxI2BWDh2CEDMCZuXQIQgxI2BWDh2CEDMCZuXQIQgxI2BWTrcOma/nLxSX++PzXru2brD1vRn/n3K6CFmBrRLS99q1XEYq2F1CWl8XITmgXEK6rspCSIPfLr1a8j/631rJdbaU2PIqbHDk3S2G6RC107a6dZdmg4AhhZRcmQ0YF23RRUjt4N76bm+GqHOqiN4BwV2ELOcomQW1c4AZcsAfM9qW3TpkNNDqeRGikgqKQ0gQaDUNQlRSQXEICQKtpkGISiooDiFBoNU0CFFJBcUhJAi0mgYhKqmgOIQEgVbTIEQlFRSHkCDQahqEqKSC4hASBFpN8wJSoQVArXcjFQAAAABJRU5ErkJggg==)

```less
exclusion(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![0099ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2ZUQ6CMBAF4bLeSS+LgUisBOtLlddHHP9Il+5mpts1YRyu0zTwiyEwIiTGxVIIQrJ8ICTMB0IQkkYgrB5mCELCCISVQ4cgJIxAWDl0CELCCISVQ4cgJIxAWDl0CELCCISV061DpssrifH2fD5ibd29dW+Xty5CViirhPL5iLWtjFK+suaS0e17yBHQa3sq0LfvOyWUuf6mQ7ZX1XIaH9dkbc0t5lRCZjjfgK11AR2yc0LnE6tcPa1XEELe9HvrDCk75JO8PfgIqVzArX8/f/UeM8Q9HU+ar8tQPykrS9kIsWDWkyBEZ2WJRIgFs54EITorSyRCLJj1JAjRWVkiEWLBrCdBiM7KEokQC2Y9CUJ0VpZIhFgw60kQorOyRCLEgllPghCdlSXyDt1d78FmZNbwAAAAAElFTkSuQmCC)

```less
exclusion(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![006600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDUlEQVRoQ+2ZUQ6CMBAF28vqmTwtKlJTG9NsILZDHP9ICTxm+hYTcrqkJfnDEMgKwbhYgyiE5UMhMB8KUQiNACyP7xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOJMa8hy+/wMk6/5jYa0NtrXFCEFeJFQH0fXouc9gR45VyFbcyKyalgt9N5adAOMljHtA9XeHduOsvUBtlEXWYtI7okdIei0I6sdRUclf5M1QkB7D4V0RqRCHuPnFzt97zX/Rkg9bspD+7f3RWLKyJqx885yT4XATClEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDh31uMoAB3khnMAAAAASUVORK5CYII=)

```less
exclusion(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![ff9900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDklEQVRoQ+2Z4QrCIBhF58v2TutlFwaS2Ar7cHqI08855e6cXQ2Wjn07Nn8YAkkhGBfPIAph+VAIzIdCFEIjAMvjGaIQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJz1Dbk1n2Pu6YXobOzX+8tq0XmTha0VUiDVElqAZ4LKtXp+u9aIscky1n8PUcib8nUNabeQHK198+u4eeyKFnxb04ZUBD61Z6ZIhXQIqSH1bnnRZimkQ0jksFZIx6sV2XrystG/r9F5HY8y8pZ1h/rIp/ijtRQCk6kQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOI8ACR378E6bVrjAAAAAElFTkSuQmCC)

```less
exclusion(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![ff66ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHklEQVRoQ+2ZWxKCMBAEzWU9lJdFsQoqRh5rANNo8ycbwux0JrGK1F277uKFcSAJBMPiKUQgLB4CgfEQiEBoDsD0eIYIBOYATI4JEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkNE9IuqUXSx6fA8bfU7VPx5d+b33+aH5NgQzm5BCGhqdq+b2yvlRbmjNSOxpCPv9pgcyt/Cm4EdOXFsdfACm3jr7pwcy5Ws0zJYzc3LX3fRPEqLPlJ9wtW1bfQHQLMyHBpSWQd6NOfYaYkODKjwyrPQ9q/7bWvi/Sy55jmiZkz0Z+ZS6BwEgKRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYnDtk2i+wJpWMIQAAAABJRU5ErkJggg==)

### average

> 分别对 RGB 的三种颜色值取平均值，然后输出结果。

参数：

- `color1`: 颜色对象。
- `color2`: 颜色对象。

返回值： `颜色（color）`

案例：

```less
average(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![803300](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABCklEQVRoQ+2ZQQ6CMBQF6Zn0fur9vJMGk5rSSGMa+Z3GcWc+lMcMryxI19PyWPxhCCSFYFy8giiE5UMhMB8KUQiNACyP7xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOIMa8jlvv0MczunNxrSLNrXECEZeJZQ/ifNomUM+x7SC70GVK9Tzn8h+W+ErDe6ty21ZGVA5bnlVlev+6mB5THr/JvrRYqZasuyIQc9Gm5Z+2CnakivyN7zDnoem8sOEdJ6h9Bm0VKGCYm+0VmupxCYKYUoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFucJ2CUlCPPF1+MAAAAASUVORK5CYII=)

```less
average(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![994d1a](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABR0lEQVRoQ+2W0Q0CIRAFvTa0Ji3DnrQMe7IOzZlgkHDA/dyOOv4ROHi84e06XY77x84fxoFJIBgWLyECYfEQCIyHQARCcwCmxx4iEJgDMDkmRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTE5aQ8+3+YcX1dHiPW3NpUVqTf1fOzePavrVvKFxCgJRm5uPWXM/w3NSlfUpIFBBJx9cBGUnPfLlaglqpooD5OSAjpXBNmdsaVAiQ/AXnF05GlaamMtMqbWUq1iSkt++WUMKAtOr9aC9I60qQ83gNkNoDiWr8YUBGG/lS3e8Z3ptfgh7dZ8KBlK989LXmZa31l7ksd7XzeiX070rWlhemnxWWELoxUfoEEuX8wrkCEQjMAZgcEyIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQmBAnjx9ARDxstSgAAAAAElFTkSuQmCC)

```less
average(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![b36633](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2WUQ6CMBAF5Ux6DM+hZ/IcegzvpMEE06xQQcN2jMMXsKV9fcNb6M6H7W3jgXGgEwiGxUOIQFg8BALjIRCB0ByA6fEbIhCYAzA5JkQgMAdgckyIQGAOwOSYEIHAHIDJMSECgTkAk9MkIfvT9WnD5bh7saSs98VyTHYtm1cTIP0mB2MjkHi/vJ46j/PV5lg69u+BRANqEMqxU4DHDK2NXTLPGrCaJ2TY1FRSypYV29Xc2rBGrVW+a6NrmD82Z3MgPYi5b+zcdlZrid/UMqAIJLj89y0rJuTTFKzxXEYi4hrNEzL2Dcn+ta2tlw2lGZDsjf7KegKBkRKIQGAOwOSYEIHAHIDJMSECgTkAk2NCBAJzACbHhAgE5gBMjgkRCMwBmBwTIhCYAzA5d8IbHvDw/r2YAAAAAElFTkSuQmCC)

```less
average(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![cc804d](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABJ0lEQVRoQ+2WUQ6CMBQE5UyeQ6+kXslz6Jk0GGtqIy0gKdMw/DWvpcsO+9rudj48dj4YBzqBYFi8hAiExUMgMB4CEQjNAZgezxCBwByAyTEhAoE5AJNjQgQCcwAmx4QIBOYATI4JEQjMAZic5hKyP12/LLxfjp9xrhYmhTnxurTWj3/Va7BrCkhqZjzO1aYYngMmkMSBf4CMSU+/3aaBzGk/Q2tKCSkBKdVrpKPfY7WWNbbdzJmX/um5d+Tm1oIQ74MAkn74UNtYsmXFB3cJWE0wmwGSu2UJ5O3OkmdI3HqC+VOutqmWta6+qyWkZhtoaS+BwGgJRCAwB2ByTIhAYA7A5JgQgcAcgMkxIQKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYnCdkvRj4bT2jGgAAAABJRU5ErkJggg==)

```less
average(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![e69966](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2WWw6CMBQFZRluENeEC9RlaDApKZXH5ed2PsYvmxJ6mOkpDK9p/Nz8YQgMCsG4+AVRCMuHQmA+FKIQGgFYHt8hCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMWxIQqBEYDFQTbkPk4rTO/nYxlnz2X7wgkpwGcJ9f8ZzNE4Ohe9LltEWQ8tpIVyBeaR2Pq+7T17iUAIqY+fciy1R9IctJ3bGkdk1bAj6/WQ060hezs4ekxdgRuRtSc9W0p3IS3YMyHR4yb6LrqyXoac7kLqL6izF3c7H9350eu21s+QUK/RTUj98CXQ3rnuZ2/2tnC9hUDXhujhn4BCYLtCIQqBEYDFsSEKgRGAxbEhCoERgMWxIQqBEYDFsSEKgRGAxbEhCoERgMWxITAhX6ufOdjekcixAAAAAElFTkSuQmCC)

```less
average(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![ffb380](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABIklEQVRoQ+2Zaw6DIBgEywF7J+/UA1pttEGiBAyVqRn/KQ+XHfbDxDC+hvHhhXEgCATD4iNEICweAoHxEIhAaA7A9HiGCATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5HRPSHgOG0um3wHf+7Qt7hj3W5/XzJV7z97cV3HrCmQ1MGdu2nY0Jn0e359tuwpC/J7bAEnNE0jldtorR2saatqOEjTLKSlLufRULqlJ979OSGnJs2QV7pVSQzc1dvkImHd/6XiBdABy1nRL1gKr5pyIz4NffNrm5izcW826dT1Dmq3iRhMJBAZTIAKBOQCTY0IEAnMAJseECATmAEyOCREIzAGYHBMiEJgDMDkmRCAwB2ByTIhAYA7A5LwBGZ884JOc3RMAAAAASUVORK5CYII=)

```less
average(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![ff3300](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABIUlEQVRoQ+2ZUQqDMBAF9Uzt/U/QO1ksSMMSy7pYM+D0sybhOZMXBeflMS2TPwyBWSEYF58gCmH5UAjMh0IUQiMAy+MzRCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwOOMb8gqfY57zF1Hv2tHx22rVeRcLGytkg9RKiAB7grb/2vlxrTOuXSxj/PeQo0IioOz8qqxbCYlHyHrzcee3QPaOstiudt1ek9Y1s+25lZAI5sju/3Ws9a7ZkOTWyh45e8tl5yvkT0KqYKvzkrdx5rBxb1nVZ0j19bU670zaibXGCUmEu+MQhcCsK0QhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwOG/n9tfRi2GNwAAAAABJRU5ErkJggg==)

```less
average(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![80b300](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABOUlEQVRoQ+2ZUQ6CMBAF4X7eST2T3k+DoaY20goOssHxT0sf60x2qbE/Xbpb5ysMgV4hYVw8ClFILB8KCeZDIQqJRiBYPT5DFBKMQLBy7BCFBCMQrBw7RCHBCAQrxw5RSDACwcqxQxQSjECwcjbrkOPh9W+Y87V/oplayz/Pr08bl2QOe2v7fu1rEyEJQIKav6+t5fBKIUszW/dTyNg572TVhJTgPpWskJFcayy1hCQBU50yrLcyhnWFZDP725FVwsy7xA6ZMWyXzvtyZClkBvTapWsIWZrpyGo8Q1rH0DWOtn9/7IUabZcxm/wO2SVJ6EspBAJJxSiEIgnlKAQCScUohCIJ5SgEAknFKIQiCeUoBAJJxSiEIgnlKAQCScUohCIJ5SgEAknFKIQiCeUoBAJJxSiEIgnl3AEIWjwIF5UcdwAAAABJRU5ErkJggg==)

```less
average(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![803380](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABBklEQVRoQ+2ZQQqDMBQF65na+6n3650qLaSkAYMEzZ/QcSfB+JzxfRdOy3153TwwBCaFYFx8giiE5UMhMB8KUQiNACyP3xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOKENWR+zj8o1sf6PSet9fYVIiQBTxLyc9Jabxlh/0NaoZeAyn3y9TMk/42Q94PujaWarAQovzYfdeW+R8bgkfv1FDPUyLIhF70ajqx9sEM1pFVk63UXvY/VbUOE1L4htLXeUsKE9H7QUe6nEJgphSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwO5FDYIne50xQAAAABJRU5ErkJggg==)

### negation

> 与 `difference` 函数效果相反。

输出结果是更亮的颜色。**注意**：效果 *相反* 不代表做加法运算。

参数：

- `color1`: 被减的颜色对象。
- `color2`: 减去的颜色对象。

返回值： `颜色（color）`

案例：

```less
negation(#ff6600, #000000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![000000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA2klEQVRoQ+3ZQQqDMAAF0eT+h7ZKFVooLuMUnuBCssj3TyYGnGOMbb9dkQYmIBESZwxAWjwGIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCyLuBbfv+DTPnsTZ6Y6t5PWLIBeOC8PlcGlsN45gPkNPUX4sDkH3bYsgD/9RLpd9lYQhDfEMYEjza3h3BV29bj5yyVr/kP80HSIwWIIDEGojFYQggsQZicRgCSKyBWByGABJrIBaHIYDEGojFYQggsQZicRgCSKyBWJwX0nckECBGF/wAAAAASUVORK5CYII=) ![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=)

```less
negation(#ff6600, #333333);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![333333](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA/ElEQVRoQ+3ZMQ7CMBBEUXIm+/4n8J1AQTJatnC5+0GfLnKRYZ4mQeIaYzwffjANXIJgLN5BBGF5CALzEEQQWgOwPL5DBIE1AIvjQgSBNQCL40IEgTUAi+NCBIE1AIvjQgSBNQCL07aQtdZXFXPOzzXprNqrBWQXvhHiNemsGgPzf0hGiEWQziqAWhayv1h8NMVH1n1OOquA2PdoBckwGSXCEM4qYARJLZ8ekX8LQnpxn7JUAOR7tC2E9NP2lKUapQ2k+ov+yv0EgUkJIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVgcFyIIrAFYHBciCKwBWBwXIgisAVicF/vg8+kTGUjwAAAAAElFTkSuQmCC) ![cc9933](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABOUlEQVRoQ+2ZbQ6CQAwF5Ux6Ju+kZ8IzaTDBrBvD40MejzD+27TSOmO7Jjbt7fw88Yoh0CAkxsW7EYRk+UBImA+EICSNQFg/3CEICSMQ1g4TgpAwAmHtMCEICSMQ1g4TgpAwAmHtMCEICSMQ1s7uJuR8bb8QPu6Xz3mNmNvXroT0wHsJ5XmNmFvG7v4PmQu9Bls/p4wPxRyCNp2QqStmqZCyXrnqOtBDMYeIvsZmQpaumxJSvcJ+xcZOwWEn5B9rY+4z5r7PMSkREzJ3x49dYWPzytVVrzSHjM0v9al3SL3ruzM/e11flYPW2WxlHZS3/NgIkYi8CQjx8pbVECIReRMQ4uUtqyFEIvImIMTLW1ZDiETkTUCIl7eshhCJyJuAEC9vWQ0hEpE3ASFe3rIaQiQibwJCvLxltRdDyw/Qd4snowAAAABJRU5ErkJggg==)

```less
negation(#ff6600, #666666);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![666666](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZQQqAIBQF87J6Jk9bGBjmoqV/wmlVGPSc4alQyjmfhxeGQFIIxsUdRCEsHwqB+VCIQmgEYHncQxQCIwCLY0MUAiMAi2NDFAIjAItjQxQCIwCLY0MUAiMAixPWkFrrC0Up5Xkmja32FSKkA28Sxvs2edLYahlh/0NmCePESWPbCRkn3Jeseblq70SNbSdkBk1bwhQC21MUopC4X7iko+1XltUtCTn2rp7kn76nEJgthSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAW5wLy2y/geqx/aAAAAABJRU5ErkJggg==) ![99cc66](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABKElEQVRoQ+2WUQ6CMBAF5UzeCc+EZ9IzaTCpqQ2QdknoqOMfQu1jhrc4TLfxcfKDITAoBOPiFUQhLB8KgflQiEJoBGB5fIcoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABanW0PG8/SB4nq/vI+3zkX5Hb1fNGcXIQlOkpAfb52L3mTt75d7R/fbs+7vhJSwCBLyTF8ppHX8lNfPAMp25lDy8bnnaY+s7SJkDhqFFBk/R4/IiIi0ppuQPPTW2Gh5p6z9jkIqHpHIk543a2msKKQC/Nolre+BpUal72r+Mu/Zb8dtNi9FjKzm1D+8QCEwuQpRCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALM4TA+wjyOuzeE8AAAAASUVORK5CYII=)

```less
negation(#ff6600, #999999);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![999999](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA9ElEQVRoQ+2ZOw6EMBDF4LK5U3LZ5aegbArKGSNMB1PwYvMIEmut9bd4YAisCsG4OIMohOVDITAfClEIjQAsj3uIQmAEYHFsiEJgBGBxbIhCYARgcWyIQmAEYHFsiEJgBGBx0hpSSvlD0Vq7z0mzaF8pQjrwLmE8J82iZaT9DyFBf8qikP21RZL1GSHHQud94rg2v8JGIFmzaCkpe8i8yLkV45w0i5CTJoS6kT89AJ8Q0hfpZ+9FIq0hEU/bG++hEJg1hSgERgAWx4YoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWZwOzGkfIzOt2RQAAAABJRU5ErkJggg==) ![66ff99](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABQ0lEQVRoQ+2WUQ6CMBAF4bJ6JrwspiaY2pBSeAt9wfEL0+6yzLBbxsc8zQM/GwIjQmxcfApBiJcPhJj5QAhC3AiY1cMZghAzAmbl0CEIMSNgVg4dghAzAmbl0CEIMSNgVg4dghAzAmbldOuQaXj8oHgOr+//2tqyaW/83v29PHURssBJEvLrBKG2VsrIJdbW9tyj3Hu1mO5CygduAVLbs7aGkI3XqhwfafvyttfW8g7Kb7EVi5BGISXIrRF2dGQdFXn1uEr3sxhZrWeKIiSHu3fkXSnmb4S0Sm85w84U1EXI2ghp/ew9ev7w2Xvma3Tj3N065MZMpUdDiIQvPhgh8UyljAiR8MUHIySeqZQRIRK++GCExDOVMiJEwhcfjJB4plJGhEj44oMREs9UyogQCV98MELimUoZESLhiw9GSDxTKeMbJIr3sesvdroAAAAASUVORK5CYII=)

```less
negation(#ff6600, #cccccc);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![cccccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA5klEQVRoQ+2ZMRKAIBDE5E/8/wX8ScUZFGnsjjDGSucKlsSVwlRK2TcvDIGkEIyLK4hCWD4UAvOhEIXQCMDyeIYoBEYAFseGKARGABbHhigERgAWx4YoBEYAFseGKARGABbHhigERgAWx4Yo5CGQc37hOP9e3s+kWaSzaQ1pwKuE/r5unjSLlFHXQggZNz0K6ufRM4UMDZkt6zdC+k9T27RnyMRPVvSbt8p6086QVQBF51RINPGP9RSiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBsCE3IAKYDzoYL0FMoAAAAASUVORK5CYII=) ![33cccc](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABG0lEQVRoQ+2aQQ6DIBRE65ns/U+AZ2pjExrKgoDQ/Gfy3JmvZpjHDBu3PaXXwwvjwCYQDIuPEIGweAgExkMgAqE5ANPjGSIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJMTlpC07z9WPI/je9+awfxbLicESDY8QyjvW7Plqwd+MARI7UMNoZy3ZkA/pyWFAimrqaysc1W9s/PZ3rq7Q02GAsnbaSQhvfV2tRajE3lrIKuqLxpCuY4QIFd3b1lldcX9azZ9KAx+IARIfUaMnAMz73qGDO4OH/c3INweCKssnBMQQQKBgMgyBCIQmAMwOSZEIDAHYHJMiEBgDsDkmBCBwByAyTEhAoE5AJNjQgQCcwAm5w0+8/O5CbJHwwAAAABJRU5ErkJggg==)

```less
negation(#ff6600, #ffffff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ffffff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAAA6ElEQVRoQ+2ZOw6AIBAF3fsf2g/RiMZgtwxhbCl8zPDYglj3b/HDEAiFYFyUIAph+VAIzIdCFEIjAMvjDFEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALE73hkTEA0n9PENay/LWVcgF/OuNjLSWJaP7ewgJeivLFELe19Gx6asppLVMGTakoj19Q8ppOAe6M+Q+GQ71k8X0DSHNiVaWqWZI9mZH+F/XK2sEQNkZFZJN/Od/ClEIjAAsjg1RCIwALI4NUQiMACyODVEIjAAsjg1RCIwALI4NUQiMACyODYEJ2QD93i+YgM+JAQAAAABJRU5ErkJggg==) ![0099ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABL0lEQVRoQ+2ZUQ6CMBAF4bLeSS+LgUisBOtLlddHHP9Il+5mpts1YRyu0zTwiyEwIiTGxVIIQrJ8ICTMB0IQkkYgrB5mCELCCISVQ4cgJIxAWDl0CELCCISVQ4cgJIxAWDl0CELCCISV061DpssrifH2fD5ibd29dW+Xty5CViirhPL5iLWtjFK+suaS0e17yBHQa3sq0LfvOyWUuf6mQ7ZX1XIaH9dkbc0t5lRCZjjfgK11AR2yc0LnE6tcPa1XEELe9HvrDCk75JO8PfgIqVzArX8/f/UeM8Q9HU+ar8tQPykrS9kIsWDWkyBEZ2WJRIgFs54EITorSyRCLJj1JAjRWVkiEWLBrCdBiM7KEokQC2Y9CUJ0VpZIhFgw60kQorOyRCLEgllPghCdlSXyDt1d78FmZNbwAAAAAElFTkSuQmCC)

```less
negation(#ff6600, #ff0000);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![ff0000](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHElEQVRoQ+2Z7Q6CMBAE6fs/NPhBY1OLxo1LN2SMv6y3OWa40oSyLsvtyyeFQEFIiopnHwjJ8oGQMB8IQUgagbB+eIYgJIxAWDtMCELCCIS1w4QgJIxAWDtMCELCCIS1w4QgJIxAWDvzJ2TtXseU8kI0Wvv1/zVNrTtZ2FwhFVIroQc4ElR/a+v7rH+snSxj/vsQhLwpnzch/RbyuD327epozTEFnzKZkIbAaHoQYr5F2LKCtqx7KwgJEqI8Q1qJ9VK+HZM59pq3uYvHzztlXRysenkIUcmZ6hBiAqvGIkQlZ6pDiAmsGosQlZypDiEmsGosQlRypjqEmMCqsQhRyZnqEGICq8YiRCVnqkOICawaixCVnKkOISawauwGg+vf2ReonYcAAAAASUVORK5CYII=) ![006600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDUlEQVRoQ+2ZUQ6CMBAF28vqmTwtKlJTG9NsILZDHP9ICTxm+hYTcrqkJfnDEMgKwbhYgyiE5UMhMB8KUQiNACyP7xCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOJMa8hy+/wMk6/5jYa0NtrXFCEFeJFQH0fXouc9gR45VyFbcyKyalgt9N5adAOMljHtA9XeHduOsvUBtlEXWYtI7okdIei0I6sdRUclf5M1QkB7D4V0RqRCHuPnFzt97zX/Rkg9bspD+7f3RWLKyJqx885yT4XATClEITACsDg2RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwODZEITACsDh31uMoAB3khnMAAAAASUVORK5CYII=)

```less
negation(#ff6600, #00ff00);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![00ff00](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABFUlEQVRoQ+2aWwqDMBQFm/0vuhVtIIQ8hBvMINN+lag5nfGYUk2f7/H2hSGQFIJxcQZRCMuHQmA+FKIQGgFYHtcQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDibGtIfRsmHX+r5ddo7M42rf2j8z3lbYuQDCdLKD+PxmoZpcTRWHS+p2Rsux8SBVTvXwJrjUXnU8h55+y6hPVglpDqbesxhUxOqSggG7K4swrpA3VR/z9007tELj4Xp4fbIqRcG3LCuz97W08tzdaQyHxTgos32CZk8fd4zeEUAlOpEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi/ADkXt/ZzdvOhAAAAABJRU5ErkJggg==) ![ff9900](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABDklEQVRoQ+2Z4QrCIBhF58v2TutlFwaS2Ar7cHqI08855e6cXQ2Wjn07Nn8YAkkhGBfPIAph+VAIzIdCFEIjAMvjGaIQGAFYHBuiEBgBWBwbohAYAVgcG6IQGAFYHBuiEBgBWJz1Dbk1n2Pu6YXobOzX+8tq0XmTha0VUiDVElqAZ4LKtXp+u9aIscky1n8PUcib8nUNabeQHK198+u4eeyKFnxb04ZUBD61Z6ZIhXQIqSH1bnnRZimkQ0jksFZIx6sV2XrystG/r9F5HY8y8pZ1h/rIp/ijtRQCk6kQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOI8ACR378E6bVrjAAAAAElFTkSuQmCC)

```less
negation(#ff6600, #0000ff);
```

![ff6600](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABEElEQVRoQ+2Z4Q6CIBhF5WXrmXxaGy2WIzQ/3OS4nf4VYLd7OOJWWh7TMvnCNJAEgmHxDiIQFg+BwHgIRCC0BmB5PEMEAmsAFkdDBAJrABZHQwQCawAWR0MEAmsAFkdDBAJrABZnvCFz9XfMM30rao1F55er9a67GNhYIKWkNYS6wBag/Fm9dv1+byxfPzJXIJ8GWrCiALfgCqSxzepbSJ5SbNga61lTG6EhO85Hd3zvbap33cW3q/x19z1DIjtdIAe3lob8FDXOkLPnQfkp/x6Tfew9aIfTmg2MM0QgArnDHtAQGCWBCATWACyOhggE1gAsjoYIBNYALI6GCATWACyOhggE1gAsjoYIBNYALM4LK4vnyTSosnoAAAAASUVORK5CYII=) ![0000ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABHUlEQVRoQ+2Z0QqDMBAE9f8/2tpiIE2LCLtJFh3fSrjzOtNNClmXZdsWnhgCK0JiXHwGQUiWD4SE+UAIQtIIhM3DGYKQMAJh45AQhIQRCBuHhCAkjEDYOCQEIWEEwsYhIQgJIxA2zrSEtLcw6z5JeXqsqb1HeZsipAAvEurPPdZaGbX8K2ujZEy7D+kB/aznFeht/UgJ9bsek5B/F9VtQr/AVFvoSDmPEfKGepYCErID4gz5zR4JOZg8OiH19lF+I73/9nKGjDwJb/SuKVvWjfjZvwpC7Ei1hgjR+NmrEWJHqjVEiMbPXo0QO1KtIUI0fvZqhNiRag0RovGzVyPEjlRriBCNn70aIXakWkOEaPzs1QixI9UaIkTjZ69+AUTg39mlpcxCAAAAAElFTkSuQmCC) ![ff66ff](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAGQAAAAoCAYAAAAIeF9DAAABF0lEQVRoQ+2WYRKCIBgF87J1pk5LYzPMMKT0hRprbf8KhOcuT5vSNaWLHwyBSSEYF88gCmH5UAjMh0IUQiMAy+M7RCEwArA4NkQhMAKwODZEITACsDg2RCEwArA4NkQhMAKwOOMbcq+I3IrvS2Ofzq+Bb73+YIFjhWQ4pYR8w0tj5W/1eGustWZk7GAJ5fLnFbJ28pfkRqC3DsdfCKkfHfNNZ5hrYz3X1DJKuO/2+6KIvNW5GxJ9hNmQ4NHa8g6Zt1BIEHR0mkJeSI17ZPW+D3r/tvbuFz1cO80bJ2SnG/i1ZRQCM6oQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOLYEIXACMDi2BCFwAjA4tgQhcAIwOI8AMCpv6H+U2B6AAAAAElFTkSuQmCC)

 

 