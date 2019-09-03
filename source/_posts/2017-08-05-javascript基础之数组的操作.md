---
title: javascript基础
date: 2017-08-05 19:13:13
tags: js
---
#### javascript基础
##### 一. js对数组的操作
###### 1. 数组的基本操作之 —— 增/删 (push(), pop(), unshift(), shift()) 操作
* 增： push(): 向数组后添加内容，可接受任意数量的参数，会对原数组产生影响,返回修改后数组的长度  

<!--more-->

* 删： pop(): 删除数组最后一个元素，不接收参数，会对原数组产生影响，返回被删除的元素值，如果数组为空，则返回 undefined  

* 增：unshift(): 向数组开头添加内容，可接受任意参数量的参数，会对原数组产生影响，返回修改后的数组的长度  

* 删： shift(): 删除数组第一个元素，不接收参数，会对原数组产生影响，返回被删除的元素值，如果数组为空，则返回 undefined

		var pushNumber = [1, 2, 3, 4, 5, 6];
		var pushResult1 = pushNumber.push(7, '帅气');
		console.log(pushResult1); // 8
		console.log(pushNumber); // [1, 2, 3, 4, 5, 6, 7, '帅气']
		var pushResult2 = pushNumber.unshift([7, '帅气']); // 注意添加的是一个数组，为一项
		console.log(pushResult2); // 9 
		console.log(pushNumber); // [[7, '帅气'], 1, 2, 3, 4, 5, 6, 7, '帅气'];
		
		var popNumber = [1, 2, 3, 4, 5, 6];
		var popResult1 = popNumber.pop();
		console.log(popResult1); // 6
		console.log(popNumber); // [1, 2, 3, 4, 5]
		var popResult2 = popNumber.shift();
		console.log(popResult2); // 1
		console.log(popNumber); // [2, 3, 4, 5]

###### 2. 数组的基本操作之 —— 通过指定索引对指定位置添加或者删除相应位置的元素(splice(), slice())
> 说明：  
> splice(): 从数组中添加或删除元素，返回被删除的项目，如果是添加，则返回一个空数组（注：会对原数组进行更改，获取的是新的数组），使用如下：  
> a. 使用splice()方法添加元素：splice(index, 0, data1, data2, ...)，添加时，splice()方法返回一个空数组;  
> 参数解析： index(插入的元素的起始位置，值为1，就是第一位开始，并非索引值), 0(删除的个数), data1/data2/...(添加的元素)；  

	// 向数组第一项之后插入新值
	var spliceAddArr = [1, 2, 3, 4, 5, 6];
	var spliceAddResult = spliceAddArr.splice(1, 0, 'a', 'b', 'c');
	console.log('spliceAddArr: ', spliceAddArr, '; spliceAddResult: ', spliceAddResult); // spliceAddArr: [1, "a", "b", "c", 2, 3, 4, 5, 6] ; spliceAddResult: []

> b.1 使用splice()方法删除元素： splice(index, 3)，删除时，splice()方法返回被删除的元素的数组集合（注：会对原数组进行更改，原数组变更为删除之后的新数组）  
> 参数解析： index(删除元素的起始位置，值为1，就是从第一位开始，并非索引值)，3(删除的个数)  
> b.2 使用splice()方法删除元素的同时添加元素： splice(index, 3, data1, data2, ...)  
> 参数解析： index(删除元素的起始位置，值为1，就是从第一位开始，并非索引值)，3(删除的个数)，data1/data2/...（添加的元素）；  

	// 使用splice()删除元素，返回被删除元素
	var spliceDelArr = [1, 2, 3, 4, 5, 6];
	var spliceDelResult = spliceDelArr.splice(1, 3);
	console.log('spliceDelArr: ', spliceDelArr, '; spliceDelResult: ', spliceDelResult); // spliceDelArr: [1, 5, 6] ; spliceDelResult: [2, 3, 4]
	
	// 使用splice()删除元素的同时添加元素
	var spliceAddArr = [1, 2, 3, 4, 5, 6];
	var spliceAddResult = spliceAddArr.splice(1, 3, 'a', 'b', 'c');
	console.log('spliceAddArr: ', spliceAddArr, '; spliceAddResult: ', spliceAddResult); // spliceDelArr: [1, "a", "b", "c", 5, 6] ; spliceDelResult: [2, 3, 4]

