当项目中需要引入一个当独的页面的时候，可以使用iframe实现。  
主页面和子页面的通信：  
1.主页面和子页面同源，主页面可以通过window.frames[子页面的iframename]来获取子页面的window对象。也可以通过getElementById(子页面的iframeid).contentWindow来获取window对象。  
子页面可以通过parent属性来获取父页面window对象（注意：如果没有父页面，parent即为当前页面的window对象。所以可以通过parent===window来判断是否有没父副页面）  
2.主页面和子页面同顶级域名，当时二级域名不同。  
这个情况：主页面访问子页面的方法会报错。但是子页面调用父方法正常.  
解决方案：主页面和子页面设置相同的document.domain  
3.主页面跟子页面不同源  
1.通过子页面监听hashchange事件，父页面修改子页面hash的方式  
通过中间层

    2. 通过postMessage方式。第一个参数是传输数据、第二个是origin.

指明目标窗口的源，协议+主机+端口号[+URL]，URL会被忽略.设置为*代表所有都可以接受.注意：要传递数据，必须获取iframe的window对象。  
3.window.name  
4.再调套多一层iframe做中间层。