
#Set��Map���ݽ⹹
##Set
- �����÷�
	ES6�ṩ�����������飬����Ա��ֵ��Ψһ��,�ж��Ƿ��ظ���===
	Set������һ�����캯������������Set���ݽṹ��
	```
	var s = new Set();
	[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));
	for (let i of s) {
	  console.log(i);
	}
	// 2 3 5 4
	```
	Set�������Խ���һ�����飨����������Ķ�����Ϊ������������ʼ����
	ʵ������ȥ��
	```
	var arr =  [...new Set([2,2,2,3,4,5])];
	var arr2 =Array.from(new Set([2,2,2,3,4,5]));
	```
	�������ǲ���ȵ�
	```
	set.Add({});
	set.Add({});
	set.size = 2
	```
- Set��ʵ���ͷ��� 
	��������
	```
	var set = new Set([1,2,3]);
		add(value)	 //Set
		delete(value)   //true
		has(value)	 //true
		clear()  
	```
	��������
	```
	let set = new Set(['red', 'green', 'blue']);
		keys()   //for(let x of set.keys())
		values()   //for(let x of set.values())
		entries()   //for(let x of set.values())
		forEach()	//   set.forEach((value,key)=>console.log(value);)	
	```  
	����Ӧ��...
	```
	let a = new Set([1, 2, 3]);
	let b = new Set([4, 3, 2]);
	// ����
	let union = new Set([...a, ...b]);
	// Set {1, 2, 3, 4}
	// ����
	let intersect = new Set([...a].filter(x => b.has(x)));
	// set {2, 3}
	// �
	let difference = new Set([...a].filter(x => !b.has(x)));
	// Set {1}
	```
 ##Map
- Map�ĽṹĿ�ĺ��÷�	 
	JavaScript�Ķ���Object�����������Ǽ�ֵ�Եļ��ϣ�Hash�ṹ�������Ǵ�ͳ��ֻ�����ַ������������������ʹ�ô����˺ܴ�����ơ�
	Ϊ�˽��������⣬ES6�ṩ��Map���ݽṹ���������ڶ���Ҳ�Ǽ�ֵ�Եļ��ϣ����ǡ������ķ�Χ�������ַ������������͵�ֵ���������󣩶����Ե�������Ҳ����˵��Object�ṹ�ṩ�ˡ��ַ�����ֵ���Ķ�Ӧ��Map�ṹ�ṩ�ˡ�ֵ��ֵ���Ķ�Ӧ����һ�ָ����Ƶ�Hash�ṹʵ�֡��������Ҫ����ֵ�ԡ������ݽṹ��Map��Object�����ʡ�
	```
	//����
	var m = new Map();
	var o = {p: 'Hello World'};
	m.set(o, 'content')
	m.get(o) // "content"
	m.has(o) // true
	m.delete(o) // true
	m.has(o) // false

	//����
	var map = new Map([
	  ['name', '����'],
	  ['title', 'Author']
	]);
	map.size // 2
	map.has('name') // true
	map.get('name') // "����"
	map.has('title') // true
	map.get('title') // "Author"
	```
	�����ͬһ������θ�ֵ�������ֵ������ǰ���ֵ��
	let map = new Map();
	map
	.set(1, 'aaa')
	.set(1, 'bbb');
	map.get(1) // "bbb"
	���Map�ļ���һ�������͵�ֵ�����֡��ַ���������ֵ������ֻҪ����ֵ�ϸ���ȣ�Map������Ϊһ����������0��-0�����⣬��ȻNaN���ϸ������������Map������Ϊͬһ������
	let map = new Map();
	map.set(NaN, 123);
	map.get(NaN) // 123
	map.set(-0, 123);
	map.get(+0) // 123
- ʵ�������ԺͲ�������
	���ԡ���������
	```
	map.size=2
	set(key, value) //����ж�Ӧ��key,����
	get(key)  //���û�з���underfind
	has(key)  //true
	clear()
	```
	�������� ͬSet
	