1、CORS(跨域资源共享，Cross-origin resource sharing)

需要浏览器和服务端同时支持(前端基本上没有参与)。当浏览器检测到有跨域请求(主要是ajax/fetch)时，会自动添加一些请求头字段(可能还有一次预检请求)，支持CORS的服务器能处理这些请求头，并返回响应的响应头，这个时候就可以跨域通信了。

两种请求：

1）简单请求：

满足下列条件的请求视为简单请求：

- 请求方法是GET、POST、HEAD之一；
- 请求头字段不得人为设置[CORS 安全的首部字段集合](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)之外的字段(通常浏览器自动添加的字段不会超出限制)；
- Content-Type限于application/x-www-form-urlencoded、multipart/form-data、text/plain三者之一；

当一个跨域的简单请求发起时，浏览器会为该次请求的请求头添加origin字段，指明本次请求的源。支持CORS的服务器则会在响应头里添加Access-Control-Allow-Origin字段，指明允许跨域的源(也可以是*)。当origin与Access-Control-Allow-Origin匹配，或者Access-Control-Allow-Origin为*，则允许跨域访问。

请求字段：

- origin：指明本次请求来源(协议 + 域名 + 端口);

响应字段：

- Access-Control-Allow-Origin：指明允许跨域的源地址或者是*；

2）非简单请求：

简单请求之外的请求都是非简单请求(通常有自定义字段的请求就是非简单请求)。与简单请求不同的是，非简单请求在发送正式请求之前会发起一次预检间请求。预检请求的请求方法是OPTIONS，并会带有origin、Access-Control-Request-Method、Access-Control-Request-Headers字段。支持CORS的服务器则会返回Access-Control-Allow-Origin、Access-Control-Allow-Methods、Access-Control-Allow-Headers字段。若响应的字段都匹配，则允许跨域。

请求字段：

- origin：同上；
- Access-Control-Request-Method：指明本次CORS请求使用的HTTP方法；
- Access-Control-Request-Headers：指明本次CORS请求将会发送的额外请求头字段，多个用逗号分隔；

响应字段：

- Access-Control-Allow-Origin：同上；

- Access-Control-Allow-Methods：指明服务器支持的所有跨域请求的HTTP方法，多个用逗号分隔；

- Access-Control-Allow-Headers：指明服务器允许跨域请求携带的额外请求头字段；

- Access-Control-Expose-Headers：可选，指明允许浏览器访问的响应头字段的白名单，多个用逗号分隔(通常XHR的getResponseHeader方法只能拿到有限的响应头字段，需要取额外的响应头字段时，就需要概字段)；

- Access-Control-Max-Age：可选，指明预检请求的结果在多少秒内有效；

- 附带身份凭证的请求：

  通常浏览器不会为跨域请求发送身份凭证信息(cookie)，需要设置特殊标志位来让浏览器发送身份凭证信息。ajax中需要将withCredentials属性设为true，而fetch中需要将credentials属性设为'include'。

  注意：对于携带身份凭证的请求，不允许Access-Control-Allow-Origin字段为*。

  请求字段：

  - cookie：身份凭证信息；

  响应字段：

  - Access-Control-Allow-Credentials：该值为true则标识服务器允许浏览器发送身份凭证；

2、JSONP(JSON with Padding)

利用<script>的src可以跨域的特点来跨域，只适用于GET请求。

3、postMessage

适用于多窗口的情况(比如主窗口与iframe)，利用postMessage接口，以及message事件来通信。