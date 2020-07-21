# oop

## 理解对象

* 对象的属性类型
  + 数据属性
  + 访问器属性
    - getter函数，读取访问器的属性
    - setter函数，写入访问器的属性
* 定义多个对象
* 读取对象的属性

## 创建对象 

* 工厂模式
  + 没有解决了对象识别的问题
  + 解决了创建多个相似对象的问题
* 构造函数模式
  + 与工厂模式的区别
    - 没有显式地创建对象
    - 直接将属性和方法赋给了this对象
    - 没有return语句
  + 构造函数始终以一个大写字母开头
  + 主要有4个步骤
    - 创建一个新的对象
    - 将构造函数的作用域赋给新对象（this指向这个新对象）
    - 执行构造函数中的代码（给对象添加新属性）
    - 返回新对象
  + 缺点
    - 对象里面的方法都要在每一个实例上重新创建一遍
    - 没有封装性可言
* 原型模式
  + 理解原型对象
    - 每创建一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性
    - 默认情况下原型对象对自动获得一个construor属性，这个属性是指向prototype属性所在函数的指针
    - 可以用isPrototypeOf()来判断对象之间是否存在原型的关系，返回值是Boolean
    - 调用原型上的方法，会执行两次搜索，第一次先询问实例函数，第二次再去询问原型
    - 使用hasOwnPrototype()可以用来检测一个属性是否存在实例中
  + 原型与in操作符
    - 单独使用
      - in操作符用来判断属性是否存在对象中，无论存在实例还是原型上。
      - function Person() {};

                Person.prototype.name = 'jack';
                Person.prototype.sayName = function () {
                    console.log("hello jack");
                };
                let person1 = new Person();
                let person2 = new Person();
                console.log("name" in person1, 'name' in person2);
                //true true
                person1.name = 'marry';
                console.log(person1.name);
                console.log("name" in person1, 'name' in person2);
                //true true

    - for-in循环使用
      - 返回所有能够通过对象访问、可枚举的属性其中包括了原型上的属性和实例上的属性
      - 可以用Object.keys()和Object.getOwnPropertyNames()来替代fon-in
  + 更简单的原型语法
    - 再函数的原型上设置属性时，constructor属性不会再指向该函数
    - function Person() {};

                Person.prototype = {
                    // constructor:Person,
                    name: "jack",
                    age: 18,
                    sayName: function () {
                        console.log(this.name);
                    }
                };

  + 原型的动态性
    - 在创建实例之后重写原型对象会切断现有原型和任何之前已经存在的对象实例之间的联系
    - function Person() {};

                Person.prototype.sayName = function () {
                    console.log('jack');
                };
                let friend = new Person();
                friend.sayName();
                Person.prototype = {
                    name: 'jack',
                    age: 18,
                    sayAge: function () {
                        console.log(this.name);
                    }
                }
                friend2.sayAge(); //error

  + 原生对象的原型
    - console.log(typeof Array.prototype.sort); //function

      console.log(typeof String.prototype.startsWith); //function

  + 原型对象的问题

   - 省略了为构造函数传递初始化参数的环节
   - 其共享的属性导致了实例化函数属性的污染
   - function Person() {}; 

                Person.prototype = {
                    name: "jack",
                    age: 18,
                    sayName: function () {
                        console.log(this.name);
                    },
                    friends: ['marry', 'luo']
                }
                let person1 = new Person();
                let person2 = new Person();
                person1.friends.push("fei");
                console.log(person1.friends);
                console.log(person2.friends);
                console.log(person1.friends === person2.friends);

* 组合使用构造函数模式和原型模式
  + 每一个实例都会有自己的一份实例属性的副本，但是同时又共享着对方的引用
  + 认同度最高的一种创建自定义类型的方法，是定义引用类型的一种默认形式
  + function Person(name, age, sex) {

                this.name = name;
                this.age = age;
                this.sex = sex;
                this.friend = ['xiaoming', 'xiaohong'];
            }
            Person.prototype = {
                constructor: Person,
                sayName: function () {
                    console.log(this.name);
                }
            }

* 动态原型模式
  + 把所有的信息封装在构造函数中，以便于开发者可以通过构造函数初始话原型
  + 使用动态原型模式时不能使用对象字面量重写原型会切断创建实例之后和新原
  + function Person(name, age) {

                this.name = name;
                this.age = age
                // 方法
                if (typeof this.sayName != 'function') {
                    Person.prototype.sayName = function () {
                        console.log(this.name)
                    }
                }
            };
            let person1 = new Person('jack',18)
            person1.sayName()
            console.log(person1 instanceof Person)型之间的联系

* 寄生构造函数模式
  + 这种模式的基本思想是创建一个函数，该函数只作为封装创建对象的代码，然后再返回新创建的对象。

  + function Person(name, age) {

                let obj = new Object();
                obj.name = name;
                obj.age = age;
                obj.sayName = function () {
                    console.log('hello ' + this.name);
                };
                return obj
            }
            let person1 = new Person('jack', 18);
            person1.sayName();

* 稳妥构造函数模式
  + 新创建的对象和的实例方法不引用this
  + 不适用new操作符调用构造函数
  + function Person(name, age) {

                let obj = new Object();
                obj.sayName = function () {
                    return name
                };
                return obj
            }
            let person1 = new Person('jack',18);
            console.log(person1.sayName());
## 继承
  - 实现继承主要依靠原型链来实现的
  - 原型链
    - 别忘记默认的原型
      - 所有函数的默认原型都是Object的实例，最顶层。
    - 确定原型和实例的关系
      - 可以通过instanceof操作符和isPrototypeof()方法来确定原型和实例之间的关系
    - 谨慎地定义方法
    - 原型链的问题
  - 借用构造函数
    - 传递参数
    - 借用构造函数的问题 
  - 组合继承
  - 原型式继承
  - 寄生式继承
  - 寄生组合式继承

