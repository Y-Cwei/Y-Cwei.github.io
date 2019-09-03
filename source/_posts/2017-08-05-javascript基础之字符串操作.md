---
title: js对字符串的操作
date: 2017-08-05 19:13:13
tags: js
---
## js对字符串的操作

### 1. 将其它类型的数据转换为字符串
> 语法：  
> 可以调用 stringObj.toString() 方法，String(stringObj) 方法，或者饮食转化 "" + stringObj;  
> 使用如下：   

<!--more-->

	var num = 19;
	var numStr0 = num.toString();
	var numStr1 = String(num);
	var numStr2 = '' + num;

### 2. 获取字符串的长度
> 语法：  
> stringObj.length

	var str = 'javaScript';
	console.log(str.length); // 10

### 3. 字符串替换
> 语法：  
> stringObj.replace(rgExp, replaceText)，返回替换后的字符串，原字符串不变，参数说明如下：  
> reExp可以是正则表达式对象(RegExp)，也可以是字符串(string)，replaceText是替代的查找到的字符串；  
> 使用如下：  

	var str = 'php是世界上最好的语言';
	var strRes = str.replace('php', 'javascript才');
	console.log(str + '?开玩笑，' + strRes);

### 4. 查找给定位置的字符或其字符编码值
> 语法：  
> 1. 查找指定位置的字符： stringObj.charAt(index)，返回查找到的字符，不影响原数组；  
> 参数解析，index为要查找的指定位置字符的下标索引。
> 2. 查找指定位置字符的字符编码值，stringObj.charCodeAt(index)  

	// 获取指定位置的字符
	var str = 'javaScript';
	var strCharAt = str.charAt(6);
	var strCharCodeAt = str.charCodeAt(6);
	console.log(str+"的第7位字符是:"+strCharAt+",其字符编码值为："+strCharCodeAt); // javaScript的第7位字符是:r,其字符编码值为：114

### 5. 字符串连接  
> 语法：  
> 1. 直接使用 + ，  
> 2. stringObj.concat(stringObj2)，返回拼接后的字符串，对原字符串无影响   

	var firstStr = 'java';
	var lastStr = 'script';
	var strRes = firstStr.concat(lastStr);
	console.log(strRes+"是由字符串"+firstStr+'和'+lastStr+'拼接而成的'); // javascript是由字符串java和script拼接而成的

### 6. 字符串的切割和提取
> 语法：  
> 1. stringObj.slice(start, end)，返回截取得到的字符串，不影响原字符串，  
> 2. stringObj.substring(start, end)，返回截取得到的字符串，不影响原字符串，  
> 3. stringObj.substr(start, end)，返回截取得到的字符串，不影响原字符串。  
> 参数解析：  
> 1. 对于slice()和substring()方法而言：
> start： 必选，非负，指定开始位置的下标，  
> end: 可选，非负，指定结束位置的下标，不传，默认到字符串末尾，  
> 返回的子串包括 start 处的字符，但不包括 stop 处的字符。  
> 2. 对于substr()方法而言：  
> start: 必选，非负，指定开始位置的下标，  
> end: 可选，非负，指定截取的最大长度，不传，默认到字符串结尾。  
> 3. 对于三种方法而言，参数end如果是一个大于字符串长度的值，则截取到字符串结尾终止

	var str = 'javaScript';
	var sliceStr0 = str.slice(1, 5);
	var sliceStr1 = str.slice(1, 15);
	var substringStr0 = str.substring(1, 5);
	var substringStr1 = str.substring(1, 15);
	var substrStr0 = str.substr(1, 5);
	var substrStr1 = str.substr(1, 15);
	console.log(str); // javascript
	console.log(sliceStr0); // avaS
	console.log(sliceStr1); // avaScript
	console.log(substringStr0); // avaS
	console.log(substringStr1); // avaScript
	console.log(substrStr0); // avaSc
	console.log(substrStr1); // avaScript

