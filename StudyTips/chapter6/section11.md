# nginx+tomcat部署项目JS报错

## 1. 问题描述

项目部署两个月未出问题，无更改，突然报错且功能异常。

页面涉及iFrame的无法显示，且在该区域显示:

```
#谷歌内核的浏览器
XXXX拒绝了我们的连接请求
#IE内核的浏览器
此内容不能在框架中显示
此处应该有内容，但发布者不允许在框架中显示该内容。这是为了保护可能要输入到此站点中的信息的安全性。
尝试此操作
```

页面控制台JS报错

```
1.Refused to execute script from 'XXXX' because its MIME type ('text/html') is not executable, and strict MIME type checking is enabled.
2.Refused to display 'XXXX' in a frame because it set 'X-Frame-Options' to 'deny'.
3.VM3510:168 Uncaught DOMException: Blocked a frame with origin "XXXX" from accessing a cross-origin frame.
    at eval (eval at globalEval (XXXX), <anonymous>:168:53)
```

## 2. 问题排查

根据JS报错，搜索找到导致该问题的最大怀疑项目，nginx配置新增了如下内容：

```
	add_header 'X-XSS-Protection' '1; mode=block';	#此配置未发现影响
	add_header 'X-Frame-Options' 'DENY';			#导致内嵌iFrame不显示
	add_header 'X-Content-Type-Options' 'nosniff';	#导致其他js报错
```

