# 初始化

初始化脚本

```
npm i zh-http-client --save
```

参考资料: axios

# 例子

## get 请求，url参数

```js
httpClient.addApi({
	'zhihu_question': {
		method: 'GET',
		url: 'https://www.zhihu.com/question/<%= question_id %>/',
		responseType: 'document',
	}
});
const response = await httpClient.request('zhihu_question', {question_id: 37644888});
assert.equal(response.status, 200);
```

## get 请求，自定义参数

```js
httpClient.addApi({
	'baidu_search': {
		method: 'GET',
		url: 'http://www.baidu.com/s',
		responseType: 'document',
		params: {
			wd: '<%= wd %>'
		}
	}
});
const response = await httpClient.request('baidu_search', {wd: 'node.js'});
assert.equal(response.status, 200);
```

## post json

```
httpClient.addApi(
	'edit_user' => {
	  method: 'post',
	  url: '/user/<%= userid %>/edit',
	  headers: { 'content-type': 'application/json' },
	  data: {
	    firstName: '<%= first_name %>',
	    lastName: '<%= last_name %>'
	  }
	}
);
const response = await httpClient.request('edit_user', {
	userid: 1000,
	first_name: 'li',
	last_name: 'bai'
});
assert.equal(response.status, 200);
```

## post form

```js
const FormData = require('form-data');
const fs = require('fs');

let form = new FormData();
form.append('file', fs.createReadStream(__dirname + '/README.md'), {
	filename: '111.md'
});

httpClient.addApi(
	'upload' => {
	  method: 'post',
	  url: '/upload',
	  headers: { 'content-type': 'multipart/form-data' },
	  data: form
	}
);

const response = await httpClient.request('upload');
assert.equal(response.status, 200);
```

## post application/x-www-form-urlencoded

```js
const querystring = require('querystring');
httpClient.addApi(
	'edit_user' => {
	  method: 'post',
	  url: '/user/<%= userid %>/edit',
	  headers: { 'content-type': 'application/x-www-form-urlencoded' },
	  data: querystring.stringify({
	    firstName: '<%= first_name %>',
	    lastName: '<%= last_name %>'
	  })
	}
);
const response = await httpClient.request('edit_user', {
	userid: 1000,
	first_name: 'li',
	last_name: 'bai'
});
assert.equal(response.status, 200);
```

## 默认配置

```javascript
httpClient.setDefaultSetting({
	baseURL: 'https://some-domain.com/api/',
	timeout: 1000,
	responseType: 'json',
	maxRedirects: 5,
});
```

# request config

