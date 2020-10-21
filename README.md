# HiRequest 【hi-got】

为了方便以及统一大家对于数据请求的方式，HiUI 特封装请求工具 HiRequest

### 快速使用

```javascript
import HiRequest from '@hi-ui/hiui/es/hi-request'

HiRequest.get('/user?ID=12345').then((response) => {
  // handle success
  console.log(response)
})
```

### 支持 AMD/CJS/ESM 模块引入

```js

// ESM
import HiRequest Tool from 'hi-request';

//cjs
const HiRequest = require('hi-request');

// AMD
require(['hi-request'],function(){
  ...
})

// script 脚本
<script src="https://xxx.com/hi-request.min.js"></script>

```

## Get 请求

```javascript
// 简单的 Get 请求示例
// Make a request for a user with a given ID
HiRequest.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response)
  })
  .catch(function (error) {
    // handle error
    console.log(error)
  })
  .then(function () {
    // always executed
  })

// 上面的请求也可以这样做
HiRequest.get('/user', {
  params: {
    ID: 12345
  }
})
  .then(function (response) {
    console.log(response)
  })
  .catch(function (error) {
    console.log(error)
  })
  .then(function () {
    // always executed
  })

// 想要使用 async/await ? 将' async '关键字添加到外部函数/方法中。
async function getUser() {
  try {
    const response = await HiRequest.get('/user?ID=12345')
    console.log(response)
  } catch (error) {
    console.error(error)
  }
}
```

## POST 请求

```js
// 一个简单的post请求
HiRequest({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
})
```

```javascript
HiRequest.post('/user', {
  firstName: 'Fred',
  lastName: 'Flintstone'
})
  .then(function (response) {
    console.log(response)
  })
  .catch(function (error) {
    console.log(error)
  })
```

## 执行多个并发请求

```js
function getUserAccount() {
  return HiRequest.get('/user/12345')
}

function getUserPermissions() {
  return HiRequest.get('/user/12345/permissions')
}

HiRequest.all([getUserAccount(), getUserPermissions()]).then(
  HiRequest.spread(function (acct, perms) {
    // Both requests are now complete
  })
)
```

## Upload 上传文件 请求方法

```js
HiRequest.upload(({
  url: 'https://upload', // 上传地址
  name: 'filename', // 文件参数
  file: '', // 文件
  params: {
    id:1
  }, // 其他参数
  withCredentials:true,
  headers: {
    token:'token'
  },
  onUploadProgress: (event) => {
    // 上传进度
  }
}).then((res) => {
  if (res.status === 200) {
    // 返回结果
  } else {
    onerror(res.response)
  }
}).catch(error => {
  onerror(error.response)
});
```

## Download 下载文件 请求方法

```js
HiRequest.download({
  url: 'https://download', // 上传地址
  filename: '下载文件名', // 文件
  params: {
    id: 1
  }, // 其他参数
  withCredentials: true,
  headers: {
    token: 'token'
  },
  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: (progressEvent) => {
    // 对原生进度事件的处理
  },
  downloadSuccess: (res) => {
    // 下载成功
  },
  downloadFail: (res) => {
    // 下载失败
  }
})
```

```js
// 获取远程图片
HiRequest({
  method: 'get',
  url: 'http://bit.ly/2mTM3nY',
  responseType: 'stream'
}).then(function (response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
})
```

## HiRequest(url[, config],[host])

```js
HiRequest('/user/12345', { method: 'get' }, 'api')
```

## 更多快捷使用方式

##### HiRequest(config)

##### HiRequest.get(url[, config])

##### HiRequest.delete(url[, config])

##### HiRequest.head(url[, config])

##### HiRequest.options(url[, config])

##### HiRequest.post(url[, data[, config]])

##### HiRequest.put(url[, data[, config]])

##### HiRequest.patch(url[, data[, config]])

##### HiRequest.upload(url[, data[, config]])

##### HiRequest.jsonp(url[, data[, config]])

## JSONP 请求

```js
HiRequest.jsonp('/users.jsonp')
  .then(function (response) {
    return response.json()
  })
  .then(function (json) {
    console.log('parsed json', json)
  })
  .catch(function (ex) {
    console.log('parsing failed', ex)
  })
```

## 设置 JSONP 回调参数名称，默认为'callback'

```js
HiRequest.jsonp('/users.jsonp', {
  jsonpCallback: 'custom_callback'
})
  .then(function (response) {
    return response.json()
  })
  .then(function (json) {
    console.log('parsed json', json)
  })
  .catch(function (ex) {
    console.log('parsing failed', ex)
  })
```

## 设置 JSONP 回调函数名称，默认为带 json\_前缀的随机数

