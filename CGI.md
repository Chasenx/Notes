# CGI

## 简介

- Common Gateway Interface （通用网关协议）

- 主要用于 `Apache` 等 `web` 服务器对动态资源的请求，当客户端请求这个资源的时候， `web` 服务器不是直接把这个文件作为静态资源来返回，而是执行这个程序，把标准输出返回给客户端

- 一种古老的技术，可以实现 `C/C++` 的 web 编程

## Apache 配置

- 一般是通过 `virtualhost` 来配置web，所有的端口需要先在 `ports.conf` 里面声明 Listen 80
- 可以直接设置一个文件夹作为 `cgi` 程序，`ScriptAlias "/cgi-bin/" "/var/www/cgi-bin/"`

- 也可以通过后缀名来判断

```bash
<VirtualHost *:9999>
    AddHandler cgi-script .cgi
    <Directory "/var/www/cgi-bin/">
#       Require all granted
        Options +ExecCGI
        AddHandler cgi-script .cgi
    </Directory>
</VirtualHost>
```

## 基本程序

- C++ 版本
```c++
include<stdio.h>
int main() {
	printf("Content-type: text/html\r\n\r\n"); // 这句话一定要，然后要空一行
	printf("<html>");
	printf("<head>");
	printf("<meta charset=\"utf-8\">");
	printf("<title>Hello World</title>");
	printf("</head>");
	printf("body");
	printf("<h1>I am first CGI!!!</h1>");
	printf("</body>");
	printf("</html>");
	return 0;
}
```
- Bash 版本
```bash
#!/bin/bash
echo -e "Content-type: text/html\r\n"  # -e 表示有转义，echo会在后面自己换行
echo "<h1>My second CGI!!!!!</h1>"
```

## 请求内容

|     变量名      | 描述                                                         |
| :-------------: | :----------------------------------------------------------- |
|  CONTENT_TYPE   | 这个环境变量的值指示所传递来的信息的MIME类型。目前，环境变量CONTENT_TYPE一般都是：application/x-www-form-urlencoded,他表示数据来自于HTML表单。 |
| CONTENT_LENGTH  | 如果服务器与CGI程序信息的传递方式是POST，这个环境变量即使从标准输入STDIN中可以读到的有效数据的字节数。这个环境变量在读取所输入的数据时必须使用。 |
|   HTTP_COOKIE   | 客户机内的 COOKIE 内容。                                     |
| HTTP_USER_AGENT | 提供包含了版本数或其他专有数据的客户浏览器信息。             |
|    PATH_INFO    | 这个环境变量的值表示紧接在CGI程序名之后的其他路径信息。它常常作为CGI程序的参数出现。 |
|  QUERY_STRING   | 如果服务器与CGI程序信息的传递方式是GET，这个环境变量的值即使所传递的信息。这个信息经跟在CGI程序名的后面，两者中间用一个问号'?'分隔。 |
|   REMOTE_ADDR   | 这个环境变量的值是发送请求的客户机的IP地址，例如上面的192.168.1.67。这个值总是存在的。而且它是Web客户机需要提供给Web服务器的唯一标识，可以在CGI程序中用它来区分不同的Web客户机。 |
|   REMOTE_HOST   | 这个环境变量的值包含发送CGI请求的客户机的主机名。如果不支持你想查询，则无需定义此环境变量。 |
| REQUEST_METHOD  | 提供脚本被调用的方法。对于使用 HTTP/1.0 协议的脚本，仅 GET 和 POST 有意义。 |
| SCRIPT_FILENAME | CGI脚本的完整路径                                            |
|   SCRIPT_NAME   | CGI脚本的的名称                                              |
|   SERVER_NAME   | 这是你的 WEB 服务器的主机名、别名或IP地址。                  |
| SERVER_SOFTWARE | 这个环境变量的值包含了调用CGI程序的HTTP服务器的名称和版本号。例如，上面的值为Apache/2.2.14(Unix) |
- 请求是通过环境变量获得的
- c 语言使用 `char * getenv(const char *name);` 头文件：`#include <stdlib.h>`
- python 使用 `cgi` 模块提供的函数或者 `os` 模块使用系统变量
- bash 可以直接使用环境变量 `$PATH`

