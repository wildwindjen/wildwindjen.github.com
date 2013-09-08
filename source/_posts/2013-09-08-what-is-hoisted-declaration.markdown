---
layout: post
title: "what is hoisted declaration"
date: 2013-09-08 22:13
comments: true
categories: 
- JavaScript
---
## Variable Hoisted
猜猜看以下的程式，答案是什麼？
{% codeblock variable hoisted sample 1 %}
var a = 1;
function t(){
	console.log(a);
	var a = 2;
}

t();
{% endcodeblock %}

事實上他等同以下程式，JavaScript 底層把宣告的動作自動往上提(hoisted)了。
{% codeblock variable hoisted sample 2 %}
var a = 1;
function t(){
	var a;			// declaration hoisted!!
	console.log(a);
	var a = 2;
}

t();
{% endcodeblock %}

## Function Hoisted
### 兩種函式定義
要說明 Function Hoisted 之前，我們拿兩種定義方式來比較： function definition 和 function declaration。

{% codeblock function definition %}
var f = function(){ 
	return 1;
};
{% endcodeblock %}

以及

{% codeblock function declaration %}
function f(){ 
	return 1;
};
{% endcodeblock %}

### 兩種函式的 Hoist 比較
以下結果是 `TypeError: undefined is not a function`。確實 var b 往上提了，可是函式主體沒有，所以呼叫了 undefined 當函式。
{% codeblock function hoisted sample 1 %}
function b(){
		console.log('w1');
	}

function a(){
	b();
	var b = function(){
		console.log('w2');
	}
}

a();
{% endcodeblock %}

如果是使用 function declaration，則結果可以正常執行，執行的 b() 會是 w2。因為除了內部的 function b 宣告以外，連函式主體，都往上提了。
{% codeblock function hoisted sample 1 %}
function b(){
		console.log('w1');
	}

function a(){
	b();
	function b(){
		console.log('w2');
	}
}

a();
{% endcodeblock %}