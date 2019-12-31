--- 
layout:     post
title:      javaScript学习笔记
subtitle:   面向对象
date:       2017-07-01
author:     Ruiyoung
header-img: img/post-bg-js-version.jpg
catalog: true
tags:
    - javaScript
---
### 基本概念  

> 类：每个对象都由类定义，可以把类看做对象的配方。
>> 类不仅要定义对象的的属性和方法，还要定义对象的内部工作工作原理。  
>> JavaScript并没有正式的类，创建一个对象只要定义一个该对象的构造函数并通过它创建对象即可。虽然类并不真正存在，我们也把对象定义叫做类。而且从功能上说，两者是等价的。  
> 实例：程序使用类创建对象时，生成的对象叫作类的实例。由类创建对象实例的过程叫做实例化  
> JavaScript 对象定义：可以把对象理解为属性集合，每个属性存放一个原始值、对象或函数  
>> 对象一般认为由方法和属性构成。方法的实质就是函数，而属性的实质就是变量，只不过这里由于它从属于某个对象所以叫法不同。  
> 面向对象：可以简单的理解为，不必去了解对象的内部结构，就可以去使用它。就比如我们可以使用手机打电话，但是不必去了解它内部的工作原理。就像我们使用Date对象的方法可以获取和设置时间，单是我们并不用去弄清楚它为什么会实现这个功能。  

### 创建对象的方法  

> 本质上都是把"属性"和"方法"，封装成一个对象  
##### 基本模式  
```
var people1=new Object();
people1.name='孙悟空';
people1.weapon='金箍棒';

//this是指的当前作用域下的对象，注意和谁调用这个方法有关，和在哪定义没啥关系
//这里也可以简单理解为this就指的这个函数属于谁，属于谁this就是指的谁
people1.run=function(){
return this.name+'的武器是'+this.weapon
}
alert(people1.name) 
alert(people1.run()) //注意方法的调用需要加()

```
缺陷：
- 如果创建多个对象会比较繁琐，效率低  
- 实例与原型之间，没有任何办法，可以看出有什么联系  

##### 工厂模式  

```
function creatPeople(name,weapon){
    var people=new Object() //可以类比为加工对象的原材料
    people.name=name;
    people.weapon=weapon;
    people.run=function(){
        return this.name+'的武器是'+this.weapon
    }  //以上步骤可以类比为加工对象的过程
    return people //注意一定要讲创建的对象返回 
    //可以类比为产品加工完毕出厂的工作
}
var wukong=creatPeople('孙悟空','金箍棒');
var bajian=creatPeople('猪八戒','钉耙');
//alert(wukong.run())
```
- 使用创建并返回特定类型的对象的工厂函数(其实就是普通函数，没啥区别，只是叫法不同)  
- 创建过程类似于工厂生产产品的过程，即：原材料--加工--产品...  
- 解决了多次重复创建多个对象的麻烦。  
- 问题：  
&emsp;&emsp;1.创建出的实例之间没有内在的联系，不能反映出它们是同一个原型对象的实例。    
&emsp;&emsp;2.创建对象的时候没有使用 new 关键字  
&emsp;&emsp;3.会造成资源浪费，因为每生成一个实例，都增加一个重复的内容，多占用一些内存。  

##### 构造函数模式  

```
//注意：构造函数不需要使用 return语句返回对象，它的返回是自动完成的
function People(name,weapon){
    this.name=name;
    this.weapon=weapon;
    this.run=function(){
        return this.name+'的武器是'+this.weapon
    }  
}
var wujing=new People('沙悟净','禅杖');
var wukong=new People('孙悟空','金箍棒');
var bajian=new People('猪八戒','钉耙');
alert(wujing.run())
//alert(wujing instanceof People)
var monster=new Object();
//People.call(monster,'妖怪','葫芦')
People.apply(monster,['妖怪','葫芦'])
// alert(monster.run())
// alert(monster.name)
var monster1=new People('小妖','长矛')
var monster2=new People('小妖','长矛')
alert(monster1.run()+'\n'+monster2.run())
alert(monster1.run==monster2.run)//两个对象实例的地址是不同的，说明两个对象会占用两个地址空间的内存
```
- new 调用的函数为构造函数，构造函数和普通函数区别仅仅在于是否使用了new来调用。  
- 所谓“构造函数”，就是专门用来生成“对象”的函数。它提供模板，作为对象的基本结构。  
- 构造函数内部使用了this变量。对构造函数使用new运算符，就能生成实例，并且this变量会绑定在实例对象上。  
- instanceof 验证原型对象与实例对象之间的关系。  
- 使用call和apply方法实现对象的冒充  
- 问题：浪费内存--使用构造函数每生成一个实例，都增加一个重复的内容，多占用一些内存。这样既不环保，也缺乏效率。  

