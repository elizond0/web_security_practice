# web_security_practice

## 1. [利用 CSS 注入（无 iFrames）窃取 CSRF 令牌](http://www.freebuf.com/articles/web/162687.html)

## 2. JavaScript 反调试：[JavaScript AntiDebugging Tricks](www.freebuf.com/articles/system/163579.html)

- 主要针对的是如何给代码主动调试增加困难，技术方法：

1. 检测未知的执行环境（只想在浏览器中被执行）；
2. 检测调试工具（例如 DevTools）；
3. 代码完整性控制；
4. 流完整性控制；
5. 反模拟；

### 2.1 函数重定义

- 这是一种最基本也是最常用的代码反调试技术了，修改它原本的功能，并隐藏特定信息或显示伪造的信息，也可以让它显示伪造的信息。例如 console.log 函数，修改原函数进行重定义。

- 实际上，为了控制代码的执行方式，还可以基于上述代码来构建一个代码段，并重定义 eval 函数。把 JavaScript 代码传递给 eval 函数，接下来代码将会被计算并执行。

- @/demo/2.1-Function redefinitions.html

### 2.2 断点

- 很多商业产品会在代码中定义一个无限循环的 debugger 指令，不过某些浏览器会屏蔽这种代码，而有些则不会。这种方法的主要目的就是让那些想要调试你代码的人感到厌烦，因为无限循环意味着代码会不断地弹出窗口来询问你是否要继续运行脚本代码：

```javascript
setTimeout(function() {
  while (true) {
    eval("debugger");
  }
}, 1000);
```

- @/demo/2.2-Breakpoints.html

### 2.3 隐式流完整性控制

- 当尝试对代码进行反混淆处理时，首先会尝试重命名某些函数或变量，但是在 JavaScript 中可以检测函数名是否被修改过，或者说可以直接通过堆栈跟踪来获取其原始名称或调用顺序。

- arguments.callee.caller 可以创建一个堆栈跟踪来存储之前执行过的函数，源代码的混淆程度越强，这个技术的效果就越好。

- @/demo/2.3-Implicit control of flow integrity.html

### 2.4 代理对象

- 代理对象是目前 JavaScript 中最有用的一个工具，这种对象可以帮助我们了解代码中的其他对象，包括修改其行为以及触发特定环境下的对象活动。比如说，可以创建一个代理对象并跟踪每一次 document.createElemen 调用，然后在控制台中记录下相关参数和信息

- 可以利用这些信息并通过拦截某些特定函数来调试代码，

- @/demo/2.4-Proxy objects.html

### 2.5 限制执行环境

- 检测执行环境，location.hostname 和浏览器独有的 window 对象

1. 代码在浏览器中执行，而不是模拟器和 nodejs，
2. 资源服务器地址，而不是 localhost 本地服务器

- @/demo/2.5-Restrictional enviroments.html

## 3. [粘贴板信息 jacking](https://github.com/dxa4481/Pastejacking)

- 兼容性 IE9+，使用 window.selection 和 event.originalEvent.clipboardData 接口修改复制的文本，或屏蔽复制功能

- 可用于网络钓鱼攻击，例如在浏览器显示正常的代码，但是复制的时候进行部分篡改，以执行危险脚本造成损失，尤其是复制到终端执行时

- @/demo/3-clipboard jacking.html

## 4. [弹窗 jacking](https://github.com/dxa4481/windowHijacking)

- window.open 打开的页面有权修改 opener.location，打开网之后，再重定向该网页，具有安全隐患，而且打开后的页面可以通过定时器进行二次修改

- 例如：先跳转到淘宝，然后一定时间后 location 修改成高仿钓鱼网站，然后通过记录账号密码或者结合剪切板进行盗取

- @/demo/4-Window Hijacking.html

## 5. [XSSJacking](https://github.com/dxa4481/XSSJacking)

## 6. 利用[Data URLs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs)执行 js xss

- Data URLs 可以插入文本、base64/图片、HTML 文档标签、以及 js 脚本，升级新版本浏览器可避免在点击 url 钓鱼短链接时就中招。

- 在浏览器地址栏中直接输入 `data:text/html,<script>alert('hi');</script>` 是可以执行 js 脚本的，而处于安全原因(防钓鱼)在 chrome71/Firefox65 版本中，通过 a 标签跳转的方式会触发控制台警告 `Not allowed to navigate top frame to data URL`

- 右键‘在新标签中打开’或者通过 iframe 的方式可以绕过此限制，img 标签的 src 也可以插入脚本，通过右键也可以成功执行代码，直接点击无效。

- @/demo/6-Data URLs Jacking