​		经过确认，此配置项为安全漏洞修复手段，但是此配置项可能会导致某些项目功能异常。以下会根据这三项做详细介绍（注：以下内容来自网站[HTTP | MDN Web Docs]( https://developer.mozilla.org/zh-CN/docs/Web/HTTP)）：

### 2.1 X-XSS-Protection

​		HTTP **X-XSS-Protection** 响应头是Internet Explorer，Chrome和Safari的一个功能，当检测到跨站脚本攻击 ([XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS))时，浏览器将停止加载页面。虽然这些保护在现代浏览器中基本上是不必要的，当网站实施一个强大的[`Content-Security-Policy`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy)来禁用内联的JavaScript (`'unsafe-inline'`)时, 他们仍然可以为尚不支持 [CSP](https://developer.mozilla.org/en-US/docs/Glossary/CSP) 的旧版浏览器的用户提供保护。

​		语法格式如下：

```
# 禁止XSS过滤。
X-XSS-Protection: 0
# 启用XSS过滤（通常浏览器是默认的）。 如果检测到跨站脚本攻击，浏览器将清除页面（删除不安全的部分）。
X-XSS-Protection: 1
# 启用XSS过滤。 如果检测到攻击，浏览器将不会清除页面，而是阻止页面加载。
X-XSS-Protection: 1; mode=block
# 启用XSS过滤。 
# 如果检测到跨站脚本攻击，浏览器将清除页面并使用CSP report-uri指令的功能发送违规报告。
X-XSS-Protection: 1; report=<reporting-uri>
```

​		例子如下：

当检测到XSS攻击时阻止页面加载：

```bash
X-XSS-Protection: 1;mode=block
```

PHP

```
header("X-XSS-Protection: 1; mode=block");
```

Apache (.htaccess)

```bash
<IfModule mod_headers.c> 
  Header set X-XSS-Protection "1; mode=block" 
</IfModule>
```

### 2.2 X-Frame-Options

​		The **X-Frame-Options** [HTTP] 响应头是用来给浏览器 指示允许一个页面 可否在 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/frame), [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/iframe), [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/embed) 或者 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/object) 中展现的标记。站点可以通过确保网站没有被嵌入到别人的站点里面，从而避免 [clickjacking](https://zh.wikipedia.org/wiki/clickjacking) 攻击。

​		The added security is only provided if the user accessing the document is using a browser supporting `X-Frame-Options`. [`Content-Security-Policy`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy) HTTP 头中的 [frame-ancestors](https://developer.mozilla.org/en-US/docs/Security/CSP/CSP_policy_directives#frame-ancestors) 指令会[替代](https://www.w3.org/TR/CSP2/#frame-ancestors-and-frame-options)这个非标准的 header。CSP 的 frame-ancestors 会在 Gecko 4.0 中支持，但是并不会被所有浏览器支持。然而 `X-Frame-Options` 是个已广泛支持的非官方标准，可以和 CSP 结合使用。

​		语法格式如下：

```
# 表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
X-Frame-Options: deny
# 表示该页面可以在相同域名页面的 frame 中展示。
X-Frame-Options: sameorigin
# 表示该页面可以在指定来源的 frame 中展示。
X-Frame-Options: allow-from https://example.com/
```

> **Note:** 设置 meta 标签是无效的！例如 `<meta http-equiv="X-Frame-Options" content="deny">` 没有任何效果。不要这样用！只有当像下面示例那样设置 HTTP 头 `X-Frame-Options` 才会生效。

​		在 Firefox 尝试加载 frame 的内容时，如果 X-Frame-Options 响应头设置为禁止访问了，那么 Firefox 会用 about:blank 展现到 frame 中。也许从某种方面来讲的话，展示为错误消息会更好一点。

​	例子如下：

**配置 Apache**

配置 Apache 在所有页面上发送 X-Frame-Options 响应头，需要把下面这行添加到 'site' 的配置中:

```html
Header always set X-Frame-Options "sameorigin"
```

要将 Apache 的配置 `X-Frame-Options` 设置成 deny , 按如下配置去设置你的站点：

```html
Header set X-Frame-Options "deny"
```

要将 Apache 的配置 `X-Frame-Options` 设置成 `allow-from`，在配置里添加：

```html
Header set X-Frame-Options "allow-from https://example.com/"
```

**配置 nginx**

配置 nginx 发送 X-Frame-Options 响应头，把下面这行添加到 'http', 'server' 或者 'location' 的配置中:

```html
add_header X-Frame-Options sameorigin always;
```

**配置 IIS**

配置 IIS 发送 `X-Frame-Options` 响应头，添加下面的配置到 Web.config 文件中：

```xml
<system.webServer>
  ...

  <httpProtocol>
    <customHeaders>
      <add name="X-Frame-Options" value="sameorigin" />
    </customHeaders>
  </httpProtocol>

  ...
</system.webServer>
```

**配置 HAProxy**

配置 HAProxy 发送 `X-Frame-Options` 头，添加这些到你的前端、监听 listen，或者后端的配置里面：

```html
rspadd X-Frame-Options:\ sameorigin
```

或者，在更加新的版本中：

```html
http-response set-header X-Frame-Options sameorigin
```

**配置 Express**

要配置 Express 可以发送 `X-Frame-Options` header，你可以用借助了 [frameguard](https://helmetjs.github.io/docs/frameguard/) 来设置头部的 [helmet](https://helmetjs.github.io/)。在你的服务器配置里面添加：

```js
const helmet = require('helmet');
const app = express();
app.use(helmet.frameguard({ action: "sameorigin" }));
```

或者，你也可以直接用 [frameguard](https://helmetjs.github.io/docs/frameguard/)：

```js
const frameguard = require('frameguard')
app.use(frameguard({ action: 'sameorigin' }))
```

### 2.3 X-Content-Type-Options

​		**X-Content-Type-Options**` 响应首部相当于一个提示标志，被服务器用来提示客户端一定要遵循在 [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 首部中对  [MIME 类型](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 的设定，而不能对其进行修改。这就禁用了客户端的 [MIME 类型嗅探](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#MIME_sniffing)行为，换句话说，也就是意味着网站管理员确定自己的设置没有问题。

​		这个消息首部最初是由微软在 IE 8 浏览器中引入的，提供给网站管理员用作禁用内容嗅探的手段，内容嗅探技术可能会把不可执行的 MIME 类型转变为可执行的 MIME 类型。在此之后，其他浏览器也相继引入了这个首部，尽管它们的 MIME 嗅探算法没有那么有侵略性。

​		站点安全测试人员通常欢迎对这个首部进行设置。

​		注意: `nosniff` 只应用于 "`script`" 和 "`style`" 两种类型。事实证明，将其应用于图片类型的文件会导致[与现有的站点冲突](https://github.com/whatwg/fetch/issues/395)。

​		语法如下：

```
X-Content-Type-Options: nosniff
```

下面两种情况的请求将被阻止：

- 请求类型是"`style`" 但是 MIME 类型不是 "`text/css`"，
- 请求类型是"`script`" 但是 MIME 类型不是  [JavaScript MIME 类型](https://html.spec.whatwg.org/multipage/scripting.html#javascript-mime-type)。

## 3. 问题解决

修改Nginx配置项

```
	add_header 'X-XSS-Protection' '1; mode=block';	#此配置未发现影响
	add_header 'X-Frame-Options' 'sameorigin';			#修改或注释此项
	add_header 'X-Content-Type-Options' 'nosniff';	#目前先注释
```