##### 原型(Prototype)模式  

```
function Peopleobj(){}
Peopleobj.prototype.name='喽啰';
Peopleobj.prototype.weapon='大刀';
Peopleobj.prototype.run=function(){
    return this.name+'的武器是'+this.weapon 
}
var monster_1=new Peopleobj()
monster_1.job=[]
var monster_2=new Peopleobj()
//alert(monster_1.name+'\n'+monster_1.run())
//alert(monster_2.name+'\n'+monster_2.run())
//alert(monster_1.run)
//alert(monster_2.run)
//alert(monster_1.run==monster_2.run) //说明他们的引用是同一个地址
//这时所有实例的方法，其实都是同一个内存地址，指向prototype对象，因此就提高了运行效率。
//alert(Peopleobj.prototype.isPrototypeOf(monster_1));
// alert(monster_1.hasOwnProperty("name"));
// alert(monster_1.hasOwnProperty("job"));
alert("jobb" in monster_1);

//这种写法和前面的方式在使用上基本相同，注意是基本
function Monster(){} 
Monster.prototype={
    constructor: Monster, //此外强制指回Monster
    name:'喽啰', //原型字面量方式会将对象的constructor变为Object，
    weapon:'大刀',
    job:['巡山','打更'],
    run:function() {return this.name+'的工作是'+this.job }
}
var monsterA=new Monster()
monsterA.job.push('砍柴')
var monsterB=new Monster()
monsterB.job.push('挑水')
alert(monsterA.job)
alert(monsterB.job)
//alert(monsterA.constructor)
```
> Javascript规定，每一个构造函数都有一个prototype属性，指向另一个对象。这个对象的所有属性和方法，都会被构造函数的实例继承。可以把那些不变的属性和方法，直接定义在prototype对象上。  
- prototype方式定义的方式，函数不会拷贝到每一个实例中，所有的实例共享prototype中的定义，节省了内存。  
- Prototype模式的验证方法   
&emsp;&emsp;1.isPrototypeOf()这个方法用来判断，某个proptotype对象和某个实例之间的关系。    
&emsp;&emsp;2.hasOwnProperty()每个实例对象都有一个hasOwnProperty()方法，用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。   
&emsp;&emsp;3.in运算符in运算符可以用来判断，某个实例是否含有某个属性，不管是不是本地属性。in运算符还可以用来遍历某个对象的所有属性。  
- 对象的constructor属性用于返回创建该对象的构造函数(在JavaScript中，每个具有原型的对象都会自动获得constructor属性)  
- 原型方式的问题:  
&emsp;&emsp;构造函数没有参数。使用原型方式，不能通过给构造函数传递参数来初始化属性的值  
&emsp;&emsp;属性指向的是对象，而不是函数时。函数共享不会造成问题，但对象却很少被多个实例共享，如果共享的是对象就会造成问题。  

##### 构造函数和原型组合模式  

```
//构造函数和原型组合模式
function Monster(name,arr){
    constructor: Monster, 
    this.name=name
    this.job=arr
} 
Monster.prototype={
    run:function() {return this.name+'的工作是'+this.job }
} 
var monsterI=new Monster('小旋风',['巡山','打更','砍柴'])
var monsterII=new Monster('小钻风',['巡山','打更','挑水'])
alert(monsterI.run())
alert(monsterII.run())
```
> 目前最为常用的创建对象的方式。(jQuery类型的封装就是使用组合模式来实例的)   
> 这种概念非常简单，即用构造函数定义对象的所有非函数属性，用原型方式定义对象的函数属性（方法）。结果是，所有函数都只创建一次，而每个对象都具有自己的对象属性实例。  
> 组合模式还支持向构造函数传递参数，可谓是集两家之所长  

