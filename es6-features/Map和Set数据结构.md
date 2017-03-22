
#Set和Map数据解构
##Set
- 基本用法
	ES6提供，类似于数组，但成员的值是唯一的,判断是否重复用===
	Set本身是一个构造函数，用来生成Set数据结构。
	```
	var s = new Set();
	[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));
	for (let i of s) {
	  console.log(i);
	}
	// 2 3 5 4
	```
	Set函数可以接受一个数组（或类似数组的对象）作为参数，用来初始化。
	实现数组去重
	```
	var arr =  [...new Set([2,2,2,3,4,5])];
	var arr2 =Array.from(new Set([2,2,2,3,4,5]));
	```
	对象总是不相等的
	```
	set.Add({});
	set.Add({});
	set.size = 2
	```
- Set的实例和方法 
	操作方法
	```
	var set = new Set([1,2,3]);
		add(value)	 //Set
		delete(value)   //true
		has(value)	 //true
		clear()  
	```
	遍历方法
	```
	let set = new Set(['red', 'green', 'blue']);
		keys()   //for(let x of set.keys())
		values()   //for(let x of set.values())
		entries()   //for(let x of set.values())
		forEach()	//   set.forEach((value,key)=>console.log(value);)	
	```  
	遍历应用...
	```
	let a = new Set([1, 2, 3]);
	let b = new Set([4, 3, 2]);
	// 并集
	let union = new Set([...a, ...b]);
	// Set {1, 2, 3, 4}
	// 交集
	let intersect = new Set([...a].filter(x => b.has(x)));
	// set {2, 3}
	// 差集
	let difference = new Set([...a].filter(x => !b.has(x)));
	// Set {1}
	```
 ##Map
- Map的结构目的和用法	 
	JavaScript的对象（Object），本质上是键值对的集合（Hash结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。
	为了解决这个问题，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的对应，是一种更完善的Hash结构实现。如果你需要“键值对”的数据结构，Map比Object更合适。
	```
	//对象
	var m = new Map();
	var o = {p: 'Hello World'};
	m.set(o, 'content')
	m.get(o) // "content"
	m.has(o) // true
	m.delete(o) // true
	m.has(o) // false

	//数组
	var map = new Map([
	  ['name', '张三'],
	  ['title', 'Author']
	]);
	map.size // 2
	map.has('name') // true
	map.get('name') // "张三"
	map.has('title') // true
	map.get('title') // "Author"
	```
	如果对同一个键多次赋值，后面的值将覆盖前面的值。
	let map = new Map();
	map
	.set(1, 'aaa')
	.set(1, 'bbb');
	map.get(1) // "bbb"
	如果Map的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map将其视为一个键，包括0和-0。另外，虽然NaN不严格相等于自身，但Map将其视为同一个键。
	let map = new Map();
	map.set(NaN, 123);
	map.get(NaN) // 123
	map.set(-0, 123);
	map.get(+0) // 123
- 实例的属性和操作方法
	属性、操作方法
	```
	map.size=2
	set(key, value) //如果有对应的key,覆盖
	get(key)  //如果没有返回underfind
	has(key)  //true
	clear()
	```
	遍历方法 同Set
	