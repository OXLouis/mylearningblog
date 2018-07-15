## WSGI介绍
WSGI：全称是Web Server Gateway Interface，WSGI不是服务器，python模块，框架，API或者任何软件，只是一种规范，描述web server如何与web application通信的规范。server和application的规范在PEP 3333中有具体描述。要实现WSGI协议，必须同时实现web server和web application，当前运行在WSGI协议之上的web框架有Torando,Flask,Django

### WSGI server 和 application
WSGI协议主要包括server和application两部分：
1. WSGI server负责从客户端接收请求，将request转发给application，将application返回的response返回给客户端；
2. WSGI application接收由server转发的request，处理请求，并将处理结果返回给server。
3. application中可以包括多个栈式的中间件(middlewares)，这些中间件需要同时实现server与application，因此可以在WSGI服务器与WSGI应用之间起调节作用：对服务器来说，中间件扮演应用程序，对应用程序来说，中间件扮演服务器。

### WSGI要求
1. WSGI 规定每个 python 程序（Application）必须是一个可调用的对象（实现了call 函数的方法或者类），接受两个参数 environ（WSGI 的环境信息） 和 start_response（开始响应请求的函数），并且返回 iterable。
1. environ 和 start_response 由 http server 提供并实现
2. environ 变量是包含了环境信息的字典
3. Application 内部在返回前调用 start_response
4. start_response也是一个 callable，接受两个必须的参数，status（HTTP状态）和 response_headers（响应消息的头）
5. 可调用对象要返回一个值，这个值是可迭代的。

### django中WSGI具体实现：
1. application:

```python
class WSGIHandler(base.BaseHandler): 
   initLock = Lock() 
   request_class = WSGIRequest 
   def __call__(self, environ, start_response): 
   # 加载中间件 
    if self._request_middleware is None: 
         with self.initLock: 
             try: # Check that middleware is still uninitialized. 
                 if self._request_middleware is None: 
                    self.load_middleware() 
             except: # Unload whatever middleware we got 
                    self._request_middleware = None raise          
     set_script_prefix(get_script_name(environ)) # 请求处理之前发送信号   
     signals.request_started.send(sender=self.__class__, environ=environ) 
     try: 
          request = self.request_class(environ)  
     except UnicodeDecodeError: 
           logger.warning('Bad Request (UnicodeDecodeError)',exc_info=sys.exc_info(), extra={'status_code': 400,}
           response = http.HttpResponseBadRequest() 
     else: 
           response = self.get_response(request) 
     response._handler_class = self.__class__ status = '%s %s' % (response.status_code, response.reason_phrase) 
     response_headers = [(str(k), str(v)) for k, v in response.items()] for c in response.cookies.values(): response_headers.append((str('Set-Cookie'), str(c.output(header='')))) 
     # server提供的回调方法，将响应的header和status返回给server     
     start_response(force_str(status), response_headers) 
     if getattr(response, 'file_to_stream', None) is not None and environ.get('wsgi.file_wrapper'): 
          response = environ['wsgi.file_wrapper'](response.file_to_stream) 
     return response
```

2. server:
```python
def run(addr, port, wsgi_handler, ipv6=False, threading=False): 
   server_address = (addr, port) 
   if threading: 
        httpd_cls = type(str('WSGIServer'), (socketserver.ThreadingMixIn, WSGIServer), {}) 
   else: 
        httpd_cls = WSGIServer # 这里的wsgi_handler就是WSGIApplication 
   httpd = httpd_cls(server_address, WSGIRequestHandler, ipv6=ipv6) 
    if threading: 
        httpd.daemon_threads = True httpd.set_app(wsgi_handler)    
     httpd.serve_forever()
```
### 参考资料
[聊聊CGI、FastCGI、WSGI](https://www.jianshu.com/p/1f8aec6f2bd1)
[WSGI是个啥？](https://www.jianshu.com/p/8de20cb0fd81)
`墙裂推荐:` [Python Web开发最难懂的WSGI协议，到底包含哪些内容？](https://www.jianshu.com/p/5d2ec4314a86)
[如何理解Nginx, WSGI, Flask之间的关系](https://blog.csdn.net/lihao21/article/details/52304119)
[Web 应用 & 框架](http://pythonguidecn.readthedocs.io/zh/latest/scenarios/web.html)