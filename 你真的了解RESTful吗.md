前不久，在网上看到一个段子，一个码农去面试，面试官问什么是RESTful API，这看似一个很简单的常识问题，码农却哑巴了。下面来看一下他们的对话：
"""
面试官：了解RESTful吗？
我：听说过。
面试官：那什么是RESTful？
我：就是用起来很规范，挺好的
面试官：是RESTful挺好的，还是自我感觉挺好的
我：都挺好的。
面试官：... 把门关上。
我：.... 要干嘛？先关上再说。
面试官：我说出去把门关上。
我：what ？，夺门而去
是的，如果没有去细细的研究，问我我估计也会这么讲，那究竟什么是RESTful API，它有哪些规范呢？
"""
# 概述
在没有前后端分离概念之前，一个网站的完成总是“all in one”，在这个阶段，页面、数据、渲染全部在服务端完成，这样做的最大的弊端是后期维护，扩展极其痛苦，开发人员必须同时具备前后端知识。 于是后来慢慢的兴起了前后端分离的思想：即后端负责数据编造，而前端则负责数据渲染，前端静态页面调用指定api获取到有固定格式的数据，再将数据展示出来，这样呈现给用户的就是一个”动态“的过程。

而关于api这部分的设计则成了一个问题。如何设计出一个便于理解，容易使用的api则成了一个问题，而所谓的RESTful就是用来规范我们的API的一种约束。

作为REST，其实是Representational State Transfer（表象层状态转变）三个单词的缩写，它由Roy Fielding于2000年论文中提出，它代表着分布式服务的架构风格。

要深刻理解消化Representational State Transfer这三个单词到底意味着什么，可以从以下几个方面进行理解： 1.每一个URI代表一种资源； 2.客户端和服务器之间，传递这种资源的某种表现层； 3.客户端通过四个HTTP动词（get、post、put、delete），对服务器端资源进行操作，实现”表现层状态转化”。

# RESTful6大原则
REST之父Roy Fielding在论文中阐述REST架构的6大基本原则，它们分别是：

## 1. C-S架构
数据的存储在Server端，Client端只需使用就行。两端彻底分离的好处使client端代码的可移植性变强，Server端的拓展性变强。两端单独开发，互不干扰。

## 2. 无状态
http请求本身就是无状态的，基于C-S架构，客户端的每一次请求带有充分的信息能够让服务端识别。请求所需的一些信息都包含在URL的查询参数、header、div，服务端能够根据请求的各种参数，无需保存客户端的状态，将响应正确返回给客户端。无状态的特征大大提高的服务端的健壮性和可拓展性。

当然，这种无状态性的约束也是有缺点的，客户端的每一次请求都必须带上相同重复的信息确定自己的身份和状态，造成传输数据的冗余性，但这种确定对于性能和使用来说，几乎是忽略不计的。

## 3.统一的接口
REST架构的核心内容，统一的接口对于RESTful服务非常重要。客户端只需要关注实现接口就可以，接口的可读性加强，使用人员方便调用。

REST接口约束定义为：资源识别; 请求动作; 响应信息; 它表示通过uri标出你要操作的资源，通过请求动作（http method）标识要执行的操作，通过返回的状态码来表示这次请求的执行结果。

## 4.一致的数据格式
服务端返回的数据格式要么是XML，要么是Json（获取数据），或者直接返回状态码，一些知名网站的开放平台的操作数据的api，post、put、patch都是返回的一个状态码 。

如请求一条微博信息，服务端响应信息应该包含这条微博相关的其他URL，客户端可以进一步利用这些URL发起请求获取感兴趣的信息，再如分页可以从第一页的返回数据中获取下一页的URT也是基于这个原理。

## 5.可缓存
在万维网上，客户端可以缓存页面的响应内容。因此响应都应隐式或显式的定义为可缓存的，若不可缓存则要避免客户端在多次请求后用旧数据或脏数据来响应。管理得当的缓存会部分地或完全地除去客户端和服务端之间的交互，进一步改善性能和延展性。

## 6.按需编码、可定制代码
服务端可选择临时给客户端下发一些功能代码让客户端来执行，从而定制和扩展客户端的某些功能。比如服务端可以返回一些 Javascript 代码让客户端执行，去实现某些特定的功能。提示：REST架构中的设计准则中，只有按需编码为可选项。如果某个服务违反了其他任意一项准则，严格意思上不能称之为RESTful风格。

# 最佳示例
## 1. 版本控制
如github开放平台的API：http://developer.github.com/v3/ 可以发现，一般的项目加版本v1，v2，v3版本号，为的是兼容一些老版本的接口，这个加版本估计只有大公司大项目才会去使用。

## 2.参数命名规范
query parameter可以采用驼峰命名法，也可以采用下划线命名的方式，推荐采用下划线命名的方式，据说后者比前者的识别度要高，其中，做前端开发基本都后后者，而做服务器接口开发基本用前者。

