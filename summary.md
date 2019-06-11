## 前端知识点总结

### TCP、UDP、HTTP之间的关系

1、TCP/IP是一个协议组，可分为三个层次：网络层、传输层和应用层
在网络层有IP协议、ICMP协议、ARP协议、RARP协议和BOOTP协议
在传输层中有TCP和UDP协议
在应用层有FTP、HTTP、TELNET、SMTP、DNS等协议
因此，HTTP本身就是一个协议,是从web服务器传输超文本到本地浏览器的传送协议

TCP是基于TCP协议实现的网络文本协议，属于传输层。
UDP是和TCP对等的，属于传输层

TCP(Transmission Control Protocol，传输控制协议) 是**基于连接的协议，需要进过三次对话，先建立连接，之后在发送数据**

UDP(User Data Protocol，用户数据报协议) 是**与TCP相对应的协议。它是面向非连接的协议，它不与对方建立连接，而是直接将数据包发送过去，**UDP适用于一次只传送少量数据，对可靠性要求不高的应用环境，比如我们经常使用的“ping”命令，正因为UDP协议没有连接的过程，所以它的通信效率高，但也正因为如此，他的可靠性不如TCP协议高。

### socket是什么？

socket是应用层与TCP/IP协议族通信的中间软件抽象层，它是一组接口。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socker去组织数据以符合指定的协议。

![协议层之间的关系](http://zh.mweb.im/asset/img/set-up-git.gif)

### 说说TCP的传输三次握手，为什么不是两次握手？

为了准确无误的把数据送达目标处，TCP采用了三次握手策略：

*
	发送端首先发送一个带SYN标志的数据包给对方，接受端收到后，回传一个带有SYN/ACK标志的数据包传达确认收到。最后发送端在回传一个带ACK标志的数据包，代表“握手”结束。若在握手中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。
*

有人会困惑为什么要进行三次握手呢（两次确认）？这主要是为了防止已失效的请求连接报文忽然又传送到了，从而产生错误。
假定A向B发送一个连接请求，由于一些原因，导致A发出的连接请求在一个网络节点逗留了比较多的时间。此时A会将此连接请求作为无效处理 又重新向B发起了一次新的连接请求，B正常收到此连接请求后建立了连接，数据传输完成后释放了连接。如果此时A发出的第一次请求又到达了B，B会以为A又发起了一次连接请求，如果是两次握手：此时连接就建立了，B会一直等待A发送数据，从而白白浪费B的资源。 如果是三次握手：由于A没有发起连接请求，也就不会理会B的连接响应，B没有收到A的确认连接，就会关闭掉本次连接

### HTTPS是什么

HTTPS即加密的HTTP,HTTPs并不是新协议，而是HTTP+SSL(TLS)，原本HTTP和TCP直接通信，而加上SSL之后就变成HTTP先和SSL通信，在有SSL和TCP通信。


### 介绍几种常见的状态码
* 2XX:代表请求已成功被服务器接收、理解、并接受
* 200:表示请求已经成功，请求所希望的响应头或数据体将随相应返回
* 201:表示请求成功并且服务器创建了新的资源，且其URI已经随Location头信息返回。
* 3XX:代表需要客户采取进一步的操作才能完成请求，这些状态码用来重定向，后续的请求地址在本次响应的Location域中指明
* 301:被请求的资源已永久移动到新的位置。服务器返回此响应（对GET或HEAD请求的响应）时，会自动将请求者转到新位置
* 302:请求的资源临时从不同的URI响应请求，但请求者应继续使用原有位置来进行以后的请求。
* 304:自从上次请求后，请求的网页为修改过。服务器返回此响应时，不会反回网页内容。
* 4XX:表示请求错误。代表客户端看起来可能发生了错误，妨碍了服务器的处理。
* 401:请求要求身份验证。对于需要登录的网页，服务器可能返回次响应。
* 403:服务器已经理解请求，但是拒绝执行它。与401响应不同的是，身份验证并不能提供任何帮助，而这个请求也不应该被重复提交。
* 404:请求失败，请求所希望得到的资源未被在服务器上发现。
* 5XX:代表服务器在处理请求的过程中有错误或者异常发生，也可能是服务器意识到当前的软硬件无法完成对请求的处理
* 500:服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理
* 503:由于临时的服务器维护或者过载，服务器当前无法处理请求。



### if判断条件的转化问题

* 任意值与布尔值比较，都会将两边的值转化为Number。
* 空数组与空对象都是object类型，直接判断为true。
* 引用类型之间比较的内存地址，不需要进行隐式转换。

### bind的实现原理

bind的作用改变函数执行时上下文this的指向。

bind不为人知的秘密：
1、bind执行时可传入预设参数，达到函数柯理化的效果：
```
	// 柯里化
	function test(x) {
	    return function(y){
	        return x + y;
	    }
	};
	test(1)(2); // 3

	// bind
	function test2(a, b) {
	    return a + b;
	}
	test2.bind(null, 1)(2); // 3
```
2、bind执行后返回函数，若当做构造函数会发生什么？
```
	function Test3(a, b) {
	    this.a = a;
	    this.b = b;
	}
	Test3.prototype.add = function () {
	    return this.a + this.b;
	}

	// 如果不用 bind，正常来说这样处理
	var t1 = new Test3(1, 2);
	t1.add(); // 3, this 指向 t1

	// 使用 bind
	var NewTest3 = Test3.bind(null, 3);
	var t2 = new NewTest3(4);
	t2.add(); // 7, this 指向 t2
```
将绑定的函数当做构造函数使用，bind提供的this指向无效，但是还是可以预设参数。

3、自己实现bind函数
```
Function.prototype.bind = function(context){
	var that = this;
	var args = Array.prototype.slice.call(arguments, 1);
	 var bound = function() {
       if(this instanceof bound) { // 如果是作为构造函数，那么第一个参数 context 就不需要了
            return that.apply(this, args.concat(Array.prototype.slice.call(arguments)));
       } else {
            return that.apply(context, args.concat(Array.prototype.slice.call(arguments))) ;
       }
    }

    // 维护原型关系
    if(this.prototype) {
        bound.prototype = this.prototype;
    }
    return bound;
}
```
### 谈谈预检请求

出于安全考虑，在跨越请求时，为了避免跨域请求对服务器的用户数据产生未预期的影响，浏览器针对非简单的跨域请求先发送一个OPTIONS请求，从而货值服务器是否允许改跨越请求，如果允许就发送带数据的真是请求，不允许泽阻止发送真是请求。

哪些是非简单请求：
1、请求方法不是GET/HEAD/POST
2、POST请求的Content-Type 并非application/x-www-form-urlencode,multipart/form-data,或text/plain
3、请求设置了自定义的header字段

### js实现判断一个变量是否为整数

思路：
1、先判断该数是不是number，不是返回 false,是则继续对1取余，如果余数为0则为整数。
2、使用parseInt取整，然后判断是否与原值相等。

```
function isInt(w){
	if(typeof w !== 'number') return false;
	if(w%1 === 0) return true;
	else return false;
}
```

### 进程和线程是什么

线程是最小的执行单元，而进程有至少一个县城组成。如何调度进程和县城，完全有操作系统决定，程序自己不能决定什么时候执行，执行多长时间。

### 死锁是什么
当两个以上的运算单元，双方都在等待对方停止运行，以获取系统资源，但是没有一方提前退出时，就称为死锁。


### 快速排序的算法
