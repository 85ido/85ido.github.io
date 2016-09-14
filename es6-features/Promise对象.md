# Promise����
## Promise�ĺ���
Promise���첽��̵�һ�ֽ���������ȴ�ͳ�Ľ�����������ص��������¼�����������͸�ǿ��
��νPromise����˵����һ�����������汣����ĳ��δ���Ż�������¼���ͨ����һ���첽�������Ľ����
���﷨��˵��Promise��һ�����󣬴������Ի�ȡ�첽��������Ϣ��Promise�ṩͳһ��API�������첽������������ͬ���ķ������д���
Promise���������������ص㡣
��1�������״̬�������Ӱ�졣
Promise�������һ���첽������������״̬��Pending�������У���Resolved������ɣ��ֳ�Fulfilled����Rejected����ʧ�ܣ���ֻ���첽�����Ľ�������Ծ�����ǰ����һ��״̬���κ������������޷��ı����״̬����Ҳ��Promise������ֵ�����������Ӣ����˼���ǡ���ŵ������ʾ�����ֶ��޷��ı䡣

��2��һ��״̬�ı䣬�Ͳ����ٱ䣬�κ�ʱ�򶼿��Եõ���������
Promise�����״̬�ı䣬ֻ�����ֿ��ܣ���Pending��ΪResolved�ʹ�Pending��ΪRejected��ֻҪ���������������״̬�������ˣ������ٱ��ˣ���һֱ����������������ı��Ѿ������ˣ����ٶ�Promise������ӻص�������Ҳ�������õ��������������¼���Event����ȫ��ͬ���¼����ص��ǣ����������������ȥ�������ǵò�������ġ�
����Promise���󣬾Ϳ��Խ��첽������ͬ�����������̱������������˲��Ƕ�׵Ļص����������⣬Promise�����ṩͳһ�Ľӿڣ�ʹ�ÿ����첽�����������ס�

PromiseҲ��һЩȱ�㡣
���ȣ��޷�ȡ��Promise��һ���½����ͻ�����ִ�У��޷���;ȡ����
��Σ���������ûص�������Promise�ڲ��׳��Ĵ��󣬲��ᷴӦ���ⲿ��
������������Pending״̬ʱ���޷���֪Ŀǰ��չ����һ���׶Σ��ոտ�ʼ���Ǽ�����ɣ���

���ĳЩ�¼����ϵط���������һ����˵��ʹ��streamģʽ�ǱȲ���Promise���õ�ѡ��
## �����÷�
ES6�涨��Promise������һ�����캯������������Promiseʵ����
```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* �첽�����ɹ� */){
    resolve(value);
  } else {
    reject(error);
  }
});
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```
Promise���캯������һ��������Ϊ�������ú��������������ֱ���resolve��reject��������������������JavaScript�����ṩ�������Լ�����
resolve�����������ǣ���Promise�����״̬�ӡ�δ��ɡ���Ϊ���ɹ���������Pending��ΪResolved�������첽�����ɹ�ʱ���ã������첽�����Ľ������Ϊ�������ݳ�ȥ��
reject�����������ǣ���Promise�����״̬�ӡ�δ��ɡ���Ϊ��ʧ�ܡ�������Pending��ΪRejected�������첽����ʧ��ʱ���ã������첽���������Ĵ�����Ϊ�������ݳ�ȥ��
Promiseʵ�������Ժ󣬿�����then�����ֱ�ָ��Resolved״̬��Reject״̬�Ļص�������
 then�������Խ��������ص�������Ϊ��������һ���ص�������Promise�����״̬��ΪResolvedʱ���ã��ڶ����ص�������Promise�����״̬��ΪRejectʱ���á����У��ڶ��������ǿ�ѡ�ģ���һ��Ҫ�ṩ������������������Promise���󴫳���ֵ��Ϊ������

������һ��Promise����ļ����ӡ�
```javascript
function timeout(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms, 'done');
  });
}
timeout(100).then((value) => {
  console.log(value);
});
```
��������У�timeout��������һ��Promiseʵ������ʾһ��ʱ���Ժ�Żᷢ���Ľ��������ָ����ʱ�䣨ms�������Ժ�Promiseʵ����״̬��ΪResolved���ͻᴥ��then�����󶨵Ļص�������

Promise�½���ͻ�����ִ�С�
```javascript
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});
promise.then(function() {
  console.log('Resolved.');
});
console.log('Hi!');

// Promise
// Hi!
// Resolved
```
��������У�Promise�½�������ִ�У���������������ǡ�Promise����Ȼ��then����ָ���Ļص����������ڵ�ǰ�ű�����ͬ������ִ����Ż�ִ�У����ԡ�Resolved����������

�������첽����ͼƬ�����ӡ�
```javascript
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    var image = new Image();

    image.onload = function() {
      resolve(image);
    };

    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };

    image.src = url;
  });
}
```
��������У�ʹ��Promise��װ��һ��ͼƬ���ص��첽������������سɹ����͵���resolve����������͵���reject������

������һ����Promise����ʵ�ֵ�Ajax���������ӡ�
```javascript
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('������', error);
});
```
��������У�getJSON�Ƕ�XMLHttpRequest����ķ�װ�����ڷ���һ�����JSON���ݵ�HTTP���󣬲��ҷ���һ��Promise������Ҫע����ǣ���getJSON�ڲ���resolve������reject��������ʱ�������в�����

�������resolve������reject����ʱ���в�������ô���ǵĲ����ᱻ���ݸ��ص�������reject�����Ĳ���ͨ����Error�����ʵ������ʾ�׳��Ĵ���resolve�����Ĳ�������������ֵ���⣬����������һ��Promiseʵ������ʾ�첽�����Ľ���п�����һ��ֵ��Ҳ�п�������һ���첽����������������������
```javascript
var p1 = new Promise(function (resolve, reject) {
  // ...
});

var p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})
```
��������У�p1��p2����Promise��ʵ��������p2��resolve������p1��Ϊ��������һ���첽�����Ľ���Ƿ�����һ���첽������

ע�⣬��ʱp1��״̬�ͻᴫ�ݸ�p2��Ҳ����˵��p1��״̬������p2��״̬�����p1��״̬��Pending����ôp2�Ļص������ͻ�ȴ�p1��״̬�ı䣻���p1��״̬�Ѿ���Resolved����Rejected����ôp2�Ļص�������������ִ�С�
```javascript
var p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error('fail')), 3000)
})

var p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000)
})

p2
  .then(result => console.log(result))
  .catch(error => console.log(error))
// Error: fail
```
��������У�p1��һ��Promise��3��֮���Ϊrejected��p2��״̬��1��֮��ı䣬resolve�������ص���p1����ʱ������p2���ص�����һ��Promise�����Ժ����then��䶼�����Ժ��ߣ�p1�����ֹ���2�룬p1��Ϊrejected�����´���catch����ָ���Ļص�������
## 3��Promise.prototype.then()
Promiseʵ������then������Ҳ����˵��then�����Ƕ�����ԭ�Ͷ���Promise.prototype�ϵġ�����������ΪPromiseʵ�����״̬�ı�ʱ�Ļص�������
ǰ��˵����then�����ĵ�һ��������Resolved״̬�Ļص��������ڶ�����������ѡ����Rejected״̬�Ļص�������

then�������ص���һ���µ�Promiseʵ����ע�⣬����ԭ���Ǹ�Promiseʵ��������˿��Բ�����ʽд������then���������ٵ�����һ��then������
```javascript
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```
����Ĵ���ʹ��then����������ָ���������ص���������һ���ص���������Ժ󣬻Ὣ���ؽ����Ϊ����������ڶ����ص�������
������ü�ͷ����������Ĵ������д�ø���ࡣ
```javascript
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("Resolved: ", comments),
  err => console.log("Rejected: ", err)
);
```
## 4��Promise.prototype.catch()
Promise.prototype.catch������.then(null, rejection)�ı���������ָ����������ʱ�Ļص�������
```javascript
getJSON("/posts.json").then(function(posts) {
  // ...
}).catch(function(error) {
  // ���� getJSON �� ǰһ���ص���������ʱ�����Ĵ���
  console.log('��������', error);
});
```
��������У�getJSON��������һ��Promise��������ö���״̬��ΪResolved��������then����ָ���Ļص�����������첽�����׳�����״̬�ͻ��ΪRejected���ͻ����catch����ָ���Ļص���������������������⣬then����ָ���Ļص�����������������׳�����Ҳ�ᱻcatch��������
```javascript
p.then((val) => console.log("fulfilled:", val))
  .catch((err) => console.log("rejected:", err));
```
// ��ͬ��
```javascript
p.then((val) => console.log("fulfilled:", val))
  .then(null, (err) => console.log("rejected:", err));
```
������һ�����ӡ�
```javascript
var promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});
// Error: test
```
��������У�promise�׳�һ�����󣬾ͱ�catch����ָ���Ļص���������ע�⣬�����д������������д���ǵȼ۵ġ�

// д��һ
```javascript
var promise = new Promise(function(resolve, reject) {
  try {
    throw new Error('test');
  } catch(e) {
    reject(e);
  }
});
promise.catch(function(error) {
  console.log(error);
});
```
// д����
```javascript
var promise = new Promise(function(resolve, reject) {
  reject(new Error('test'));
});
promise.catch(function(error) {
  console.log(error);
});
```
�Ƚ���������д�������Է���reject���������ã���ͬ���׳�����

���Promise״̬�Ѿ����Resolved�����׳���������Ч�ġ�
```javascript
var promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });
// ok
```
��������У�Promise��resolve�����棬���׳����󣬲��ᱻ���񣬵���û���׳���

Promise����Ĵ�����С�ð�ݡ����ʣ���һֱ��󴫵ݣ�ֱ��������Ϊֹ��Ҳ����˵���������ǻᱻ��һ��catch��䲶��
```javascript
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // ����ǰ������Promise�����Ĵ���
});
```
��������У�һ��������Promise����һ����getJSON������������then����������֮���κ�һ���׳��Ĵ��󣬶��ᱻ���һ��catch����

һ����˵����Ҫ��then�������涨��Reject״̬�Ļص���������then�ĵڶ���������������ʹ��catch������
```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
��������У��ڶ���д��Ҫ���ڵ�һ��д���������ǵڶ���д�����Բ���ǰ��then����ִ���еĴ���Ҳ���ӽ�ͬ����д����try/catch������ˣ���������ʹ��catch����������ʹ��then�����ĵڶ���������

����ͳ��try/catch����鲻ͬ���ǣ����û��ʹ��catch����ָ��������Ļص�������Promise�����׳��Ĵ��󲻻ᴫ�ݵ������룬���������κη�Ӧ��
```javascript
var someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // ����һ�лᱨ����Ϊxû������
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  console.log('everything is great');
});
```
��������У�someAsyncThing����������Promise����ᱨ����������û��ָ��catch������������󲻻ᱻ����Ҳ���ᴫ�ݵ������룬�������к�û���κ������ע�⣬Chrome����������������涨�������׳�����ReferenceError: x is not defined����
```javascript
var promise = new Promise(function(resolve, reject) {
  resolve("ok");
  setTimeout(function() { throw new Error('test') }, 0)
});
promise.then(function(value) { console.log(value) });
// ok
// Uncaught Error: test
```
��������У�Promiseָ������һ�֡��¼�ѭ�������׳����󣬽������û��ָ��ʹ��try...catch��䣬��ð�ݵ�����㣬����δ����Ĵ�����Ϊ��ʱ��Promise�ĺ������Ѿ����н����ˣ����������������Promise���������׳��ġ�

Node.js��һ��unhandledRejection�¼���ר�ż���δ�����reject����
```javascript
process.on('unhandledRejection', function (err, p) {
  console.error(err.stack)
});
```
��������У�unhandledRejection�¼��ļ���������������������һ���Ǵ�����󣬵ڶ����Ǳ����Promiseʵ���������������˽ⷢ������Ļ�����Ϣ����

��Ҫע����ǣ�catch�������صĻ���һ��Promise������˺��滹���Խ��ŵ���then������
```javascript
var someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // ����һ�лᱨ����Ϊxû������
    resolve(x + 2);
  });
};