##### 动态原型模式  

```
function MonsterGo(name,arr){
    this.name=name
    this.job=arr 
    if (typeof this.run!= "function") 
    // {alert('对象初始化')
        MonsterGo.prototype.run=function(){
          return this.name+'的工作是'+this.job 
        }
        // alert('初始化结束')
    }
} 
var monsterI=new MonsterGo('小旋风',['巡山','打更','砍柴'])
var monsterII=new MonsterGo('小钻风',['巡山','打更','挑水'])
var monsterI2=new MonsterGo('小旋风',['巡山','打更','砍柴'])
var monsterII2=new MonsterGo('小钻风',['巡山','打更','挑水'])
// alert(monsterI.run())
// alert(monsterII.run())
```
> 动态原型方法的基本想法与混合的构造函数原型方式相同，即在构造函数内定义非函数属性，而函数属性则利用原型属性定义。  
> 组合模式中实例属性与共享方法（由原型定义）是分离的，这与纯面向对象语言不太一致；动态原型模式将所有构造信息都封装在构造函数中，又保持了组合的优点。  
> 其原理就是通过判断构造函数的原型中是否已经定义了共享的方法或属性，如果没有则定义，否则不再执行定义过程。该方式只原型上方法或属性只定义一次，且将所有构造过程都封装在构造函数中，对原型所做的修改能立即体现所有实例  

### 继承  

> 两个类的继承关系，就包含以以三个意思：  
>> 子类的实例可以共享父类的方法  
>> 子类可以覆盖或扩展父类的方法  
>> 子类和父类都是子类实例的类型  

- 对象冒充  
使用对象冒充（call或apply方法）（实质上是改变了this指针的指向）继承基类。  

```
function Monkey(_type,_home){
    this.type=_type;
    this.home=_home;
    this.say= function() {
        alert('我是快乐的小猴子，家住'+this.home)
    };
}

function Hero(_HP){
    this.HP=_HP;
}

function Magic_monkey(_type,_home,arr,_HP){
    //Monkey.call(this,_type,_home)
    Monkey.apply(this,[_type,_home])
    Hero.call(this,_HP)
    this.skill=arr;
}
var wukong=new Magic_monkey('猴子','花果山',['七十二变','筋斗云'],1000)
// alert(wukong.home);
// alert(wukong.type);
// alert(wukong.skill);
alert(wukong.HP);
wukong.say();

```  

- 原型链继承  

> prototype 对象是个模板，要实例化的对象都以这个模板为基础。总而言之，prototype 对象的任何属性和方法都被传递给那个类的所有实例。原型链利用这种功能来实现继承机制。    
> 原型链的弊端是不支持多重继承。记住，原型链会用另一类型的对象重写类的 prototype 属性。  
> 子类的所有属性和方法都必须出现在 prototype 属性被赋值后，因为在它之前赋值的所有方法都会被删除。因为 prototype 属性被替换成了新对象，添加了新方法的原始对象将被销毁。   

```
function Monkey(){}
Monkey.prototype.type='猴子';
Monkey.prototype.say=function(){alert('我是快乐的猴子')}

function Magicmonkey(){}
//将Magicmonkey的prototype对象指向一个Monkey的实例。
//相当于删除了prototype 对象原先的值，然后赋予一个新值。
//不能继承多个类，后边的会覆盖前边的
Magicmonkey.prototype=new Monkey();
Magicmonkey.prototype.skill='法术';
var sunWukong=new Magicmonkey()
alert(sunWukong.type)
sunWukong.say()
alert(sunWukong.skill)
```  

- 混合方式继承  
> 创建类的最好方式是用构造函数定义属性，用原型定义方法。这种方式同样适用于继承机制，用对象冒充继承构造函数的属性，用原型链继承 prototype 对象的方法。  