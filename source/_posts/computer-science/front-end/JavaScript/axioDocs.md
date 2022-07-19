---
title: axios
date: 2022-04-08 21:47:19
categories:
 - [计算机科学,前端技术,JavaScript]
tags: 
 - 前端技术
 - 软件技术
 - 编程技术
 - axios
---

# 什么是axios?

Axios是一个基于promise的HTTP库，可以用在浏览器和node.js中。

## 特性
- 从浏览器中创建[XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从node.js创建[http](http://nodejs.org/api/http.html)请求
- 支持[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防御[XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

## 安装

使用npm:
```bash
$ npm install axios
```

使用bower
```bash
$ bower install axios
```

使用cdn
```html
<script src="https://unpkg.com/axios/dist/axios.min.js"><script>
```

## 案例
执行`GET`请求
```javascript
//为给定ID的user创建请求
axios.get('/user?ID=12345')
    .then(function(response){
        console.log(response);
    })
    .catch(function(error){
        console.log(error);
    });
//也可以这样做

axios.get('/user',{
    params: {
        ID: 12345
    }
})
.then(function(response){
    console.log(response);
})
.catch(function(error){
    console.log(error);
});
```

执行`POST`请求
```javascript
axios.post('/user',{
    firstName: 'Fred',
    lastName: 'Flintstone'
})
.then(function(response){
    console.log(response);
})
.catch(function(error){
    console.log(error);
})
```
执行多个并发请求
```javascript
function getUserAccount(){
    return axio.get('/user/12345');
}

function getUserPermissions(){
    return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
    .then(axios.spread(function(acct, perms){
        //两个请求现在都执行完成
    }));
```

## axios API

可以通过向`axios`传递相关配置来创建请求
```javascript axios(config)
//发送POST请求
axios({
    methods: 'post',
    url: '/user/12345',
    data: {
        firstName: 'Fred',
        lastName: 'Flintstone'
    }
});
```
```javascript
//获取远端图片
axios({
    method: 'get',
    url: 'http://bit.ly/2mTM3nY',
    responseType: 'stream'
})
    .then(function(response){
        response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
    });
```
### axios(url[,config])

```javascript
//发送GET请求(默认的方法)
axios('/user/12345');
```

## 请求方法的别名

指定的配置将与实例的配置合并。

**axios#request(config)**
**axios#get(url[,config])**
**axios#delete(url[,config])**
**axios#head(url[,config])**
**axios#options(url[,config])**
**axios#post(url[,data[,config]])**
**axios#put(url[,data[,config]])**
**axios#patch(url[,data[,config]])**

## 请求配置

这些是创建请求时可以用的配置选项。只有`url`是必需的。如果没有指定`method`, 请求将默认使用`get`方法。
```javascript
{
    //url是用于请求的服务器URL
    url: '/user',
    //method是创建请求时使用的方法
    method: 'get', //default

    //baseURL将自动加载url前面，除非url是一个绝对url
    //可以通过设置一个baseURL便于为axios实例的方法传递相对URL
    baseURL: 'https://some-domain.com/api/',

    //transformRequest允许向服务器发送前，修改请求数据
    //只能用在'PUT', 'POST'和'PATCH'这几个请求方法
    //后面数组中的函数必须一个字符串，或ArrayBuffer，或Stream
    transformRequest: [function(data, headers){
        //对data进行任意转换处理
        return data;
    }],

    //headers是即将被发送的自定义请求头
    headers: {'X-Requested-With': 'XMLHttpRequest'},

    //params是即将与请求一起发送的URL参数
    //必须是一个无格式对象(plain object)或URLSearchParams对象
    params: {
        ID: 12345
    },

    //paramsSerializer是一个负责params序列化的函数
    paramsSerialzer: function(params){
        return Qs.stringify(params, {arrayFormat: 'brackets'})
    },

    //data是作为请求主体被发送的数据
    //只适用于这些请求方法'PUT', 'POST'和'PATCH'
    //在没有设置transformRequest'时，必须是以下类型之一:
    //- string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
    // 浏览器专属: FormData, File, Blob
    // Node 专属: Stream
    data: {
        firstName: 'Frea'
    },

    //timeout: 指定请求超时的毫秒数(0表示无超时时间)
    //如果请求花费了超时timeout的时间，请求将被中断

    timeout: 1000,

    //withCredentials 表示跨域请求时是否需要使用凭证
    withCredentials: false, //default

    //adapter允许自定义处理请求，以使测试更轻松
    //返回一个promise并应用一个有效的响应
    adapter: function(config){

    },

    // auth表示应该使用HTTP基础验证，并提供凭据
    //这将设置一个Authorization头，覆写掉现有的任意使用headers设置的自定Authorization
    auth: {
        username: 'janedoe',
        password: 's00pers3cret'
    }

    //responseType表示服务器响应的数据类型, 可以是arraybuffer，blob
    responseType: 'json', //default
    
    responseEncoding: 'utf8', //default

    // xsrfCookieName 是用作xsrf token的值的cookie的名称
    xsrfCookieName: 'XSRF-TOKEN',

    //xsrfHeaderName is the name of http header that carries the xsrf token
    xsrfHeaderName: 'X-XSRF-TOKEN',

    //onUploadProgress 允许为上传处理进度事件
    onUploadProgress: function(progressEvent){

    }

    // onDownloadProgress 允许为下载处理进度事件
    onDownloadProgress: function(progressEvent){
        //对原生进度事件的处理
    },

    //maxContentLength 定义允许的响应内容的最大尺寸
    maxContentLength: 2000,

    // validateStatus 定义对于给定的HTTP响应状态码是resolve或reject promise
    validateStatus: function(status){
        return status >= 200 && status < 300; //default
    },

    //maxRedirects 定义在node.js中follow的最大重定向数目
    //如果设置为0，将不会follow任何重定向

    maxRedirects: 5;

    socketPath: null,

    httpAgent: new http.Agent({keepAlive: true}),
    httpsAgent: new https.Agent({keepAlive: true}),

    //'proxy'定义代理服务器的主机名称和端口
    // autn 表示HTTP基础验证应当用于连接代理，并提供凭据
    // 这将会设置一个Proxy-Authorization头，覆写掉已有的通过使用header设置
    proxy: {
        host: '127.0.0.1',
        port: 9000,
        auth: {
            username: 'mikeymike',
            password: 'rapuz31'
        }
    },

    // cancelToken 指定用于取消请求的cancel token
    // 

    cancelToken: new CancelToken(function(cancel){

    })
}
```

## 响应结构

某个请求的响应包含以下信息
```javascript
{
    //data: 由服务器提供的响应
    data: {},
    //status: 来自服务器响应的HTTP状态信息
    status: 200,

    //statusText: 来自服务器响应的HTTP状态信息
    statusText: 'OK',

    //headers 服务器响应的头
    headers: {},

    //config 为请求提供的配置信息
    config: {},
    
    request: {}
}
```

使用`then`时，将收到以下响应:
```javascript
axios.get('/user/12345')
    .then(function(response){
        console.log(response.data);
        console.log(response.status);
        console.log(response.statusText);
        console.log(response.headers);
        console.log(response.config);
    });
```

在使用`catch`时，或传递rejection callback作为`then`的第二个参数时，响应可以通过`error`对象可被使用。

## 配置默认值

### 全局的axios默认值

```javascript
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

### 自定义实例默认值

```javascript
const instance = axios.create({
    baseURL: 'https://api.example.com'
});

instance.defaults.headers.common['Authorization']=AUTH_TOKEN;
```

### 配置的优先顺序

配置会以一个优先顺序进行合并，这个顺序是: 在`lib/defaults.js`找到库的默认值，然后是实例的`defaults`属性，最后是请求的`config`参数。后者将优先于前者。

```javascript
//使用库提供的配置的默认值来创建实例
var instance = axios.create();

//覆写库的超时默认值
instance.defaults.timeout=2500;

//为已知需要花费很长时间的请求覆写超时设置
instance.get('/longRequest',{
    timeout: 5000
});
```

## 拦截器

在请求或响应被`then`或`catch`处理前拦截他们

```javascript
//添加请求拦截器
axios.insterceptors.request.use(function(config){
    //在发送请求前做些什么
    return config;
},function(error){
    //对请求错误做些什么
    return Promise.reject(error);
});

//添加响应拦截器
axios.interceptors.response.user(function(response){
    //对响应数据做点什么
    return response;
}, function(error){
    //对响应错误做点什么
    return Promise.reject(error);
});
```

移除拦截器:
```javascript
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```

自定义axios实例添加拦截器
```javascript
const instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```

## 错误处理

```javascript
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // The request was made and the server responded with a status code
      // that falls out of the range of 2xx
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // The request was made but no response was received
      // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
      // http.ClientRequest in node.js
      console.log(error.request);
    } else {
      // Something happened in setting up the request that triggered an Error
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```

Y可以使用`validateStatus`配置选项定义一个自定义HTTP状态码的错误范围。
```javascript
axios.get('/user/12345',{
    validateStatus: function(status){
        return status < 500;
    }
})
```

## 取消

可以使用`CancelToken.source`工厂方法创建cancel token，
```javascript
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function(thrown) {
  if (axios.isCancel(thrown)) {
    console.log('Request canceled', thrown.message);
  } else {
     // 处理错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求（message 参数是可选的）
source.cancel('Operation canceled by the user.');
```

或者通过传递一个executor函数到`CancelToken`的构造函数来创建cancel token:
```javascript
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // executor 函数接收一个 cancel 函数作为参数
    cancel = c;
  })
});

// cancel the request
cancel();
```

## 使用 application/x-www-form-urlencoded format

默认情况下，axios将JavaScript对象序列化为JSON。要以 要以application / x-www-form-urlencoded格式发送数据，您可以使用以下选项之一。

### 浏览器
在浏览器中，可以使用URLSeachParamsAPI。

```javascript
const params = new URLSearchParams();
params.append('param1', 'value1');
params.append('param2', 'value2');
axios.post('/foo', params);
```
或者使用qs库编码数据:
```javascript
const qs = require('qs');
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```

再者(ES6):
```javascript
import qs from 'qs';
const data = { 'bar': 123 };
const options = {
  method: 'POST',
  headers: { 'content-type': 'application/x-www-form-urlencoded' },
  data: qs.stringify(data),
  url,
};
axios(options);
```

### Node.js

在node.js中，可以使用querystring模块
```javascript
const querystring = require('querystring');
axios.post('http://something.com/', querystring.stringify({ foo: 'bar' }));
```