someAsyncThing()
.catch(function(error) {
  console.log('oh no', error);
})
.then(function() {
  console.log('carry on');
});
// oh no [ReferenceError: x is not defined]
// carry on
```
�������������catch����ָ���Ļص���������������к����Ǹ�then����ָ���Ļص����������û�б����������catch������
```javascript
Promise.resolve()
.catch(function(error) {
  console.log('oh no', error);
})
.then(function() {
  console.log('carry on');
});
// carry on
```
����Ĵ�����Ϊû�б���������catch������ֱ��ִ�к����then��������ʱ��Ҫ��then�������汨������ǰ���catch�޹��ˡ�

catch����֮�У��������׳�����
```javascript
var someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // ����һ�лᱨ����Ϊxû������
    resolve(x + 2);
  });
};

someAsyncThing().then(function() {
  return someOtherAsyncThing();
}).catch(function(error) {
  console.log('oh no', error);
  // ����һ�лᱨ����Ϊyû������
  y + 2;
}).then(function() {
  console.log('carry on');
});
// oh no [ReferenceError: x is not defined]
```
��������У�catch�����׳�һ��������Ϊ����û�б��catch�����ˣ�����������󲻻ᱻ����Ҳ���ᴫ�ݵ���㡣�����дһ�£�����Ͳ�һ���ˡ�
```javascript
someAsyncThing().then(function() {
  return someOtherAsyncThing();
}).catch(function(error) {
  console.log('oh no', error);
  // ����һ�лᱨ����Ϊyû������
  y + 2;
}).catch(function(error) {
  console.log('carry on', error);
});
// oh no [ReferenceError: x is not defined]
// carry on [ReferenceError: y is not defined]
```
��������У��ڶ���catch������������ǰһ��catch�����׳��Ĵ���
## 5��Promise.all()
Promise.all�������ڽ����Promiseʵ������װ��һ���µ�Promiseʵ����
```javascript
var p = Promise.all([p1, p2, p3]);
```
��������У�Promise.all��������һ��������Ϊ������p1��p2��p3����Promise�����ʵ����������ǣ��ͻ��ȵ������潲����Promise.resolve������������תΪPromiseʵ�����ٽ�һ��������Promise.all�����Ĳ������Բ������飬���������Iterator�ӿڣ��ҷ��ص�ÿ����Ա����Promiseʵ������

p��״̬��p1��p2��p3�������ֳ����������

��1��ֻ��p1��p2��p3��״̬�����fulfilled��p��״̬�Ż���fulfilled����ʱp1��p2��p3�ķ���ֵ���һ�����飬���ݸ�p�Ļص�������

��2��ֻҪp1��p2��p3֮����һ����rejected��p��״̬�ͱ��rejected����ʱ��һ����reject��ʵ���ķ���ֵ���ᴫ�ݸ�p�Ļص�������

������һ����������ӡ�
```javascript
// ����һ��Promise���������
var promises = [2, 3, 5, 7, 11, 13].map(function (id) {
  return getJSON("/post/" + id + ".json");
});

Promise.all(promises).then(function (posts) {
  // ...
}).catch(function(reason){
  // ...
});
```
��������У�promises�ǰ���6��Promiseʵ�������飬ֻ����6��ʵ����״̬�����fulfilled������������һ����Ϊrejected���Ż����Promise.all��������Ļص�������

��������һ�����ӡ�
```javascript
const databasePromise = connectDatabase();

const booksPromise = databaseProimse
  .then(findAllBooks);

const userPromise = databasePromise
  .then(getCurrentUser);

