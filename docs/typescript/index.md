
## TypeScript 学习笔记

### 一、数据类型

ts的数据类型几乎与js一致 提供了`数字（number）`、`字符串（string）`、`结构体（Object）`、`布尔值`等基础数据类型，
另外还提供了`枚举类（enum）` `数字（number）` 和js中的一样ts中的数字都是浮点型 除了支持十进制和十六进制的字面量  
ts还支持了 `es2015`中引入的二进制和八进制表示形式

```js
    let dec:number = 6;//十进制
    let hex:number = 0xf00d;//十六进制
    let bin:number = 0b1010;//二进制
    let oct:number = 0o744;//八进制
```

#### 字符串（string）

与js的string一致 可以使用`双引号( " )` `单引号( ' )` 反引号( ` ) 皆可表示字符串

```js
    let who: string = "我";// "双引号
    let name: string = '王钢蛋';// '单引号
    let str: string = `${who}是${name}`;// `模板字符串
    console.log(str); //输出  我是王钢蛋
```

#### 数组（Array）

ts 的数组和js的数组略有不同 ,ts中有两种形式可以定义数组

##### 第一种

是在数据类型后面跟上[] 表示有此类型的元素组成的一个数组 这点跟java类似

```js
    let arr: string[] = ["蔡徐坤","谢广坤","王钢蛋","吴电鳗"];
```

##### 第二种

方式是使用数组Array的泛型

```js
    let arr: Array<string> = ["蔡徐坤","谢广坤","王钢蛋","吴电鳗"];
    //在以上两种形式声明的数组中所有元素必须都是同一类型 那么如何定义一个可以包含不同类型的 数组呢
    let arr2: Array<string | number> = [1,"one"]; //数组元素可以是 string型 亦可为 number型
    let arr3: Array<any> = [1,"yes",true,{attr:'ok'}]; //any表示任意类型
```

#### 元组（Tuple）

元组表示一个已知长度和各元素类型的数组，各个元素类型可不相同
比如

```js
let t1: [number,string]
t1 = [1,"one"];//正常赋值
t1 = ["one",1];//报错 元素类型与声明时不一致
t1 = [1,"one",3];//报错  元组长度不对
```

#### 枚举（enum）

ts的enum和java的enum基本一致,使用枚举类型可以为一组数值赋予友好的名字

```js
enum Color{ Red = "#FF0000" ,Green = "#00FF00" ,Blue = "#0000FF" }
let green:Color.Green;
console.log(green);// 输出 #00FF00
```

#### 任意值（any）

有时候在编程阶段不知道一个变量值的类型,这个值的可能来自于动态内容  这种情况下 我们不希望类型检查器对这些值进行检查 而是直接让它们通过编译，其实就跟js中直接let一个变量效果是一样的 该变量可以有任意类型的值

```js
let  a:any = 4;
a = "four";
a = true;
a = {}
//以上代码皆可运行
```

#### Null 和 Undefined

ts中`undefined`和`null`有自己各自的类型 分别叫做`undefined`和 `null` 和`void`相似他们本身的类型用处不大
默认情况下`null`和`undefined`是所有类型的子类型 也就是说 你可以把`null`和`undefined`赋值给任何类型的变量

```js
let a:boolean;
let b:string;
let c:number;
a = null;
b = undefined;
c = undefined;
```

#### 空值（void）

void类型与any类型正好相反 它表示没有任何类型 一般用在声明函数返回值时  如果一个函数没有返回值 则其返回值类型为void

```js
function fun():void{
    console.log(`Hello Wolrd！`)
}
```

声明一个void型变量没有多大意义 因为你只能赋予 undefined 和 null

```js
let a:void = undefined;
```

#### Nerver

nerver类型表示那些永远不存在值的类型 这个类型花里胡哨的 不想看
Object
object表示非原始类型，也就是除number，string，boolean，symbol，null或undefined之外的类型。
使用object类型，就可以更好的表示像Object.create这样的API。例如：

```js
declare function create(o: object | null): void;
create({ prop: 0 }); // OK
create(null); // OK
create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 二、变量声明

这章没什么好看的 就是一些 `var` `let` `const` 以及其各种作用域问题 跳过

### 三、接口（interface）

ts的核心原则之一就对值所具有的结构进行检查 它有时被称作"鸭式辩型法" 或 "结构性子类型化" 在TS中 接口的作用就是为这些类型命名和你的代码定义契约

#### 鸭式辩型法

第一次听到这个名字 表情是这样的其实理解起来非常简单,鸭式辩型来自一句名言
像鸭子一样走路并且嘎嘎叫的就叫鸭子 ---------------鲁迅
举个栗子

```js
interface Duck {
    name:string,
    age:3  
}
function printDuck(d:Duck){
    console.log(`打印鸭子:`, d);
}
let obj = {name:"可达鸭",age:18};
printDuck(obj)
```

上面例子中  变量obj拥有了 Duck的所有属性 那么obj就是一只鸭子  这就是传说中的"鸭式辩型"
下面开始正式了解`interface`

#### 初步了解

好下面来再观察下接口是如何工作的

```js
//打印人类
function printHuman(human:{name:string,age:number,sex:boolean}){
    console.log(`打印人类`,human)
}
let ladyboy = {name: '蔡徐坤', age:38, sex:true,hobby:'唱调 rap 篮球 Music'};
printHuman(ladyboy)
```

在上面代码中 类型检查器会查看 printHuman的调用,printHuman有一个参数，并要求这个对象参数name age sex三个属性,需要注意的是 我们传入的对象参数实际上包含很多属性 但编译器只会检查那些必须的属性书否存在，并且其类型是否匹配，然而 有些时候TS却不会这么宽松 我们下面会继续学习
接下在用interface重写上面的例子

```js
//定义人类
interface Human{
    name:string,
  age:number,
  sex:boolean,  
}
//打印人类
function printHuman(human:Human){
    console.log(`打印人类`,human)
}
let ladyboy = {name: '蔡徐坤', age:38, sex:true,hobby:'唱调 rap 篮球 Music'};
printHuman(ladyboy)
```

Human 接口就好比一个名字,用来描述上面例子里的需求，他代表了有 name age  sex 属性 且定一个了各自类型的对象 需要注意的是 我们这里不能像在其他语言里一样说 传给 printHuman() 的对象实现了这个接口,TS只会去关注值的外形，只要传入的对象满足上面提到的条件，那么它就是被允许的，还有要注意的就是 类型检查器不会去检查属性的顺序 只要相应的属性存在并且类型正确即可

#### 可选属性

接口里的属性不是全都必需的 有些是只在某些条件下存在或者根本不粗在 可选属性在应用 `options bags 模式很常用 ，即给函数传入的参数对象中只有部分属性赋值了
国际惯例

```js
//定义人类
interface Human{
    name:string,
  age:number,
  sex:boolean,
  hobby?:string  
}
//打印人类
function printHuman(human:Human){
    console.log(`打印人类`,human)
}
let ladyboy = {name: '蔡徐坤', age:38, sex:true,hobby:'唱调 rap 篮球 Music'};
printHuman(ladyboy)
```

在上面例子中可以看出 可选属性和普通接口的定义差不多，只是在可选属性名字定义后面加一个 问号`?`
可选属性的好处之一就是可以对可能存在的属性进行预定义 好处之二是可以捕获引用了不存在的属性时的错误比如我们故意将printHuman中的 hobby拼错误 就会得到一个错误提示

#### 只读属性

一些对象属性只能在对象刚刚创建的时候修改其值 你可以在属性名前加 readonly  来指定只读属性

```js
//定义人类
interface Human{
    name:string,
    age:number,
    readonly sex:string,
    hobby?:string,
}
let ladyboy:Human = {name: '蔡徐坤', age:38, sex:'girl',hobby:'唱调 rap 篮球 Music'};
ladyboy.age = 48 //正常执行
ladyboy.sex ='boy'//报错
```

上面代码中 ladyboy的sex在创建后将无法改变 否则会出现如下报错

#### 只读数组

 TS具有只读数组 `Readonly Array<T>` 类型 `Array<T>` 相似，它只是把所有的可变方法去掉了 如 push  之类的会改变数组元素的方法 因此可以确保数组创建后不能被修改

```js
let a: number[] = [1,2,3,4];
let b:ReadonlyArray<number> = a;
b[0] = 2; //出错
b.push(5);//出错
let c:number[] = b;//出错 不可以将ReadonlyArray<T> 直接复制给 Array<T>
let d:number[] = <number[]> b;//可以通过类型断言重写赋值
console.log(`调试:`,a);
```

#### readonly VS const

const 定义只读变量  readonly定义只读属性
额外属性检查

```js
interface  Woman {
    name: string,//姓名
    age?:number//年龄
}
function createWoman(woman:Woman):Woman{
    return   woman
}
let cxk:Woman = {name:'蔡徐坤',jjsize:'18cm'};//报错
let cxk2 = createWoman( {name:'蔡徐坤',jjsize:'18cm'})//报错
```

上面代码会出现如下报错 因为interface Woman 没jjsize属性
TS会认为这段代码可能存在bug 对象字面量会被特殊对待且经过额外的属性检查,当他们赋值给变量或作为参数传递的时候 如果一个对象的字面量存在任何 '目标类型' 不包含的属性时 你会得到一个错误

想要绕开这些检查非常简单 主要有以下三种方法

#### 方法一：类型断言

```js
let cxk:Woman = {name:'蔡徐坤',jjsize:'18cm'}  as Woman;
```

#### 方法二:索引签名

索引签名是最佳的方式 ，前提是你能够确定这个对象可能具有某些为特殊用途的额外属性 如果 Woman 带有上面定义类型的 name 和 age 属性 ,并且还带有任意数量和其他属性，那么我们可以这样定义它

```js
interface  Woman {
    name: string,//姓名
    age?:number,//年龄
    [propName :string]:any //任意数量任意类型的其他属性
}
```

后面会继续深入学习索引签名

#### 方法三:变量赋值

```js
interface  Woman {
    name: string,//姓名
    age?:number,//年龄
}
let human = {name:'蔡徐坤',jjsize:'18cm'};
let cxk:Woman = human
```

这个方法就是将这个对象复制给另变量 huamn  因为 huamn  不会经过额外属性检查所以编译器不会报错（这里我是看不大懂 凭什么就不经过额外属性检查了）
这里需要注意的是 huamn  和 Woman 必须要存在共同属性时才能使用
像下方这样写是会报错的

```js
let human = {jjsize:'18cm'};
let cxk:Woman = human;
```

#### 函数类型

接口能描述js中各种对象拥有的各样外形，除了描述带有属性的对象外，接口也可以描述函数类型 为了使用接口表示函数类型 我们需要给接口定义一个调用签名 （就是定义参数列表和返回值类型）

```js
//定义一个方法 Todo 接收 时间 地点 人物 事件 四个参数
interface Todo {
    (time:string,where:string,who:string,something:string):boolean
}
let playBasketball:Todo
playBasketball=function(time:string,where:string,who:string,something:string):boolean{
    console.log(`Todo:${who}于${time}在${where}${something}`)
    return true;
}
playBasketball("清明节","孙笑川坟头","蔡徐坤","打篮球");
//输出结果 Todo:蔡徐坤于清明节在孙笑川坟头打篮球
```

对于函数的类型检查来说 函数名不需要与接口里定义的一致,函数的参数会进行逐个检查 要求对应位置上的参数类型是兼容的 如果你不想指定类型 TS的类型系统会推断出参数类型

#### 可索引的类型

可索引类型具有一个索引签名，它描述了对象索引的类型 还有相应索引返回值的类型

```js
interface  Woman {
    name: string,//姓名
    age?:number,//年龄
}
//女团
interface WomansArray {
    [index: number]:Woman;
}
//定义女团变量
let womensTeam: WomansArray;
womensTeam = [
    {name:'蔡徐坤'},
    {name:'鹿晗'},
    {name:'张大大'}
]
//单飞的女团成员
let singleWoman:Woman = womensTeam[0];
let singleWoman2:Woman = womensTeam['1']; //字符串与number一致
```

上面的例子里 我们定义了 WomanArray 接口 它具有索引签名，这个索引签名表示了当用 `string` 去索引WomanArray 时或得到 Woman  类型的返回值
TS支持两种索引签名:字符串和数字。可以同时保持两种类型的索引 但是数字索引的返回值必须是字符串索引返回值的子类型（这句真™绕），这是因为当使用 number 索引时 JS会将它转换成 `string` 然后再去索引对象  也就是说用 `number`  100 去索引等同于用 `string` "100" 去索引 因此 两者需要保持一致
（这一块还是看不大懂 有空再看）

#### Class类型

与C#或Java里的基本作用一样TS也能够用它来明确的强制一个类去符合某种契约

```js
//动物
interface Animal {
    name?:string; //名字 动物不一定都有名字 所以用了可选属性
    eat(food: string):void   //描述一个eat(吃) 方法  任何动物都需要进食
}
// Huamn （人类） 实现 Animal 接口
class Huamn implements Animal {
    name:string; //名字
    sex:string;  //性别
    age:number;  //年龄
    //构造函数
    constructor(name:string,sex:string,age:number){
        this.name = name;
        this.sex = sex;
        this.age =age;
    }
    //实现接口中的eat()
    eat(food:string){
        console.log(`${this.age}岁的${this.name}${this.sex}士正在吃${food}`)
    }
}
let  cxk:Huamn = new Huamn("蔡徐坤","女",38)
cxk.eat("篮球");//输出结果 ..... 38岁的蔡徐坤女士正在吃篮球
```

你可以在接口中描述一个方法，在类里实现它 如同上例中的 setTime 方法一样,接口描述了类的公共部分 ，而不是公共和私有两部分 它不会帮你检查类是否具有私有成员

#### 类静态部分与实例部分的区别

类具有两个类型:静态部分的类型和实例的类型，当用构造器 `constructor` 签名去定义一个接口并试图定义一个类去实现这个接口时会得到一个错误 ，这是因为一个类实现了一个接口时，只对其实力部分进行了类型检查 `constructor` 属于类的静态部分所以不在检查范围内，因此我们应该直接操作类的静态部分
（这一块太花里胡哨了 迟点再看）

#### 接口继承

和类一样 接口也可以相互继承，这让我们能够从一个接口里复制成员到另一个接口里，可以灵活地接口分割到可重用的模块里

```js
//动物
interface Animal {
    name?:string; //名字 动物不一定都有名字 所以用了可选属性
    eat(food: string):void   //描述一个eat(吃) 方法  任何动物都需要进食
}
// Huamn （人类） 实现 Animal 接口
interface Huamn extends Animal {
    name:string; //名字
    sex:string;  //性别
    age:number;  //年龄
}
class Japanese implements Huamn{
    name:string; //名字
    sex:string;  //性别
    age:number;  //年龄
    Country:string = "日本";
    constructor(name:string,sex:string,age:number){
        this.name = name
        this.sex = sex
        this.age = age
    }
    eat(food: string): void {
        console.log(`来自${this.Country}${this.age}岁的${this.name}${this.sex}士正在吃${food}`)
    }
}
let  cxk:Japanese = new Japanese("蔡徐坤","女",38)
cxk.eat("篮球"); //输出  来自日本38岁的蔡徐坤女士正在吃篮球
```

#### 一个接口可以继承多个接口 创建出多个接口合成的接口

```js
//动物
interface Animal {
    name?: string; //名字 动物不一定都有名字 所以用了可选属性
    eat(food: string): void   //描述一个eat(吃) 方法  任何动物都需要进食
}
// Huamn （人类） 实现 Animal 接口
interface Huamn extends Animal {
    name: string; //名字
    sex: string;  //性别
    age: number;  //年龄
}
//明星
 interface  Star {
    fans: number//粉丝数
    opus: string[] //代表作
}
//假明星  继承了   Human和Star 两个接口
interface FakeStar extends Huamn,Star{
    buyFakeFans(num:number) //制造假粉丝
    sendLawyersLetter(who:string)//发律师函
}
class StupidStar implements FakeStar{
    public name:string;
    public sex:string;
    public fans:number;
    public opus:string[]=[];
    public age:number;
    constructor(name,sex,age,fans){
        this.name = name
        this.sex = sex
        this.age = age
        this.fans =fans
    }
    //添加作品
    public addOpus(name:string){
        this.opus.push(name)
    }
    public eat(food: string): void {
        console.log(`粉丝量有${this.fans / 10000 }万,今年${this.age}岁，代表作有${JSON.stringify(this.opus)}的${this.name}${this.sex}士正在吃${food}`)
    }
    public buyFakeFans(num: number) {
        console.log(`粉丝量有${this.fans / 10000 }万,今年${this.age}岁，代表作有${JSON.stringify(this.opus)}的${this.name}${this.sex}士成功给自己买了${num/10000}万个假粉丝,`)
        this.fans += num
    }
    public sendLawyersLetter(who: string) {
        console.log(`粉丝量有${this.fans / 10000 }万,今年${this.age}岁，代表作有${JSON.stringify(this.opus)}的${this.name}${this.sex}士给${who}发了一封律师函`)
    }
}
let cxk: StupidStar = new StupidStar("蔡徐坤", "女", "38",10000000)
cxk.buyFakeFans(100000); //打印输出  粉丝量有1000万,今年38岁，代表作有[]的蔡徐坤女士成功给自己买了10万个假粉丝
cxk.eat("篮球"); //打印输出 粉丝量有1010万,今年38岁，代表作有[]的蔡徐坤女士正在吃篮球
cxk.addOpus("《鸡你太美》") //添加作品
cxk.sendLawyersLetter("BiliBili");//粉丝量有1010万,今年38岁，代表作有["《鸡你太美》"]的蔡徐坤女士给BiliBili发了一封律师函
```

#### 混合类型

这个感觉没什么用 看不明白 有空再看。下面是原文
先前我们提过，接口能够描述JavaScript里丰富的类型。 因为JavaScript其动态灵活的特点，有时你会希望一个对象可以同时具有上面提到的多种类型。
一个例子就是，一个对象可以同时做为函数和对象使用，并带有额外的属性。

```js
    interface Counter {
        (start: number): string;
        interval: number;
        reset(): void;
    }
    function getCounter(): Counter {
        let counter = <Counter>function (start: number): string { return '' };
        counter.interval = 123;
        counter.reset = function () { };
        return counter;
    }
    let c = getCounter();
    c(10);
    c.reset();
    c.interval = 5.0;
```

在使用JavaScript第三方库的时候，你可能需要像上面那样去完整地定义类型。

#### 接口继承类

##### 这是什么骚操作

   当接口继承哼了一个Class类型时 它会继承Class的成员 但不包括其实现，就好像接口声明了类中所有存在的成员但并没有提供具体实现 ，接口同样会继承Class的 `private` 和 `protected`  这意味整合当你创建一个接口继承了一个拥有私有保护的成员类时 这个接口类型只能被这个类或其子类所实现 当项目中有一个庞大的继承结构时这很有用 但要注意的是代码只在子类拥有特定属性时起作用 除了继承自基类 子类之间不必相关联

```js
class Control {
    private state: any;
}
interface SelectableControl extends Control {
    select(): void;
}
class Button extends Control implements SelectableControl {
    select() { }
}
class TextBox extends Control {
    select() { }
}
// Error: Property 'state' is missing in type 'Image'.
class Image implements SelectableControl {
    select() { }
}
class Location {
}
```

在上面的例子里，SelectableControl包含了Control的所有成员，包括私有成员state。 因为state是私有成员，所以只能够是Control的子类们才能实现SelectableControl接口。 因为只有Control的子类才能够拥有一个声明于Control的私有成员state，这对私有成员的兼容性是必需的。
在Control类内部，是允许通过SelectableControl的实例来访问私有成员state的。 实际上，SelectableControl就像Control一样，并拥有一个select方法。 Button和TextBox类是SelectableControl的子类（因为它们都继承自Control并有select方法），但Image和Location类并不是这样的

### 四、类（Class）

基本说明就不说了 跟Java一毛一样

#### 继承

TS中我们可以使用常用的面向对象模式，基于类的程序设计中一种最基本的模式是允许使用继承来拓展现有的类

```js
class Animal {//动物
    move(distanceInMeters: number = 0) { //移动方法
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}
class Dog extends Animal {//狗
    bark() { //叫
        console.log('Woof! Woof!');
    }
}
const dog = new Dog();//实例化
dog.bark();
dog.move(10);
dog.bark();
```

#### super关键字

 在子类中 super 指向父类的this 实际应用见下例

```js
class Animal{
    name:string
    constructor(name:string){
        this.name = name
    }
    eat(food:string){
        console.log(`${this.name}正在吃${food}`)
    }
}
class Human extends Animal{
    constructor(name:string) {
        super(name);
    }
    eating(food:string){
        super.eat(food)
    }
}
let cxk = new Human("蔡徐坤");
cxk.eating("🏀")
```

#### 类成员修饰符

控制类成员的访问权限 默认全部为public
还有另外三种修饰符
`public`
`private`
`protected`
`readonly`