```javascript
HiRequest.jsonp('/users.jsonp', {
  jsonpCallbackFunction: 'function_name_of_jsonp_response'
})
  .then(function (response) {
    return response.json()
  })
  .then(function (json) {
    console.log('parsed json', json)
  })
  .catch(function (ex) {
    console.log('parsing failed', ex)
  })
```

## 设置 JSONP 请求超时，默认为 5000ms

```javascript
HiRequest.jsonp('/users.jsonp', {
  timeout: 3000
})
  .then(function (response) {
    return response.json()
  })
  .then(function (json) {
    console.log('parsed json', json)
  })
  .catch(function (ex) {
    console.log('parsing failed', ex)
  })
```

### `jsonpCallback`和之间的区别`jsonCallbackFunction`

这两个功能可以很容易地相互混淆，但是有一个明显的区别。

默认值为

- `jsonpCallback`，默认值为`callback`。这是回调参数的名称
- `jsonCallbackFunction`，默认值为`null`。这是回调函数的名称。为了使其与众不同，它是一个`jsonp_`前缀为的随机字符串`jsonp_1497658186785_39551`。如果由服务器设置，则将其保留为空白；如果回调函数名称是固定的，则将其显式设置。

##### Case 1:

```js
HiRequest.jsonp('/users.jsonp', {
  jsonpCallback: 'cb'
})
```

请求网址将为`/users.jsonp?cb=jsonp_1497658186785_39551`，并且服务器应使用以下函数进行响应：

```js
jsonp_1497658186785_39551(
  { ...data here... }
)
```

##### Case 2:

```js
HiRequest.jsonp('/users.jsonp', {
  jsonpCallbackFunction: 'search_results'
})
```

请求网址将为`/users.jsonp?callback=search_results`，并且服务器应始终使用名为的函数进行响应`search_results`

```js
search_results(
  { ...data here... }
)
```

## HiRequestAPI

```js
{
  // `url` 上传地址必填项
  url: '/user',

  // `method` is the request method to be used when making the request
  method: 'get', // default
  // 添加上传文件方式

  type: 'basics', // 如果是上传就使用upload
  // 当type: 'upload'的时候；需要指定下面的参数
    file?: any, // 需要上传的文件
    name?: string, // 文件参数名称
  // 发送接口时request拦截器  可以是一个函数 也可以是数组，每个数组项都是函数
  beforeRequest: [function (config){
      // 对config进行自定义处理
      return config
  }],
  // 获取结果时；返回页面层面数据时候 拦截器 可以是一个函数 也可以是数组，每个数组项都是函数
  beforeResponse: [function (res){
      // 对返回结果进行自定义处理
      return res
  }],
  // 返回数据异常结果 可以是一个函数 也可以是数组，每个数组项都是函数
  errorResponse: [function (error){
      // 对error进行自定义处理
      console.log(error.response)
  }],
  // 请求异常结果  可以是一个函数 也可以是数组，每个数组项都是函数
  errorRequest: [function (error){
      // 对config进行自定义处理
      console.log(error.request)
  }],
  // 异常结果 返回或者其他异常都会走这个
  errorCallback: function (error){
      // 对config进行自定义处理
      console.log(err,error.request || error.response)
  },
  // 如果url不是绝对地址，' baseURL '将被加在' url '前面。
  // 为HiRequest的一个实例设置' baseURL '可以方便地将相对url传递给该实例的方法。
  baseURL: 'https://some-domain.com/api/',
  // 允许在请求数据被发送到服务器之前对其进行更改
  // 只适用于请求方法 'PUT'， 'POST'， 'PATCH'和'DELETE'
  transformRequest: [function (data, headers) {
    // Do whatever you want to transform the data

    return data;
  }],

  // 允许之前对响应数据进行更改
  transformResponse: [function (data) {
    // Do whatever you want to transform the data

    return data;
  }],

  // 自定义headers
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` 与请求一起发送的URL参数
  // Must be a plain object or a URLSearchParams object
  params: {
    ID: 12345
  },

  data: {
    firstName: 'Fred'
  },

  // 只有值没有key
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` 指定请求超时的毫秒数(0 表示无超时时间)
  // 如果请求话费了超过 `timeout` 的时间，请求将被中断
  timeout: 1000,

   // `withCredentials` 表示跨域请求时是否需要使用凭证
  withCredentials: false, // default

  // `adapter` 允许自定义处理请求，以使测试更轻松
  // 返回一个 promise 并应用一个有效的响应 (查阅 [response docs](#response-api)).
  adapter: function (config) {
    /* ... */
  },

 // `auth` 表示应该使用 HTTP 基础验证，并提供凭据
  // 这将设置一个 `Authorization` 头，覆写掉现有的任意使用 `headers` 设置的自定义 `Authorization`头
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

   // `responseType` 表示服务器响应的数据类型，可以是 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

   // `xsrfCookieName` 是用作 xsrf token 的值的cookie的名称
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

   // `onUploadProgress` 允许为上传处理进度事件
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` 允许为下载处理进度事件
  onDownloadProgress: function (progressEvent) {
    // 对原生进度事件的处理
  },

   // `maxContentLength` 定义允许的响应内容的最大尺寸
  maxContentLength: 2000,

  // `validateStatus` 定义对于给定的HTTP 响应状态码是 resolve 或 reject  promise 。如果 `validateStatus` 返回 `true` (或者设置为 `null` 或 `undefined`)，promise 将被 resolve; 否则，promise 将被 rejecte
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` 定义在 node.js 中 follow 的最大重定向数目
  // 如果设置为0，将不会 follow 任何重定向
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` 和 `httpsAgent` 分别在 node.js 中用于定义在执行 http 和 https 时使用的自定义代理。允许像这样配置选项：
  // `keepAlive` 默认没有启用
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' 定义代理服务器的主机名称和端口
  // `auth` 表示 HTTP 基础验证应当用于连接代理，并提供凭据
  // 这将会设置一个 `Proxy-Authorization` 头，覆写掉已有的通过使用 `header` 设置的自定义 `Proxy-Authorization` 头。
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` 指定用于取消请求的 cancel token
  // （查看后面的 Cancellation 这节了解更多）
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

## Errors 异常处理

```js
HiRequest.get('/user/12345').catch(function (error) {
  if (error.response) {
    // 请求发出后，服务器使用状态码进行响应，该状态码超出了2xx的范围
    console.log(error.response.data)
    console.log(error.response.status)
    console.log(error.response.headers)
  } else if (error.request) {
    // The request was made but no response was received
    // `error.request` is an instance of XMLHttpRequest in the browser and an instance of
    console.log(error.request)
  } else {
    // Something happened in setting up the request that triggered an Error
    console.log('Error', error.message)
  }
  console.log(error.config)
})
```

使用' validateStatus '配置选项，您可以定义应该抛出错误的 HTTP 代码。

```js
HiRequest.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500 // 状态码大于或等于500时，不再进行处理
  }
})
```

使用' toJSON '你得到一个对象更多的信息，关于 HTTP 错误。

```js
HiRequest.get('/user/12345').catch(function (error) {
  console.log(error.toJSON())
})
```

## 取消请求

> [cancelable promises proposal](https://github.com/tc39/proposal-cancelable-promises).

通过使用 `CancelToken.source` 生成一个标记，用作取消使用

```js
const CancelToken = HiRequest.CancelToken
const source = CancelToken.source()