Promise.all([
  booksPromise,
  userPromise
])
.then(([books, user]) => pickTopRecommentations(books, user));
```
��������У�booksPromise��userPromise�������첽������ֻ�еȵ����ǵĽ���������ˣ��Żᴥ��pickTopRecommentations����ص�������
##6��Promise.race()
Promise.race����ͬ���ǽ����Promiseʵ������װ��һ���µ�Promiseʵ����
```javascript
var p = Promise.race([p1,p2,p3]);
```
��������У�ֻҪp1��p2��p3֮����һ��ʵ�����ȸı�״̬��p��״̬�͸��Ÿı䡣�Ǹ����ȸı��Promiseʵ���ķ���ֵ���ʹ��ݸ�p�Ļص�������

Promise.race�����Ĳ�����Promise.all����һ�����������Promiseʵ�����ͻ��ȵ������潲����Promise.resolve������������תΪPromiseʵ�����ٽ�һ������

������һ�����ӣ����ָ��ʱ����û�л�ý�����ͽ�Promise��״̬��Ϊreject�������Ϊresolve��
```javascript
var p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
])
p.then(response => console.log(response))
p.catch(error => console.log(error))
```
��������У����5��֮��fetch�����޷����ؽ��������p��״̬�ͻ��Ϊrejected���Ӷ�����catch����ָ���Ļص�������
## 7��Promise.resolve()
��ʱ��Ҫ�����ж���תΪPromise����Promise.resolve��������������á�
```javascript
var jsPromise = Promise.resolve($.ajax('/whatever.json'));
```
������뽫jQuery���ɵ�deferred����תΪһ���µ�Promise����

Promise.resolve�ȼ��������д����
```javascript
Promise.resolve('foo')
// �ȼ���
new Promise(resolve => resolve('foo'))
```
Promise.resolve�����Ĳ����ֳ����������

��1��������һ��Promiseʵ��

���������Promiseʵ������ôPromise.resolve�������κ��޸ġ�ԭ�ⲻ���ط������ʵ����

��2��������һ��thenable����

thenable����ָ���Ǿ���then�����Ķ��󣬱��������������
```javascript
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
```
Promise.resolve�����Ὣ�������תΪPromise����Ȼ�������ִ��thenable�����then������
```javascript
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});
```
��������У�thenable�����then����ִ�к󣬶���p1��״̬�ͱ�Ϊresolved���Ӷ�����ִ������Ǹ�then����ָ���Ļص����������42��

��3���������Ǿ���then�����Ķ��󣬻�����Ͳ��Ƕ���

���������һ��ԭʼֵ��������һ��������then�����Ķ�����Promise.resolve��������һ���µ�Promise����״̬ΪResolved��
```javascript
var p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s)
});
// Hello
```
�����������һ���µ�Promise�����ʵ��p�������ַ���Hello�������첽�������жϷ����������Ǿ���then�����Ķ��󣩣�����Promiseʵ����״̬��һ���ɾ���Resolved�����Իص�����������ִ�С�Promise.resolve�����Ĳ�������ͬʱ�����ص�������

��4���������κβ���

Promise.resolve�����������ʱ����������ֱ�ӷ���һ��Resolved״̬��Promise����

���ԣ����ϣ���õ�һ��Promise���󣬱ȽϷ���ķ�������ֱ�ӵ���Promise.resolve������
```javascript
var p = Promise.resolve();
p.then(function () {
  // ...
});
```
�������ı���p����һ��Promise����

��Ҫע����ǣ�����resolve��Promise�������ڱ��֡��¼�ѭ������event loop���Ľ���ʱ������������һ�֡��¼�ѭ�����Ŀ�ʼʱ��
```javascript
setTimeout(function () {
  console.log('three');
}, 0);

Promise.resolve().then(function () {
  console.log('two');
});

console.log('one');

// one
// two
// three
```
��������У�setTimeout(fn, 0)����һ�֡��¼�ѭ������ʼʱִ�У�Promise.resolve()�ڱ��֡��¼�ѭ��������ʱִ�У�console.log(��one��)��������ִ�У�������������

##8��Promise.reject()
Promise.reject(reason)����Ҳ�᷵��һ���µ�Promiseʵ������ʵ����״̬Ϊrejected�����Ĳ����÷���Promise.resolve������ȫһ�¡�
```javascript
var p = Promise.reject('������');
// ��ͬ��
var p = new Promise((resolve, reject) => reject('������'))

p.then(null, function (s){
  console.log(s)
});
// ������
```
�����������һ��Promise�����ʵ��p��״̬Ϊrejected���ص�����������ִ�С�

�������õĸ��ӷ���
ES6��Promise API�ṩ�ķ������Ǻܶ࣬��Щ���õķ��������Լ��������������β�����������ES6֮�С��������õķ�����

## 9��done()
Promise����Ļص�����������then������catch������β��Ҫ�����һ�������׳����󣬶��п����޷���׽������ΪPromise�ڲ��Ĵ��󲻻�ð�ݵ�ȫ�֣�����ˣ����ǿ����ṩһ��done���������Ǵ��ڻص�����β�ˣ���֤�׳��κο��ܳ��ֵĴ���
```javascript
asyncFunc()
  .then(f1)
  .catch(r1)
  .then(f2)
  .done();
