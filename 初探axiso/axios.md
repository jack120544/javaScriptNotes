### axios是什么？
- axios是一个基于Promise的http客户端，可以用于浏览器和node.js，主要的作用是拦截请求和响应，转换和请求响应数据
其他两种原生的调用后端接口的方法相比于axios是比较复杂的，这两种方法分别是XMLHttpRequest和fetch，其用法如下：
- XMLHttpRequest的get请求：
const xhr = new XMLHttpRequest();
        xhr.open("get", "http://localhost:8080/getData");//请求的端口
        xhr.onload = () => {
          console.log(xhr.response);//放回的数据
        };
        xhr.send();
- XMLHttpRequest的post请求：
const xhr = new XMLHttpRequest;
      xhr.open('post', "http://localhost:8080/login");//请求的端口
      xhr.onload = () => {
        const res = JSON.parse(xhr.response);//返回的数据
        console.log(res)；
      }
      const data = {
        user: userValue.value,
        pwd: pwdValue.value
      }
      xhr.setRequestHeader("content-type", "application/json")//设置http的标头
      xhr.send(JSON.stringify(data))//传参
- fetch的get请求
fetch("/getData", {//设置请求的端口号
        method: "get"//设置请求的方式
      }).then((res) => {
        console.log(res)
        return res.text();//获取到请求属性
      }).then((data) => {
        console.log(JSON.parse(data));//返回的数据
      })
- fetch的post请求
const data = {
        username: "h",
        passwd: "123",
      };
fetch("/login", {//设置请求的端口号
        method: "post",//设置请求的方式
        body: JSON.stringify(data),//传参
        headers: {
          "content-type": "application/json"//设置请求头
        }
      }).then((res) => {
        console.log(res)
        return res.text();//获取到请求属性
      }).then((data) => {
        console.log(JSON.parse(data));;//返回的数据
      })
### axios的用法
- 两种原生调用后端接口的方法对于我们来说还是不够简便的，相比较axios下，axios的简洁性就特别突出了，下面是axios的一些用法：
- get请求
函数调用
async function get() {
            const res = await axios({
                url: "/get",//设置请求的地址
            })
            console.log(res)//返回的数据
        }
对象调用
async function get() {
            const res = await axios.get("/get");
            console.log(res);//返回的数据
        }
- post请求
函数调用
async function post() {
const data = {
                name: "jack",
                age: 19
            }
            const res = await axios({
                url: "/post",//设置请求的地址
                method: 'post',,//设置请求的方式
                data//传参
            })
            console.log(res)//返回的数据
        }

对象调用
 async function post() {
const data = {
                name: "jack",
                age: 19
            }
            const res = await axios.post("/post",data);//传参
            console.log(res)
        }
### axios拦截器
- 请求拦截
axios.interceptors.request.use(function (config) {
            console.log(config);
            return config;//必须返回出去，不然请求不到对应的接口
        })
- 响应拦截
axios.interceptors.response.use(function (response) {
            console.log(response);
            return response;//必须返回出去,不然响应拦截的接口会返回undefined
        })
- 多个请求的顺序是从最晚定义的请求向最先定义的请求逐步调用，然而多个响应则是最先定义的响应最先调用
- 自定义axios
定义生产环境和开发环境域名和端口号
const baseURLMap = {
    [DEV]: "http://localhost:8080",
    [PROD]: "http://localhost:8081",
}

const url = DEV;

const myAxios = axios.create({
    baseURL: baseURLMap[v],//端口号
    timeout: 4000//设置超时
})
- 设置拦截器
- 请求拦截
myAxios.interceptors.request.use((config) => {
  return config;
});
- 统一处理错误
- 响应拦截
myAxios.interceptors.response.use(
  (response) => {
    return response;
  },
  (err) => {
    console.log(err.response);
    if (err.response.status === 401) {
      alert("please go to login ");
    }
    return Promise.reject(err);
  }
);
### axios源码阅读
- get和post源码解析
- 首先会在代码中定义一个Axios的函数，通过原型的方式在原型链上定义一个request方法,该方法是其中最重要的部分:
function Axios(instanceConfig) {
  this.defaults = instanceConfig;
  this.interceptors = {
    request: new InterceptorManager(),
    response: new InterceptorManager()
  };
}
- request方法有点类似中间件，通过这个方法，当我们调用get和post时会调用request函数设置请求的别名，
- get方法
utils.forEach(['delete', 'get', 'head', 'options'], function forEachMethodNoData(method) {
  /*eslint func-names:0*/
  Axios.prototype[method] = function(url, config) {
    return this.request(mergeConfig(config || {}, {
      method: method,
      url: url
    }));
  };
});
- post方法
utils.forEach(['post', 'put', 'patch'], function forEachMethodWithData(method) {
  /*eslint func-names:0*/
  Axios.prototype[method] = function(url, data, config) {
    return this.request(mergeConfig(config || {}, {
      method: method,
      url: url,
      data: data
    }));
  };
});
- 拦截器源码解析
function InterceptorManager() {
  this.handlers = [];//定义一个存放方法的数组
}
- 通过use来添加拦截的方法
InterceptorManager.prototype.use = function use(fulfilled, rejected) {

  this.handlers.push({
    fulfilled: fulfilled,
    rejected: rejected
  });
  return this.handlers.length - 1;
};
- 通过eject 来删除拦截的方法
InterceptorManager.prototype.eject = function eject(id) {
  if (this.handlers[id]) {
    this.handlers[id] = null;
  }
};
- 通过forEach来调用添加的别名，对应到get和post的源码实现
InterceptorManager.prototype.forEach = function forEach(fn) {
  utils.forEach(this.handlers, function forEachHandler(h) {
    if (h !== null) {
      fn(h);
    }
  });
};
module.exports = InterceptorManager;