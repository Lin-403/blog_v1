# 一、涉及相关知识点



## 1. process:

> process： 是node的全局变量，类似浏览器的window
>
> process.env ：（环境配置）属性会返回包含用户环境的对象，能返回项目运行所在环境的一些信息
>
> process.env.NODE_ENV：NODE_ENV是一个用户自定义的变量，它的用途是区分生产环境或开发环境



## 2. nodemon使用

> 实时同步node变化相关，

## 3. ORM  （Object Relational Mapping）
> ​    把关系数据库的表结构映射到对象，不使用sql语句即可对数据库进行操作
>
>    对象关系映射，尽量减少使用sql语句，下载相关依赖包sequelize以及数据库驱动mysql2

![image-20220322095324392](C:\Users\ASUS\Desktop\BLOG\ORM.png)

## 4.async/await
> ​     带async关键字的函数，是声明异步函数，返回值是promise对象。
>
> 其定义的函数会自动返回一个 *<u>Promise对象resolve</u>* 的值，因此其可以直接进行then操作，返回的值即为then方法的传入函数
>
> （如果async关键字函数返回的不是promise，会自动用Promise.resolve()包装）
>
> -------------------
>
> ​    await在异步函数里实现的功能是等待执行，等待后面跟的函数执行完毕在进行下一步操作。await作用就是获取Promise中返回的内容，获取Promise函数中resolve和reject的值

## 5.sequelize.sync({alter:true})



> User.sync() - 如果表不存在,则创建该表(如果已经存在,则不执行任何操作)   
>
>  Sequelize 是一个基于promise的适用于Node.js的ORM                                                         
> ​   force:为true时强制删除表 alter：为true时更新表字段

## 6.cors解决跨域

> app.use(cors({credentials:true,origin:true})) 

## 7.express.json（）
> Express中的内置中间件函数 它使用body-parser解析带有JSON有效负载的传入请求。----------主要解析数据

## 8.morgan中间件记录日志

> app.use(morgan('tiny'))    //http请求日志
>
> tiny：the minimal output
>
> :method  :url  :statue  :res[content-length] -:response-time ms

## 9.自定义中间件的使用（处理错误）