> 注： 使用splice(),不管是用来添加还是删除元素，第一个参数都可以接受一个负数作为参数，与正数相比：  
> 相同之处：  
> 使用splice()添加元素，都是从起始位置的之后添加，即如果是正3，即从起始位置往后数3位添加，即从第三第四位中间添加元素，若为负数-3，即从后往前数三位的后一位，即从后往前数第三位到第四位中间添加，如果第一个参数，大于数组的实际长度，则从数组的末尾开始添加。  
> 不同之处：  
> 使用splice()删除元素，如果是正数3，如： splice(3, 3),即从开始位置往后数三位，从第四位开始删除三项，实际删除的是第四，第五，第六项，而当为负数-3时，如： splice(-3, 3)，即从后往前数三位，从倒数第三位开始删除，并不是倒数第四位，实际删除的项为倒数第三项，倒数第二项，倒数第一项三项。  
> 如果删除的项数大于起始位置到数组的结束位置的项数，则默认到数组结束位置停止。  
	
	// splice()添加，index为正数时
	var arr1 = [1, 2, 3, 4, 5];
	var arrRes1 = arr1.splice(3, 0, 'a', 'b', 'c');
	console.log('arr1: ', arr1, '; arrRes1:', arrRes1); // arr1: [1, 2, 3, 'a', 'b', 'c', 4, 5] ； arrRes1： []

	// splice()添加，index为负数时
	var arr2 = [1, 2, 3, 4, 5];
	var arrRes2 = arr2.splice(-3, 0, 'a', 'b', 'c');
	console.log('arr2: ', arr2, '; arrRes2:', arrRes2); // arr2: [1, 2, "a", "b", "c", 3, 4, 5] ; arrRes2: []

	// splice()删除，index为正数时
	var arr3 = [1, 2, 3, 4, 5, 6, 7];
	var arrRes3 = arr3.splice(3, 3, 'a', 'b', 'c');
	console.log('arr3: ', arr3, '; arrRes3:', arrRes3); // arr3: [1, 2, 3, "a", "b", "c", 7] ； arrRes3： [4, 5, 6]

	// splice()删除，index为正数时
	var arr4 = [1, 2, 3, 4, 5, 6, 7];
	var arrRes4 = arr4.splice(-3, 3, 'a', 'b', 'c');
	console.log('arr4: ', arr4, '; arrRes4:', arrRes4); // arr3: [1, 2, 3, 4, "a", "b", "c"] ； arrRes3： [5, 6, 7]