HiRequest.get('/user/12345', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (HiRequest.isCancel(thrown)) {
    console.log('Request canceled', thrown.message)
  } else {
    // handle error
  }
})

HiRequest.post('/user/12345', {
  name: 'new name',
  cancelToken: source.token
})

// 取消这两次请求

source.cancel('Operation canceled by the user.')
```

你也可以通过传递一个 `executor` 函数给 `CancelToken` 构造函数来创建一个取消令牌:

```js
const CancelToken = HiRequest.CancelToken
let cancel

HiRequest.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // An executor function receives a cancel function as a parameter
    cancel = c
  })
})

// cancel the request
cancel()
```

## 使用建议

```js
import HiRequest from '@hi-ui/hiui/es/hi-request'

// 如需在自己项目中使用，一般大家会设置 统一 loading，或者公用的 header 等配置；
// 另外同时也会由 response 的状态码，做一些统一的处理，我们对此提供了一个简单的示例。

const HttpClient = (url, options = {}, host = 'api') => {
  const { headers, ...restOptions } = options
  return HiRequest(
    {
      url,
      headers: {
        token: 'user.token',
        ...headers
      },
      // 发送接口时request拦截器
      beforeRequest: (config) => {
        // 对config进行自定义处理
        return config
      },
      // 获取结果时；返回页面层面数据时候 拦截器
      beforeResponse: (res) => {
        // 对返回结果进行自定义处理
        return res
      },
      // 返回数据异常结果
      errorResponse: (error) => {
        // 对error进行自定义处理
        console.log(error.response)
      },
      // 请求异常结果
      errorRequest: (error) => {
        // 对config进行自定义处理
        console.log(error.request)
      },
      // 异常结果 返回或者其他异常都会走这个
      errorCallback: (error) => {
        // 对config进行自定义处理
        console.log(err, error.request || error.response)
      },
      ...restOptions
    },
    host
  ).then(
    (response) => {
      // if (loading) {
      //   closeLoading()
      // }
      const res = response.data

      if (res.code === 401) {
        // 未登录，处理未登录的一些提醒
        return
      }

      return res
    },
    (error) => {
      // if (loading) {
      //   closeLoading()
      // }
      throw error
    }
  )
}
export default HttpClient
```
