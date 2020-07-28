### BOM

* window对象

  在网页中定义的任何一个对象、变量和函数、都以window作为其global对象。

    - 全局作用域
      - 在全局声明的函数和对象都会变成window的方法和属性
    - 窗口关系及框架
      - 可以使用top.farms(i)来引用框架
    - 窗口的位置
        - 用来获取元素在浏览器窗口的位置
        - screenLeft和screenTop
        - screenX和screenY
    - 窗口的大小
        - window.innerHeight和window.innerWidth
        - document.documentElement.clientHeight和document.documentElement.clientWidth
        - document.body.clientWidth和document.body.clientHeight
    - 导航和打开窗口
        - 弹出窗口
            - alert
        - 安全限制
        - 弹出窗口屏蔽程序
    - 间接调用和超时调用
        - 可以设置超时值和间歇时间来调度代码
    - 系统对话框
* location 对象
  提供了当前窗口中加载的文档的有关信息
    - 查询字符串参数
    - 位置操作
* navigator对象
  是识别客户端浏览器的事实标准
    - 检测插件
    - 注册处理程序
* screen对象
    - 主要显示浏览器窗口外部的显示器信息
* history对象
    - 保存着用户上网的历史记录