```json
{
  // `url` is the server URL that will be used for the request
  url: '/user',

  // `method` is the request method to be used when making the request
  method: 'get', // default

  // `baseURL` will be prepended to `url` unless `url` is absolute.
  // It can be convenient to set `baseURL` for an instance of axios to pass relative URLs
  // to methods of that instance.
  baseURL: 'https://some-domain.com/api/',

  // `transformRequest` allows changes to the request data before it is sent to the server
  // This is only applicable for request methods 'PUT', 'POST', and 'PATCH'
  // The last function in the array must return a string or an instance of Buffer, ArrayBuffer,
  // FormData or Stream
  // You may modify the headers object.
  transformRequest: [function (data, headers) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `transformResponse` allows changes to the response data to be made before
  // it is passed to then/catch
  transformResponse: [function (data) {
    // Do whatever you want to transform the data

    return data;
  }],

  // `headers` are custom headers to be sent
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` are the URL parameters to be sent with the request
  // Must be a plain object or a URLSearchParams object
  params: {
    ID: 12345
  },

  // `paramsSerializer` is an optional function in charge of serializing `params`
  // (e.g. https://www.npmjs.com/package/qs, http://api.jquery.com/jquery.param/)
  paramsSerializer: function(params) {
    return Qs.stringify(params, {arrayFormat: 'brackets'})
  },

  // `data` is the data to be sent as the request body
  // Only applicable for request methods 'PUT', 'POST', and 'PATCH'
  // When no `transformRequest` is set, must be of one of the following types:
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - Browser only: FormData, File, Blob
  // - Node only: Stream, Buffer
  data: {
    firstName: 'Fred'
  },

  // `timeout` specifies the number of milliseconds before the request times out.
  // If the request takes longer than `timeout`, the request will be aborted.
  timeout: 1000,

  // `withCredentials` indicates whether or not cross-site Access-Control requests
  // should be made using credentials
  withCredentials: false, // default

  // `adapter` allows custom handling of requests which makes testing easier.
  // Return a promise and supply a valid response (see lib/adapters/README.md).
  adapter: function (config) {
    /* ... */
  },

  // `auth` indicates that HTTP Basic auth should be used, and supplies credentials.
  // This will set an `Authorization` header, overwriting any existing
  // `Authorization` custom headers you have set using `headers`.
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` indicates the type of data that the server will respond with
  // options are 'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
  responseType: 'json', // default

  // `responseEncoding` indicates encoding to use for decoding responses
  // Note: Ignored for `responseType` of 'stream' or client-side requests
  responseEncoding: 'utf8', // default

  // `xsrfCookieName` is the name of the cookie to use as a value for xsrf token
  xsrfCookieName: 'XSRF-TOKEN', // default

  // `xsrfHeaderName` is the name of the http header that carries the xsrf token value
  xsrfHeaderName: 'X-XSRF-TOKEN', // default

  // `onUploadProgress` allows handling of progress events for uploads
  onUploadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `onDownloadProgress` allows handling of progress events for downloads
  onDownloadProgress: function (progressEvent) {
    // Do whatever you want with the native progress event
  },

  // `maxContentLength` defines the max size of the http response content in bytes allowed
  maxContentLength: 2000,

  // `validateStatus` defines whether to resolve or reject the promise for a given
  // HTTP response status code. If `validateStatus` returns `true` (or is set to `null`
  // or `undefined`), the promise will be resolved; otherwise, the promise will be
  // rejected.
  validateStatus: function (status) {
    return status >= 200 && status < 300; // default
  },

  // `maxRedirects` defines the maximum number of redirects to follow in node.js.
  // If set to 0, no redirects will be followed.
  maxRedirects: 5, // default

  // `socketPath` defines a UNIX Socket to be used in node.js.
  // e.g. '/var/run/docker.sock' to send requests to the docker daemon.
  // Only either `socketPath` or `proxy` can be specified.
  // If both are specified, `socketPath` is used.
  socketPath: null, // default

  // `httpAgent` and `httpsAgent` define a custom agent to be used when performing http
  // and https requests, respectively, in node.js. This allows options to be added like
  // `keepAlive` that are not enabled by default.
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // 'proxy' defines the hostname and port of the proxy server
  // Use `false` to disable proxies, ignoring environment variables.
  // `auth` indicates that HTTP Basic auth should be used to connect to the proxy, and
  // supplies credentials.
  // This will set an `Proxy-Authorization` header, overwriting any existing
  // `Proxy-Authorization` custom headers you have set using `headers`.
  proxy: {
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` specifies a cancel token that can be used to cancel the request
  // (see Cancellation section below for details)
  cancelToken: new CancelToken(function (cancel) {
  })
}
```

# Response Schema

The response for a request contains the following information.

```json
{
  // `data` is the response that was provided by the server
  data: {},

  // `status` is the HTTP status code from the server response
  status: 200,

  // `statusText` is the HTTP status message from the server response
  statusText: 'OK',

  // `headers` the headers that the server responded with
  // All header names are lower cased
  headers: {},

  // `config` is the config that was provided to `axios` for the request
  config: {},

  // `request` is the request that generated this response
  // It is the last ClientRequest instance in node.js (in redirects)
  // and an XMLHttpRequest instance the browser
  request: {}
}
```

# 错误

```js
httpClient.request('edit_user',{})
  .catch(function (error) {
    if (error.response) {
      // 服务器返回，但是状态码不是 2**
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 没有收到服务器的返回，error.request 是 XMLHttpRequest 或 http.ClientRequest 的一个实例
      console.log(error.request);
    } else {
      // 设置触发错误的请求时发生了一些情况
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```