http://example.com/api/users/today_login 获取今天登陆的用户
http://example.com/api/users/today_login&sort=login_desc 获取今天登陆的用户、登陆时间降序排列
## 3.url命名规范
API 命名应该采用约定俗成的方式，保持简洁明了。在RESTful架构中，每个url代表一种资源，所以url中不能有动词，只能有名词，并且名词中也应该使用复数。实现者应使用相应的Http动词GET、POST、PUT、PATCH、DELETE、HEAD来操作这些资源即可

不规范的的url，冗余没有意义，形式不固定，不同的开发者还需要了解文档才能调用。

http://example.com/api/getallUsers       //GET 获取所有用户
http://example.com/api/getuser/1          //GET 获取标识为1用户信息
http://example.com/api/user/delete/1    //GET/POST 删除标识为1用户信息
http://example.com/api/updateUser/1   //POST 更新标识为1用户信息
http://example.com/api/User/add         //POST添加新的用户
规范后的RESTful风格的url，形式固定，可读性强，根据users名词和http动词就可以操作这些资源。

http://example.com/api/users               //GET 获取所有用户信息
http://example.com/api/users/1           //GET 获取标识为1用户信息
http://example.com/api/users/1          //DELETE 删除标识为1用户信息
http://example.com/api/users/1         //Patch 更新标识为1用户部分信息,包含在div中
http://example.com/api/users           //POST 添加新的用户
## 4，统一返回数据格式
对于合法的请求应该返回统一的数据格式，对于返回数据，通常会包含如下字段。 - code——包含一个整数类型的HTTP响应状态码。 - status——包含文本：”success”，”fail”或”error”。HTTP状态响应码在500-599之间为”fail”，在400-499之间为”error”，其它均为”success”（例如：响应状态码为1XX、2XX和3XX）。这个根据实际情况其实是可要可不要的。 - message——当状态值为”fail”和”error”时有效，用于显示错误信息。参照国际化（il8n）标准，它可以包含信息号或者编码，可以只包含其中一个，或者同时包含并用分隔符隔开。 - data——包含响应的div。当状态值为”fail”或”error”时，data仅包含错误原因或异常名称、或者null也是可以的。

例如，返回成功的响应json格式：

{
  "code": 200,
  "message": "success",
  "data": {
    "userName": "123456",
    "age": 16,
    "address": "beijing"
  }
}
返回失败的响应json格式：

{
  "code": 401,
  "message": "error  message",
  "data": null
}
## 5. http状态码
HTTP状态码也是有规律的：

### 1，请求未成功 
### 2，请求成功、表示成功处理了请求的状态代码。 
### 3，请求被重定向、表示要完成请求，需要进一步操作。通常，这些状态代码用来重定向。 
### 4， 请求错误这些状态代码表示请求可能出错，妨碍了服务器的处理。 
### 5，（服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。

### 6. 合理使用query parameter
在请求数据时，客户端经常会对数据进行过滤和分页等要求，而这些参数推荐采用HTTP Query Parameter的方式实现。

//比如设计一个最近登陆的所有用户
http://example.com/api/users?recently_login_day=3

//搜索用户，并按照注册时间降序
http://example.com/api/users?recently_login_day=3

//搜索用户，并按照注册时间升序、活跃度降序
http://example.com/api/users?q=key&sort=create_title_asc,liveness_desc
## 7. 多表、多参数连接查询设计URL
在做单个实体的查询比较容易和规范操作，但是在实际的API并不是这么简单而已，因为常常会设计到多表连接、多条件筛选、排序等。

比如我想查询一个获取在6月份的订单中大于500元的且用户地址是北京，用户年龄在22岁到40岁、购买金额降序排列的订单列表，那么接口可能如下：

http://example.com/api/orders?order_month=6&order_amount_greater=500&address_city=北京&sort=order_amount_desc&age_min=22&age_max=40
从这个URL上看，参数众多、调用起来还得一个一个仔细对着，而且API本身非常不容易维护，命名看起来不是很容易，不能太长，也不能太随意。

在.net WebAPI中，我们可以使用属性路由，属性路由就是将路由附加到特定的控制器或操作方法上装饰Controll及其使用[Route]属性，一种定义路由的方法称为属性路由。

这种好处就是可以精准地控制URL，而不是基于约定的路由，简直就是为这种多表查询量身定制似的的。从webapi 2开发，现在是RESTful API开发中最推荐的路由类型。

[Route(“api/orders/{address}/{month}”)]
而Action中的查询参数就只有金额、排序、年龄。减少了查询参数、API的可读性和可维护行增强了。

http://example.com/api/orders/beijing/6?order_amount_greater=500&sort=order_amount_desc&age_min=22&age_max=40
所以，在向别人介绍rest的时候，首先需要介绍服务背景，然后再介绍它的一些约束规则，切不可笼统介绍。

