### 函数表达式

* 函数声明提升：在执行代码前会先读取函数声明
* 匿名函数：创建一个函数并把它赋值给一个变量
* 递归
    - 递归函数通过名字调用自身
    - function factorial(num) {

                if (num <= 1) {
                    return 1;
                } else {
                    return num * factorial(num - 1);
                }
            }
            let anotherFactorial = factorial;
            factorial = null;
            console.log(anotherFactorial(4));//程序报错

    - 解决当factorial赋值给一个变量，factorial=null时的问题，使用arguments.callee，但是在严格模式下不能脚本访问arguments.callee
    - function factorial(num) {

                if (num <= 1) {
                    return 1;
                } else {
                    return num * arguments.callee(num - 1);
                }
            }
            let anotherFactorial = factorial;
            factorial = null
            console.log(anotherFactorial(4));

    - 也可以创建多一个函数，然后赋值给变量factorial
    - let factorial = (function f(num) {

                    if (num <= 1) {
                        return 1;
                    } else {
                        return num * f(num - 1);
                    }
                })
                let anotherFactorial = factorial;
                factorial = null;
                console.log(anotherFactorial(4));//不会报错

* 闭包
    - 指的是有权访问另外一个函数作用域中变量的函数
    - 闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存
    - 慎重使用闭包
    - 闭包和变量
        - 作用域链的机制会引发一个副作用，即闭包只能取得包含函数中任何变量的最后一个值

        - function createFunction() {

                var result = new Array();
                for (var i = 0; i < 10; i++) {
                    result[i] = function () {
                        return i
                    }
                }
                return result;
            }
            console.log(createFunction())
            // 每一个函数内部i的值都是10
            // 通过创建一个匿名函数强制让闭包的行为符合预期为每个函数都返回自己的索引值

        - function createFunction() {

                var result = new Array();
                for (var i = 0; i < 10; i++) {
                    result[i] = function (num) {
                        return function () {
                            return num
                        }
                    }(i);
                }
                return result;
            }
            console.log(createFunction())

    - 关于this对象
        - var name = 'window';

            let obj = {
                name: "obj",
                getName: function () {
                    return function () {
                        return _this.name
                    }
                }
            }
            console.log(obj.getName()());//window

        - var name = 'window';

            let obj = {
                name: "obj",
                getName: function () {
                    let _this = this
                    return function () {
                        return _this.name
                    }
                }
            }
            console.log(obj.getName()());//obj
    - 内存泄漏
        - 如果闭包中的作用域链中保存着一个HTML元素，那么就意味着该元素无法被销毁
- 模仿块级作用域
- 私有变量
    - 静态私有变量
    - 模块模式
    - 增强的模块模式

