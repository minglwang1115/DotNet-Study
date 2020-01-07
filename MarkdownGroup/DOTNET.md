# 1. 使用ASP.Net Core创建Web API
- 创建Web项目
- 添加模型类
> 模型 是一组表示应用管理的数据的类。
- 添加数据库上下文
> 数据库上下文是为数据模型协调 Entity Framework 功能的主类 。 此类由 Microsoft.EntityFrameworkCore.DbContext 类派生而来。

> 注：需要通过NuGet包管理器安装`Microsoft.EntityFrameworkCore.SqlServer`
- 注册数据库上下文
> 在 ASP.NET Core 中，服务（如数据库上下文）必须向依赖关系注入 (DI) 容器进行注册。 该容器向控制器提供服务。在Startup.cs文件中。
- 构建控制器
> 在“添加其操作使用实体框架的 API 控制器”对话框中 ：
在“模型类”中选择“TodoItem (TodoApi.Models)” 。
在“数据上下文类”中选择“TodoContext (TodoAPI.Models)” 。
选择“添加” 。

![capture_20200107144359285.bmp](http://ww1.sinaimg.cn/large/005SzfLuly1ganzv2ikcij30c20g2wez.jpg)
## 参考链接

https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio#overview
# 2. 创建网站并发布
- 创建ASP.NET Web应用程序(可以考虑更换成ASP.NET Core Web应用程序，自动创建Models,View,Controllers)
- 发布流程：选择发布目标IIS => 发布方法选择文件系统
# 3. ServiceStack的使用
> ServiceStack是.Net和Mono的开源框架，相对WCF，MVC及Web API而言它是开发Web服务与Web应用的有力替代品。
## 集成方式一
- 创建网站项目(空网站即可)
- 通过NuGet安装依赖包(注意packages.config)
- 在解决方案下添加类库项目
    - 创建实体类(Model)，请求类要继承接口`IReturn<ResponseClass>`，添加响应类(请求类和响应类可以在同一个文件中定义也可以分开定义)；注意设置路由以及get、set语法糖
    ![capture_20200107153428551.bmp](http://ww1.sinaimg.cn/large/005SzfLuly1gao1ba6z4wj30et0eet8u.jpg)
    - 创建接口类(Interface)
    - 创建接口实现类(Service)，继承ServiceStack.Service类和上步接口
    - 创建服务宿主类(Service.Host),继承ServiceStack.AppHostBase，可以在`Configure`中添加允许跨域请求
    - 在Global.asax(ASP.NET Application)启动服务宿主,`new ServiceHost().Init()`
    > Global.asax 文件，有时候叫做 ASP.NET 应用程序文件，提供了一种在一个中心位置响应应用程序级或模块级事件的方法。你可以使用这个文件实现应用程序安全性以及其它一些任务。
    - 添加网站集成配置，在`Web.config`下添加`<system.webServer>`

![20200107155624.png](http://ww1.sinaimg.cn/large/005SzfLuly1gao21nwk6ej30co0nbaat.jpg)

## 集成方式二
- 安装ServiceStack扩展工具
- 创建项目时直接根据模板进行创建

![20200107160255.png](http://ww1.sinaimg.cn/large/005SzfLuly1gao29vyg2wj30cb0nbq3o.jpg)

## 两种方式对比
> 都包含实体类，接口类和接口实现类可以直接合并成一个实现类，服务宿主类可以直接放到网站项目中，不需要单独创建类库项目

## 服务调用
直接通过ajax就可以调用，注意路由配置方式

## 参考链接
1. [ServiceStack Web Service 创建与调用简单示列](https://www.cnblogs.com/woxpp/p/5012947.html)
2. [ServiceStack官网](https://servicestack.net/)
3. [.asax文件是什么](https://www.cnblogs.com/I-am-Betty/archive/2010/09/06/1819558.html)

# 4. ASP.Net前后端交互方式

## 方式一
aspx文件
> aspx文件的后端代码分成了两部分，一个是*.aspx.cs文件，一个是*.aspx.designer.cs文件(不要乱动)。两个文件其实是同一个类，用partial关键字分成了两个分部类。在*.aspx.designer.cs文件中，包含的是每一个控件的字段声明。在asp.net中，WebForm里每一种控件都是一个类，具体的添加的控件是相应类的一个个实例。比如Button是一个类，拖拽一个Button到布局视图中，这个Button就是Button类的一个实例。如果再拖拽一个Button，则第二个拖拽的Button是第二个实例，两个Button名字不同。在*.aspx.cs文件中存放的是相应控件的事件响应函数。

前后端紧耦合(前端元素+后端事件响应函数)，必须由一人开发，不属于前后端分离。

## 方式二

> 编写HTML文件和ashx文件，通过ajax技术实现HTML文件和ashx文件的交互，即前后端的交互。可以类比成HTML是aspx的前台，ashx是aspx的后台，而ajax起到了aspx事件驱动的机制。

> ashx是Generic Handler（一般处理程序）文件的后缀名，后台对数据库的操作都是写在ashx里面的。一般情况下，一个ashx文件处理一个数据库连接操作请求。

> ajax(Asynchronous Javascript And XML)，是一种技术，在很多场合中使用。这里ajax为前后端的HTML与ashx搭建起了通信的桥梁。

DOM元素触发事件函数，通过ajax发起网络请求，针对单个*.ashx，ashx响应请求，返回数据。

## 总结

ASP.Net Web应用实现前后端分离开发可以通过WebAPI、ServiceStack等方式。

## 参考链接
https://www.cnblogs.com/wangmengdx/p/9723996.html