### 8. 字符串大小写转换
> 语法：  
> stringObj.toLowerCase(): 将字符串转换为小写，返回被转换之后的字符串，不影响原字符串。  
> stringObj.toLocaleLowerCase(): 将字符串转换为小写，返回被转换之后的字符串，不影响原字符串。  
> stringObj.toUpperCase(): 将字符串转换为大写，返回被转换之后的字符串，不影响原字符串。  
> stringObj.toLocaleUpperCase(): 将字符串转换为大写，返回被转换之后的字符串，不影响原字符串。  

	var str = 'JavaScript';
	var lowerCase = str.toLowerCase();
	var localLowerCase = str.toLocaleLowerCase();
	console.log(str+"转化为小写为："+lowerCase); // JavaScript转化为小写为：javascript
	console.log(str+"转化为小写为："+localLowerCase); // JavaScript转化为小写为：javascript
	var upperCase = str.toUpperCase();
	var localUpperCase = str.toLocaleUpperCase();
	console.log(str+"转化为大写为："+upperCase); // JavaScript转化为大写为：JAVASCRIPT
	console.log(str+"转化为大写为："+localUpperCase); // JavaScript转化为大写为：JAVASCRIPT

### 7. 把字符串分割成数组 —— split()
> 用法： stringObject.split(separator,howmany)  
> separator：必选，类型为字符串或者数组，指定要切割的地方。(返回数组含用来指定切割的字符)  
> howmany：可选择，返回数组的最大长度。

	var str1 = 'JavaScript';
	console.log(str1.split('')); // ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]
	console.log(str1.split('', 6)); // ["J", "a", "v", "a", "S", "c"]
	console.log(str1.split('', 20)); // ["J", "a", "v", "a", "S", "c", "r", "i", "p", "t"]

	var str2 = 'Java Script';
	console.log(str2.split(/\s+/)); // ["Java", "Script"]

### 8. 查询字符串
> 语法：  
> stringOb.indexOf(searchvalue,fromindex),从字符串的开头开始查找，找到返回对应的下标，找不到返回-1，此方法对大小写敏感。  
> stringOb.lastIndexOf(searchvalue,fromindex),从字符串的末尾开始查找，找到返回对应的下标，找不到返回-1，此方法对大小写敏感。  
> 参数解析：  
> 相同点：  
> searchvalue：必须，规定检索的字符串值  
> 不同点：  
> indexOf()方法中参数fromindex：可选，开始检索的地方，从哪开始查找，合法取值：0—stringObject.length，当然不合法取个负值也是不会报错的只是会被视作0或者说空  
> lastIndexOf()方法中的参数fromindex：可选，建议不填，奇妙的第二个参数，能不用就不用

	var str1 = 'Java Script';
	console.log(str1.indexOf('a')); // 1
	console.log(str1.lastIndexOf('a')); // 3
	console.log(str1.indexOf('a', 1)); // 1
	console.log(str1.lastIndexOf('a', 1)); // 1
	console.log(str1.indexOf('a', 2)); // 3
	console.log(str1.lastIndexOf('a', 1)); // 1

> 注：调用此方法，要查询的指定字符若存在于目标字符串中，则返回对应的下标，若不存在，则返回 -1 ，可根据此特性，来判断某一字符串中是否含有某一指定字符，如：   

	// 判断str1中是否含有字符'a'
	var str1 = 'Java Script';
	function checkStr (targetStr) {
	  if (str1.indexOf(targetStr) > -1) {
	    alert('str1中含有字符'+targetStr);
	  } else {
	    alert('str1中不含有字符'+targetStr);
	  }
	}
	// 检查是否含有字符'a'
	checkStr('a');
	// 检查是否含有字符'abc'
	checkStr('abc');

### 9. 面试题
	
	function getSuffix(file){
      return file.slice(file.lastIndexOf(".") + 1,file.length); 
	}

	console.log(getSuffix('test.min.js')); // js