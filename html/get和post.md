  我们知道，从一个页面转向另一个页面的请求方式有两种，那就是：Post和Get. 
    二者有如下的区别: 
    首先：Post传输数据时，不需要在URL中显示出来，而Get方法要在URL中显示。 
    其次：Post传输的数据量大，可以达到2M，而Get方法由于受到URL长度限制,只能传递大约1024字节. 
    再次：Post就是为了将数据传送到服务器段,Get就是为了从服务器段取得数据.而Get之所以也能传送数据,只是用来设计告诉服务器,你到底需要什么样的数据.Post的信息作为http请求的内容，而Get是在Http头部传输的。 
    
    Get和Post方法的区别
Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息.

我们看看GET和POST的区别

1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456.  POST方法是把提交的数据放在HTTP包的Body中.

2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.

3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。

4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.


[](http://www.cnblogs.com/bukudekong/archive/2014/07/09/3834020.html)