> slice()使用说明：用于截取数组，返回截取到的新的数组，此方法可以作用于字符串属性（注：不会影响原数组）;  
> 此方法接受两个参数，slice(start, end)：  
> start: 必传，截取开始位置的下标,  
> end: 可传，截取结束的位置下标索引，不包括当前下标对应的元素，不传为数组结束。  

	var sliceArr = [1, 2, 3, 4, 5, 6, 7];
	var sliceRes0 = sliceArr.slice(2); // 实际截取的为：第三项到数组结束
	var sliceRes1 = sliceArr.slice(2, 4); // 实际截取的为：第三项，第四项
	var sliceRes2 = sliceArr.slice(-2); // 实际截取的为：第三项，第四项
	var sliceRes3 = sliceArr.slice(-9); // 从后往前第九位，数组长度不够，从第一项开始截取到数组结束
	var sliceRes4 = sliceArr.slice(-4, -2); // 实际截取的为从倒数第四项到倒数第二项：即为倒数第四项，倒数第五项两项
	var sliceRes5 = sliceArr.slice(-4, -6); // 无匹配项，返回一个空数组
	var sliceRes6 = sliceArr.slice(1, -2); // 从索引为1的位置开始，到倒数第二项，不包括倒数第二项
	var sliceRes7 = sliceArr.slice(9); // 无匹配项，返回一个空数组
	console.log('sliceArr: ', sliceArr, '； sliceRes0: ', sliceRes0); // sliceArr1: [1, 2, 3, 4, 5, 6, 7] ; sliceRes0: [3, 4, 5, 6, 7]
	console.log('sliceRes1: ', sliceRes1); // sliceRes1: [3, 4]
	console.log('sliceRes2: ', sliceRes2); // sliceRes2: [6, 7]
	console.log('sliceRes3: ', sliceRes3); // sliceRes3: [1, 2, 3, 4, 5, 6, 7]
	console.log('sliceRes4: ', sliceRes4); // sliceRes4: [4, 5]
	console.log('sliceRes5: ', sliceRes5); // sliceRes5: []
	console.log('sliceRes6: ', sliceRes6); // sliceRes6: [2, 3, 4, 5]
	console.log('sliceRes7: ', sliceRes7); // sliceRes7: []

###### 3. 数组的基本操作之 —— 对数组的每一项进行操作(map(), forEach())
> 相同点:  
> 1. 都是循环遍历数组中每一项  
> 2. map和forEach每次执行匿名函数时都支持3个参数，参数分别是item(当前项)，index(索引值)，arr(原数组)  
> 3. 匿名函数中的this都指向 window  
> 4. 只能遍历数组  
> 5. 都不会改变原数组  
> 
> 区别：  
> map方法：  
> 1. map方法返回一个新的数组，数组中的元素为原始数组调用函数处理之后的值  
> 2. map方法不会对空数组进行检测(即不会执行匿名函数，若为空数组，则调用map方法后也返回一个空数组)，map方法不会改变原始数组  
> 3. 浏览器支持： chrome / Safari 1.5+ / opera / IE9+  
> forEach方法：  
> 1. forEach()方法用来调用数组的每个元素，将元素传给回调函数  
> 2. forEach()方法不会对空数组进行检测，对于空数组是不会调用匿名函数的，注意，无论数组是否为空，调用forEach方法，返回值均为 undefined

	// map方法
	var mapArr = [1, 2, 3, 4, 5];
    var mapResult = mapArr.map(function(item, index, arr){
      console.log(this); // window
      console.log('原始数组mapArr: ', arr); // 原始数组mapArr: [1, 2, 3, 4, 5]
      return item+1;
    })
    console.log('mapArr:', mapArr, '; mapResult: ', mapResult); // mapArr: [1, 2, 3, 4, 5] ; mapResult: [2, 3, 4, 5, 6]

	// 若为空数组，调用map方法，返回一个空数组，匿名函数不会执行
	var empArr = [];
	var mapEmpResult = empArr.map(function(){item, index, arr}{
	  // 空数组调用map方法，函数体不会执行
      console.log(this);
      console.log('原始数组mapArr: ', arr);
      return item+1;
	})；
	console.log('empArr:', empArr, '; mapEmpResult: ', mapEmpResult); // empArr: [] ; mapEmpResult: []

	// forEach方法
	var forEachArr = [1, 2, 3, 4, 5];
	var forEachResult = forEachArr.forEach(function(item, index, arr){ // 调用forEach()方法，返回值均为 undefined
	  console.log(this); // window
	  console.log('原始数组forEachArr：', arr); // 原始数组forEachArr： [1, 2, 3, 4, 5]
	  return item+1;
	})
    console.log('forEachArr:', forEachArr, '; forEachResult: ', forEachResult); // forEachArr: [1, 2, 3, 4, 5] ; forEachResult: undefined

