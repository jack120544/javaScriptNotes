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
