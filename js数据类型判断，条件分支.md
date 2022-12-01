####if语句和逻辑运算
所有基本类型中Boolean值是false的只有6个，分别是：0，NaN，''，null，undefined，false引用类型Boolean值全是true
if条件是单个值时，如果truly，条件成立，如果是falsly，条件不成立

####逻辑运算符以及他们的运算规则？
1. && 逻辑与 两边都是true，才返回true，否则返回false
2. || 逻辑或 两边只有有一个true，就返回true，否则返回false
3. ！逻辑非 用来取一个布尔值相反的值

####数据类型判断
```
//typeof对于基本类型的判断是没问题的，但是遇到引用数据类型（如：Array）是不起作用的
console.log(typeof 2)    //number
console.log(typeof null) //object

//instanceof 只能正确判断引用数据类型，而不能判断基本数据类型，其内部运行机制是判断在其原型链中能否找到该类型的原型
console.log([] instanceof Array)   //true
console.log(function(){} instanceof Function)  //true
console.log({} instanceof Object) //true

//constructor似乎完全可以应对基本数据类型和引用数据类型，但如果声明了一个构造函数，并且把他的原型指向了Array的原型，所以这种情况下，constructor就显得有点力不从心
console.log((true).constrcutor === Boolean); //true
console.log(('str').constructor === String); //true
console.log(([]).constrcutor === Array); //true
console.log((function(){}).constructor === Function); //true
console.log(({}).constructor === Object); //true
console.log((2).constructor === Number); //true

//Object.prototype.toString.call()完美解决方案，可以通过toString()来获取每个对象的类型，`Object.prototype.toString.call()`使用Object对象的原型方法toString来判断数据类型：

var a = Object.prototype.toString()
console.log(a.call(2));
console.log(a.call(true));
console.log(a.call('str'));
console.log(a.call([]));
console.log(a.call(function(){}));
console.log(a.call({}));
console.log(a.call(undefined));
console.log(a.call(null));
补充：基本数据类型赋值的时候，赋的是具体的值，引用数据类型传的是地址，一个变另一个跟这变
```

####js数据类型转换
在JavaScript中数据类型转换有三种情况：
转换为数字（调用Number(),parseInt(),parseFloat()）;
转换为字符串（调用toString(),String()方法）;
转换为布尔值（调用Boolean()方法）还有隐式转换注意：null，undefined没有.toString()方法

```
//转换为数字
Number():可以把任意转换成数字，如果要转换的字符串不是数字的值，则会返回NaN

Number('1') //1
Number(true) //1
Number('123s') //NaN
Nember({}) //NaN

parseInt(string,radix):解析一个字符串并返回指定基数的十进制整数，radix是2-36之间的整数，表示被解析字符串的基数；

parseInt('2') //2
parseInt('2',10) //2
parseInt('2',2) //NaN
parseInt('a123') //NaN  如果第一个字符不是数字或者符号就返回NaN
parseInt('123a') //123

parseFloat(string):解析一个参数并返回一个浮点数
parseFloat('123a'); // 123
parseFloat('123a.01') //123
parseFloat('123.01') //123.01
parseFloat('123.01.1') //123.01

隐式转换
let str = '123';
let res = str-1; //122
str+1;//1231
+str+1; //124

转换为字符串
.toString()  //注意：null,undefined不能调用

Number(123).toString() //'123'

[].toString() //''

true.toString() //'true'

String()都能转
String(123);  //'123'
String(true); //'true'
String([]);   //''
string(null); //'null'
String(undefined);  //'undefined'
String({}); //'[Object Object]'

隐式转换，当+两边有一个是字符串，另一个是其他类型时，会先把其他类型转换成字符串再进行字符串拼接，返回字符串；

let a = 1;
a+'' //'1'
转换为布尔值
0,''(空字符串),null,undefined,NaN会转成false，其它都是true
Boolean();
Boolean(''); //false
Boolean(0); //false
Boolean(1); //true
Boolean(null); //false
Boolean(undefined); //false
Boolean(NaN); //false
Boolean({}); //true
Boolean([]); //true

条件语句
let a;
if(a){
    //...  //这里a为undefined，会转为false，所以该条件语句内部不会执行
}

隐式转换！！
let str = '111';
console.log(!!str);  //true

{}和[]的valueOf和toString的返回结果？
valueOf:返回指定对象的原始值

对象                 返回值
Array             返回数组对象本身
Boolean           布尔值
Date              存储时间是从1970年1月1日开始计算的毫秒数UTC
Function          函数本身
Number            数字值
Object            对象本身。这是默认情况
String            字符串值
                  Math和Error对象没有valueOf方法

toString():返回一个表示对象的字符串。默认情况下，toString()方法被每个Object对象继承。如果此方法在自定义对象中未被覆盖，toString()返回"[object type]",其中type是类型

({}).valueOf()  //{}
({}).toString()  //'[Object Object]'
[].valueOf //[]
[].toString() //''
```

####数据类型相比较objected.is== 和 ===
1. === 属于严格判断，直接判断两者类型是否相同，如果两边的类型不一致，不会做强制类型转换，不同则返回false。如果相同再比较大小，不会进行任何隐式转换对于引用类型来说，比较的是引用内存地址，所以===这种方式的比较，除非两者存储的内存地址相同才相等，反之false。
2. == 二等表示值相等。判断操作符两边对象或值是否相等类型可以不同，如果两边的类型不一致，则会进行强制类型转化后再进行比较，使用Number()转换成Number类型在进行判断。例外规则，null==undefined,null/undefined进行运算时不进行隐式类型转换。通常把值转为Boolean值，进行条件判断。Boolean(null)===Boolean(undefined)——false === false结果为true
3. Object.is()在===基础上特别处理了NaN,-0,+0,保证-0与+0不相等，但NaN与NaN相等

```
==操作符的强制转换类型转换规则

字符串和数字之间的相等比较，将字符串转换为数字之后再进行比较
其他类型和布尔类型之间的相等比较，先将布尔值转换为数字后，再应用其他规则进行比较。
null和undefined之间的相等比较，结果为真。其他值和它们进行比较都返回假值。
对象和非对象之间的相等比较，对象先调用ToPrimitive抽象操作后，再进行比较。
如果一个操作值为NaN,则相等比较返回false（NaN本身不等于NaN）。
如果两个操作值都是对象，则比较它们是不是指向同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回true，否则，则返回false。

'1' == 1; //true
'1' === 1; //false
NaN == NaN; //false
+0 = -0; //true
+0 === -0; //true
Object.is(+0,-0);  //false
Object.is(NaN,NaN); //true
```

####typeof null的结果是什么，为什么？
typeof null 的结果是Object。
在 JavaScript 第一个版本中，所有值都存储在 32 位的单元中，每个单元包含一个小的 类型标签(1-3 bits) 以及当前要存储值的真实数据。类型标签存储在每个单元的低位中，共有五种数据类型：

```
000:object -当前存储的数据指向一个对象
1:int      -当前存储的数据是一个31位的有符号整数
010:double -当前存储的数据指向一个双精度的浮点数
100:string -当前存储的数据指向一个字符串
110:boolean -当前存储的数据是布尔值
```

有两种特殊数据类型：
+ undefined的值是 (-2)30(一个超出整数范围的数字)；
+ null 的值是机器码 NULL 指针(null 指针的值全是 0)
那也就是说null的类型标签也是000，和Object的类型标签一样，所以会被判定为Object。