###### 4. 数组的基本操作之 —— 对数组的每一项进行操作（ filter() ）
> 说明： 遍历每一项数组，接收一个匿名函数作为参数，函数接收三个参数，参数item(遍历得到的每一项数组值)，index（索引值），arr(数组本身)， 返回一个结果为true的item组成的新数组，不会影响原数组  
	
	// 需求：获取成绩合格的学生名单
	var students = [
      {name: '张三', age: 18, scroe: 89},
      {name: '李四', age: 17, scroe: 71},
      {name: '王五', age: 18, scroe: 56},
      {name: '赵六', age: 19, scroe: 94},
      {name: '小明', age: 18, scroe: 48},
      {name: '小红', age: 19, scroe: 60},
    ];
	var studentsResult = students.filter(function(item){
      if (item.scroe >= 60) {
        return {
          name: item.name,
          age: item.age,
          score: item.scroe
        }
      }
    });
    console.log(studentsResult); // 输出如下结果
	// [{name: "张三", age: 18, scroe: 89},
	// {name: "李四", age: 17, scroe: 71},
	// {name: "赵六", age: 19, scroe: 94},
	// {name: "小红", age: 19, scroe: 60}]

###### 5. 数组的基本操作之 —— 对数组的迭代方法(every(), some())方法
> 说明：every()和some()都是对js中数组的迭代方法，区别如下：  
> 相同点：  
> 1. 均接收一个匿名函数作为参数，函数接收三个参数，参数item(每一项),index（索引值），arr(原始数组)  
> 2. 均对数组的每一项运行指定的匿名函数
> 
> 不同点:  
> every(): 对数组的每一项运行给定的函数，如果该函数每一项都返回 true, 则返回 true，若存在某项返回false，则终止函数迭代，返回false; (可用来判断是不是所有，类似于逻辑 &&)  
> some(): 对数组的每一项运行给定的函数，如果该函数存在某一项返回 true, 函数终止，并返回 true; (可用来判断有没有，类似于逻辑 ||)

	// 判断数组是是否全为基数
	// 使用every()方法实现
	var everyArr1 = [1, 3, 5, 7, 9];
	var everyArr2 = [1, 3, 5, 6, 7, 9];
	var everyResult1 = everyArr1.every(function(item, index, arr){
      if (!(item % 2)) {
        console.log('第'+(index+1)+'项为偶数,值为：'+item);
      }
	  return item % 2;
	});
	var everyResult2 = everyArr2.every(function(item, index, arr){
      if (!(item % 2)) {
        console.log('第'+(index+1)+'项为偶数,值为：'+item); // 第4项为偶数,值为：6
      }
	  return item % 2;
	});
	console.log(everyResult1?'everyArr1的每一项均为基数':'everyArr1的存在某一项不为基数'); // everyArr的每一项均为基数
	console.log(everyResult2?'everyArr2的每一项均为基数':'everyArr2的存在某一项不为基数'); // everyArr2的存在某一项不为基数
	
	// 使用some()方法实现
	var someArr1 = [1, 3, 5, 7, 9];
	var someArr2 = [1, 3, 5, 6, 7, 9];
	var someResult1 = someArr1.some(function (item, index, arr) {
	  if (!(item % 2)) {
	    console.log('第' + (index + 1) + '项为偶数,值为：' + item);
	  };
	  return !(item % 2);
	});
	var someResult2 = someArr2.some(function (item, index, arr) {
	  if (!(item % 2)) {
	    console.log('第' + (index + 1) + '项为偶数,值为：' + item); // 第4项为偶数,值为：6
	  };
	  return !(item % 2);
	});
	console.log(!someResult1 ? 'someArr1的每一项均为基数' : 'someArr1的存在某一项不为基数'); // someArr1的每一项均为基数
	console.log(!someResult2 ? 'someArr2的每一项均为基数' : 'someArr2的存在某一项不为基数'); // someArr2的存在某一项不为基数