> *中间件*函数是可以访问[请求对象](http://www.expressjs.com.cn/en/4x/api.html#req) （`req`），[响应对象](http://www.expressjs.com.cn/en/4x/api.html#res)（`res`）以及`next`应用程序请求 - 响应周期中的函数的函数。
>
> 该`next`功能是Express路由器中的一个功能，当被调用时，它将执行当前中间件之后的中间件。
>
> 中间件功能可以执行以下任务：
>
> - 执行任何代码。
> - 更改请求和响应对象。
> - 结束请求 - 响应周期。
> - 调用堆栈中的下一个中间件。
>
> -----
>
> 顺序：
>
> 用户发送请求（request）给网站，先经过中间件，然后给到urls，最终到视图层；
> 视图层返回响应（response）给用户，先经过中间件，最终到用户。

![image-20220322095429808](C:\Users\ASUS\Desktop\BLOG\路由环境.png)

## 10.常见状态码分类

> 1xx：通知
>
> 2xx：请求成功地接收
>
> 3xx：重定向
>
> 4xx：客户端错误
>
> 5xx：服务器出现错误
>
> ------
>
> 状态码：
>
> （1）200：请求成功，浏览器会把响应内容（通常是html）显示在浏览器上；
>
> （2）404：（客户端问题）请求的资源没有找到，说明客户端错误的请求了不存在的资源。
>
> （3）500：（服务端错误）请求的资源找到了，但服务器内部发生了不可预期的错误。

## 11. 加密

### （1）md5加密

> 下载指令：yarn add md5
>
> 单向数据加密，不可破解
>
> 加密成32位密码，几乎不可逆；
>
> 使用方法如下：
>
> ```js
> const newMd5PWD=md5(password+TANG)
> ```

### （2）bcrypt加密

> 安装前要安装相关依赖   node-gyp
>
> ```js
> npm install -g node-gyp
> npm install --global --production windows-build-tools
> npm install bcrypt -S
> -------------
> npm install bcrypt -S
> ```
>
> 使用：
>
> 自己内部会传入SALT，只需自定义值即可，而非自行拼接。
>
> 是单向Hash加密算法，类似Pbkdf2算法 不可反向破解生成明文。
>
> ```js
> bcrypt.hash(password,SALT_ROUNDS,(err,encrypted)=>{})
> ```
>
> 内部自带验证是否匹配的函数compare
>
> ```js
> const match=await bcrypt.compare(password,oldHashPwd)
> ```
>
> 

## 12.token

### （1）利用token进行身份验证流程

> 1. 客户端使用用户名和密码请求登录
>
> 2. 服务端收到请求，验证用户名和密码
>
> 3. 验证成功，服务端前发一个token（令牌），再把这个token返回给客户端
>
> 4. 客户端收到token后将其存储起来（例放入cookie中）
> 5. 客户端每次向服务端请求资源时需要携带（cookie中或token中携带）服务端签发的token。
> 6. 服务端收到请求，然后去验证客户端请求里的token，验证成功，返回请求数据。



### ![image-20220322141919905](C:\Users\ASUS\Desktop\BLOG\token.png)

### （2）token组成

> Header（头部）：base64UrlEncode(header)，header（JWT的元数据：签名的算法、令牌类型）
>
> Patload（负载）：base64UrlEncode(payload），存放实际需要传递的数据
>
> Signature（签名）：（`HMAC是一种使用单向散列函数来构造消息认证码的方法，其中HMAC中的H就是Hash的意思`）           HMACSHA256（base64UrlEncode(header)+'.'+base64UrlEncode(payload),secret)

### （3）token优点

> 1. 支持跨域访问：cookie无法跨域，token如果放在请求头而非cookie中，就不会产生跨域访问，就不存在跨域信息丢失问题
> 2. 无状态：token机制不需存储session信息，其本身就包含了用户所有登录信息，所以可以减轻服务端压力。
> 3. 更适用CDN（Content Delivery Network）：可以通过 内容分发网络（CDN）请求服务端所有资料
> 4. 更适用于移动端：当客户端是非浏览器平台时，cookie是不被支持的，但使用token会更简单
> 5. 无需考虑CSRF（**跨站请求伪造**Cross-site request forgery）：不依赖cookie，所以不需考虑CSRF防御。

**jwt ：Json Web Token**

> privateKey 私有，需要维护，所以放在环境变量env上（密钥）

数据校验：

![image-20220322163008846](C:\Users\ASUS\Desktop\BLOG\数据校验.png)

## 13.validator

> 一种校验器



## 14.验证

> 一共三层验证
>
> 客户端接口验证
>
> 服务器端接口验证
>
> 数据库端接口验证

## 15.性能优化

> （1）懒加载
>
> ```js
> import React,{Component,lazy,Suspense} from 'react'
> const Login=lazy(()=>
>   import('./pages/Login/index.js')
> )
> 
> ```
>
> 也可以称为路由按需加载，只有当我们访问到某个路由的时候才将其导入进来，这中间可能会有时间差：
>
> ```js
> Suspense组件处理：异步加载资源，页面显示问题
> ```
>
> 只需在路由外面包裹一层Suspense即可，使用如下：
>
> ```js
> <Suspense fallback={<p>Loading。。。</p>}>
>      <Switch>
>            <Route path="/" component={Home} exact/>
>            <Route path="/login" component={Login}/>
>      </Switch>
> </Suspense>
> ```
>
> --------------------
>
> 
>
> （2）无用渲染 PureComponent --> 针对class组件
>
> state/props 没变化=>shouldComponentUpdate 生命周期
>
> ```js
> shouldComponentUpdate(nextProps,nextState){
>    return true
> }
> ```
>
> PureComponent :将组件state/props和nextstate/nextprops比较（浅比较）
>
> 类组件直接继承React.PureComponent
>
> 做法：将之前的Component改写成PureComponent
>
> 
>
> React.memo 
>
> 作用：和PureComponent类似，但其针对函数组件重新渲染。
>
> 就是函数组件的PureComponent。
>
> 



