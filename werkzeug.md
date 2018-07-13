# werkzeug文档读书笔记

### WSGI 环境 eviron

WSGI 环境包含所有用户向应用发送信息。你可以通过它向 WSGI 发送信息，也可以 使用 create_environ() 辅助函数创建一个 WSGI 环境字典。

通常没人愿意直接使用 environ 因为它对字节串是有限制的，而且不提供访问表单数据的方法除非手动解析数据。

### WSGI应用
对于在werkzeug中创建的WSGI应用，都会传入两个参数environ和start_response，environ相当于请求的所有信息，start_response是用于启动response的函数。

### Request
1. Request 封装 environ 并提供只读的方法访问数据。
2. Request is not threadsafe，use locks around calls
3. It's not possible to pickle the request object.
可读的对象有：
* .path 
* .script_root 
* .host 
* .url 
* .method
* .args.keys() 可以显示在url中的查询键值
* .args[key] 返回键对应的值
* .headers同上
通过这个方法可以访问POST/PUT请求提交的数据。

### Response
Response 是一个标准WSGI应用。

1. response可以读写属性。常见的属性有：
    * .status
    * .status_code 这两项互相影响。
    * .content_length
    * .headers
2. The response object can be pickled or copied after freeze() wascalled.
3. It's possible to create copies using copy deepcopy.

### URL routing
通过Map存储 URL rules。将一个包含Rule对象的列表传给Map。
Map 自身的 bind 函数 返回一个MapAdapter对象，用于匹配或者创建domains给当前请求。
MapAdapter.match()可以返回一个(endpoints,args)元组，或者以下三种异常之一：
* NotFound
* MethodNotAllowed
* RequestRedirect

### Rule 格式
Rule 中的字符串 主要是带palceholders的普通URL格式。
placeholder的标准格式为 **<** converter(arguments):name **>**
