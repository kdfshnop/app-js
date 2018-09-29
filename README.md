# h5页面与app的交互;
## android与app交互;
```
此为js调用安卓方法;
public void addJavascriptInterface(Object object, String name);
1.object参数：在object对象里面添加我们想要在JS里面调用的Android方法，下面的代码中我们调用了showToast方法
2.name参数：这里的name就是我们可以在JS里面调用的对象名称，对应下面代码中的JSTest;

对应的JS代码:
function jsJava(){
    <!--调用java的方法，顶级对象，java方法-->
    <!--可以直接访问JSTest，这是因为JSTest挂载到js的window对象下了-->
    <!--判断对象是否存在-->
    if(window.JSTest){
        JSTest.showToast("我是被JS执行的Android代码");
    } 
}
```
```
//在Android代码中调用此方法
function showInfoFromApp(msg) {
    alert("来自App的信息：" + msg);
}
```
## IOS与app交互;
```
在开发过程中，iOS 中实现加载 web 页面主要有两种控件，UIWebView 和 WKWebview，两种控件对应具体的实现方法不同。WKWebView 是苹果在iOS 8中引入的新组件，目的是提供一个现代的支持最新Webkit功能的网页浏览控件，摆脱过去 UIWebView的老、旧、笨，特别是内存占用量巨大的问题。它使用与Safari中一样的Nitro JavaScript引擎，大大提高了页面js执行速度。 
相比于UIWebView的优势： 
在性能、稳定性、占用内存方面有很大提升； 
允许JavaScript的Nitro库加载并使用（UIWebView中限制） 
增加加载进度属性：estimatedProgress，不用在自己写假进度条了 
支持了更多的HTML的属性
具体分析WKWebView的优劣势 
1.内存占用是UIWebView的1/4~1/3 
2.页面加载速度有提升，有的文章说它的加载速度比UIWebView提升了一倍左右。 
3.更为细致地拆分了 UIWebViewDelegate 中的方法 
4.自带进度条。不需要像UIWebView一样自己做假进度条（通过NJKWebViewProgress和双层代理技术实现），技术复杂度和代码量，根贴近实际加载进度优化好的多。 
5.允许JavaScript的Nitro库加载并使用（UIWebView中限制） 
6.可以和js直接互调函数，不像UIWebView需要第三方库WebViewJavascriptBridge来协助处理和js的交互。 
7.不支持页面缓存，需要自己注入cookie,而UIWebView是自动注入cookie。
8.无法发送POST参数问题
```
```
WKWebView:
    //OC调用JS的方法列表
    function alertMobile() {
        document.getElementById('mobile').innerHTML = '不带参数'
    }

    function alertName(msg) {
        //有一个参数
        document.getElementById('name').innerHTML = '有一个参数 :' + msg
    }

    function alertSendMsg(num,msg) {
        //有两个参数
        document.getElementById('msg').innerHTML = '有两个参数:' + num + ',' + msg + '!!'
    }
    //JS响应方法列表
    function btnClick1() {
        window.webkit.messageHandlers.showMobile.postMessage(null)
    }
    window.webkit.messageHandlers.<方法名>.postMessage(<数据>)
    function btnClick2() {
        window.webkit.messageHandlers.showName.postMessage('有一个参数')
    }
    function btnClick3() {
        window.webkit.messageHandlers.showSendMsg.postMessage(['两个参数One', '两个参数Two'])
    }
```