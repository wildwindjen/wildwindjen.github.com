---
layout: post
title: "Lexical Scope"
date: 2013-09-09 23:42
comments: true
categories: 
- JavaScript
---
## Scope
	定義：可以用以尋找變數的範圍。

## Lexical Scope
用來決定「可以使用的 Scope 有哪些」的方式之一，相對的有 Dynamic Scope。

特性：程式的語法結構，就決定了可以在哪些 Scope 尋找變數。

看看以下的範例：

{% codeblock function definition %}
function b(){
	alert(foo);
}
function a(){
	var foo=1;
	b();
};
a();
{% endcodeblock %}

這個範例在使用 Lexical Scope 的語言裡，是行不通的。例如 JavaScript 就會報 `ReferenceError: foo is not defined` 的錯。原因在於 b function 可以尋找變數的 Scope 只有 b function 和 global enviroment。並不包含 a function。 -- 程式碼的樣子，決定了可以用的 Scope。

Dynamic Scope 的語言會在執行時期，會依照呼叫堆疊(call stack)，來動態決定可以使用的 Scope。所以上述例子，因為可以使用的 Scope 有 b function 、 a function 和 global environment， 就會成功執行，印出 1 。 -- 程式的執行順序，決定了可以用的 Scope。


## Closure
為什麼要了解 Lexical Scope？ 因為這跟 JavaScript 的 Closure 有關。

看看這個 Closure 的例子：
{% codeblock function definition %}
function a(value){
	return function a(){
		alert(value);
	};
}

var f = a(1);
var value = 3;
f();
{% endcodeblock %}

如果今天 JavaScript 不是 Lexical Scope 特性的語言。呼叫 f() 的結果，就不會是 1 ，而是 3。Closure 的運用，常見於拍下快照(snapshot)，捕捉當時的值。這是很久以前犯過的錯：

{% codeblock HTML %}
<input type="button" value="1">
<input type="button" value="2">
<input type="button" value="3">
{% endcodeblock %}

{% codeblock jQuery %}
var s = $("input").size();
for(var i=0; i<s; i++){
	$("#wade" + i).click(function(){
		alert(i);
	});
}
{% endcodeblock %}

想要按下按鈕時，分別是 0 1 2 ，可是不管按哪一顆，都是 3 。因為大家抓到的 i ，都是「現在」的值。這時候就是要利用 Closure 製造快照的效果，記住「當時」的值。改成：

{% codeblock jQuery %}
var s = $("input").size();
for(var i=0; i<s; i++){
	$("#wade" + i).click(function(n){
		return function(){
			alert(n);
		}
	}(i));
}
{% endcodeblock %}

這個例子把想要快照起來的值，當作參數傳遞進去。然後回傳一整個內部函式，代表「當時」的執行狀況。回傳內部函式的時候，會順便創造該時空下的執行環境。可以想像成複製當時 Scope 的樣子，所以當時 Scope 裡變數的值如果是 0 就是 0。

關於 Closure 的細節 ，以後再說吧，畢竟這一篇的主角是 Lexical Scope。