```
����ʵ�ִ����൱�򵥡�
```javascript
Promise.prototype.done = function (onFulfilled, onRejected) {
  this.then(onFulfilled, onRejected)
    .catch(function (reason) {
      // �׳�һ��ȫ�ִ���
      setTimeout(() => { throw reason }, 0);
    });
};
```
���������ɼ���done������ʹ�ã�������then���������ã��ṩFulfilled��Rejected״̬�Ļص�������Ҳ���Բ��ṩ�κβ�����������������done���Ჶ׽���κο��ܳ��ֵĴ��󣬲���ȫ���׳���

## 10/finally()
finally��������ָ������Promise�������״̬��Σ�����ִ�еĲ�����
����done�������������������һ����ͨ�Ļص�������Ϊ�������ú�����������������ִ�С�

������һ�����ӣ�������ʹ��Promise��������Ȼ��ʹ��finally�����ص���������
```javascript
server.listen(0)
  .then(function () {
    // run test
  })
  .finally(server.stop);
```
����ʵ��Ҳ�ܼ򵥡�
```javascript
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```
��������У�����ǰ���Promise��fulfilled����rejected������ִ�лص�����callback��

## 11��Ӧ��
����ͼƬ
���ǿ��Խ�ͼƬ�ļ���д��һ��Promise��һ��������ɣ�Promise��״̬�ͷ����仯��
```javascript
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    var image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```
## 12��Generator������Promise�Ľ��
ʹ��Generator�����������̣������첽������ʱ��ͨ������һ��Promise����
```javascript
function getFoo () {
  return new Promise(function (resolve, reject){
    resolve('foo');
  });
}

var g = function* () {
  try {
    var foo = yield getFoo();
    console.log(foo);
  } catch (e) {
    console.log(e);
  }
};

function run (generator) {
  var it = generator();

  function go(result) {
    if (result.done) return result.value;

    return result.value.then(function (value) {
      return go(it.next(value));
    }, function (error) {
      return go(it.throw(error));
    });
  }

  go(it.next());
}

run(g);
```
��������Generator����g֮�У���һ���첽����getFoo�������صľ���һ��Promise���󡣺���run�����������Promise���󣬲�������һ��next����

�ܽ᣺
promise ����״̬ Pending(������)��Resolved(����ɣ��ֳ�Fulfilled)��Rejected(��ʧ��)��
Χ��������״̬��������������resolve(Pending->Resolved),reject(Pending->Rejected)
״̬һ���ı䣬�Ͳ����ٱ�
�½�
1��new Promise(function(resolve,reject){
    ///�½�������ִ��
    if(true)
        resolve();
    else 
        reject();
}).then(function(){//resolve�ص�
})//reject�ص�����ѡ��ͨ��ֱ����catch��
2��Promise.resolve();
3��Promise.reject();
then�������ص���һ���µ�Promiseʵ����ע�⣬����ԭ���Ǹ�Promiseʵ��������˿��Բ�����ʽд������then���������ٵ�����һ��then������
catch����
1������첽�����׳�����״̬�ͻ��ΪRejected���ͻ����catch����ָ���Ļص������������������
2��then����ָ���Ļص�����������������׳�����Ҳ�ᱻcatch��������
Promise.all([p1,p2,p3]).then(function(datas){
          �����ж�Resolved(datas������)����һ��Rejected(datas����ʧ�ܵ�)
})
Promise.race([p1,p2,p3]).then(function(data){
          ��������һ��״̬�ı�
})

new Promise(function(){

}).then(function(){
    
}).catch(function(){

}).finally(function(){ 

}).done(error=>{  //��֤�׳��κο��ܳ��ֵĴ���
      
 });

Node.js��һ��unhandledRejection�¼���ר�ż���δ�����reject����
unhandledRejection�¼��ļ���������������������һ���Ǵ�����󣬵ڶ����Ǳ����Promiseʵ���������������˽ⷢ������Ļ�����Ϣ��
>process.on('unhandledRejection', function (err, p) {
>
>> console.error(err.stack);
>
>});