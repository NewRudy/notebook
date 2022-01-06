# 写了加深记忆的

前后端分离的意思是通过RESTful API  传递JSON数据进行交流，后端不交涉页面内容。

前端用前端的服务器(Nginx)，后端用后端的服务器（Tomcat），通过前端的服务器将前端的请求发送给后端的服务器，这就成为反向代理，反向代理可以不暴露服务器的真实地址

真实的项目中，直接将账号密码写上去太危险了了，一般的做法是存储密码等信息的hash值

使用数据库验证的逻辑和前面类似，大致如下：

1. 获得前端发送过来的用户名和密码信息
2. 查询数据库中是否存在相同的一对用户名和密码
3. 如果存在返回200，否则返回失败代码400

对UserDAO进行了二次封装，我们再DAO中定义了基础的增删改查操作，而具体的操作应该是再Service里面进行完成

这个项目将会很长的时间都会采用这种简单的三层架构：DAO + Servic + Controller

- DAO用于与数据库交互，定义增删改查操作
- Service负责业务逻辑，跟功能有关的代码一般都写在这里，编写、调用各种方法对DAO取得的数据进行操作
- Controller 负责数据交互，即接受前端发送的数据，通过调用Service获得处理后的数据并返回

- 在实践中我们一般倾向于让Controller清凉一些，以方便代码的阅读者快速找到分析功能的入口

在这个看脸的时代，代码写的烂，界面写的也不好，就真的没救了，所以所我们需要element等组件，为什么我的element总是没有引用进去啊

登录的页面似乎较为完善了，但是我们这个登录页面其实没有用，因为别人直接输入首页的网址就可以进去了，为了防止别人绕过登录页面，我们需要做一个拦截器。

登录界面做完，我们差不多就理解了项目的构成，之后的开发就可以直接集中在业务功能的实现上面了，之后的基本模式就是前端开发组件，后端开发控制器，调试功能

URL中的#号是什么意思呢？#号可以认为是一个锚点，里哦那个AJAX，我们就可以不重载页面就刷新数据，如果我们加上#号的特性（即改变URL但是不请求后端），我们就可以在前端实现页面的整体变化而不用每次都去请求后端。为了实现前端路由，我们就可以监听#号后面内容的变化（hashchange），从而动态的改变页面的内容，URL的#号后面的地址成为hash，这种实现模式我们称之为hash模式，是非常典型的前端路由方式。

另一种常用的模式叫history模式，这种方式使用了History API，该API是针对历史记录的API，这种模式的原理是把原来页面的状态保存到一个对象（state）里面，当页面的URL变化时找到对应的对象，从而还原了这个页面。

Vue已经实现了这两种前端路由

回顾一下单页面应用的概念，在项目中其实只有index.html这一个页面，所有其他的内容都是在这一个页面中渲染的，当我们直接在后端方位/login路径的时候，后端中并未有该内容，为了获取到我们需要的页面，我们需要想办法触发前端路由，即在后端添加处理内容，把通过这个URL渲染出的index.html返回到浏览器中。

后端访问登录页面还是没跳转过去啊

后端拦截器的开发：

1. 用户访问URL，检测是否为登录页面
2. 如果用户访问的不是登录页面，检测用户是否已经登录，否则转到登录界面

为了保存登录的状态，我们需要把用户信息保存在Session中（当用户在web页面之间进行跳转的时候，存储在Session中的对象的变量不会丢失）

我们在拦截器 LoginInterceptor 中配置的路径，即index，触发的时机在拦截器生效以后。我们访问一个URL，先通过Configurer判断是否需要拦截，如果需要，才会触发拦截器，根据我们的自定义的规则再次判断。拦截器这里没咋看懂

前端拦截器需要一个全局属性，不应该写在单一的组件里面，所以引入了一个新的工具Vuex（专门为vue开发的状态管理方案），该工具可以把需要在各个组件中传递使用的变量、方法定义在这里。

项目虽然本质上是一个单页面应用，但是表面上有多个功能页面

在一个组件中通过导入引用了其它组件，可以称之为父子组件，如果想要通过<router-view/>控制子组件的显示，则需要相关的路由的配置

Books.Vue的修改：添加搜索框，添加增加，删除按钮，完善分页功能，构造增、删、改、查功能

pojo包中读数据库中的表   dao包中提供接口   service包中根据接口写服务   controller中继续写需要的API

项目中需要查询的地方主要有三处：

- 打开页面，默认查询出所有图书并显示(页面初始化)
- 点击左侧分类栏，按照分类显示图书
- 在搜索栏中输入作者或者书名，可以模糊的查询出相关书籍

页面初始化：使用了Vue的钩子函数-mounted，钩子函数就是在某个特定条件下被触发的函数，钩子函数一般与生命周期对应，mounted翻译为已挂载，所谓挂载，就是我们写的Vue代码被转换为HTML并替换为相应的DOM这个过程，这个过程完事的时候，就会执行mounted里面的代码。



## 中途隔了几天了，review一下：

### 简介

![应用架构](https://img-blog.csdnimg.cn/20200524211402855.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L05ldWZfU29sZWls,size_16,color_FFFFFF,t_70#pic_center)

![技术架构图](https://img-blog.csdnimg.cn/20200524211507112.JPG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L05ldWZfU29sZWls,size_16,color_FFFFFF,t_70#pic_center)

![image-20201102101702795](http://111.229.14.128:9001/wutian/image-20201102101702795.png)

![image-20201102101717716](http://111.229.14.128:9001/wutian/image-20201102101717716.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191209211442519.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9sZWFybmVyLmJsb2cuY3Nkbi5uZXQ=,size_16,color_FFFFFF,t_70)

![image-20201102101749120](http://111.229.14.128:9001/wutian/image-20201102101749120.png)

### 安装Vue Cli

npm 安装 ， npm install -g vue-cli  安装脚手架，vue init webpack [name]   构建前端项目



![在这里插入图片描述](https://img-blog.csdnimg.cn/2019040120135655.png)

components中创建组件，然后设置反向代理（就是再src/main.js)里面配置axios，eg：

```js
// 设置反向代理，前端请求默认发送到 http://localhost:8443/api
var axios = require('axios')    //我个人写的是 import axios from 'axios'
axios.defaults.baseURL = 'http://localhost:8443/api'
// 全局注册，之后可在其他组件中通过 this.$axios 发送数据
Vue.prototype.$axios = axios
Vue.config.productionTip = false
```

配置页面路由：

```js
//格式如下
  routes: [
  // 下面都是固定的写法
    {
      path: '/login',
      name: 'Login',
      component: Login
    },
    {
      path: '/index',
      name: 'AppIndex',
      component: AppIndex
    }
  ]
```

在 config\index.js 中配置跨域支持：

```js
    proxyTable: {
      '/api': {    //我也不知道为啥要api，可能是配置反向代理的时候用了api吧
        target: 'http://localhost:8443',
        changeOrigin: true,
        pathRewrite: {
          '^/api': ''
        }
      }
    }

```

运行项目 npm run dev



### 后端项目创建

spring initializer，输入项目元数据，选择web，finish

pojo包中创建自己要使用的类

result包中创建Result类，构造response，主要是响应码：

```java
package com.evan.wj.result;

public class Result {
    //响应码
    private int code;    //这里code应该使用枚举，响应码是确定的

    public Result(int code) {
        this.code = code;
    }

    public int getCode() {
        return code;
    }

    public void setCode(int code) {
        this.code = code;
    }

}
```

controller包中新建一个类，用来对响应进行处理，eg：LoginController

```java
package com.evan.wj.controller;

import com.evan.wj.result.Result;    //引进了响应码
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.util.HtmlUtils;

import com.evan.wj.pojo.User;    //引进了类

import java.util.Objects;

@Controller
public class LoginController {

    @CrossOrigin
    @PostMapping(value = "api/login")    //我写的是 @RequestMapping(value = "api/login")
    @ResponseBody
    public Result login(@RequestBody User requestUser) {    //RequestBody就是前端传来的数据
    // 对 html 标签进行转义，防止 XSS 攻击
        String username = requestUser.getUsername();
        username = HtmlUtils.htmlEscape(username);

        if (!Objects.equals("admin", username) || !Objects.equals("123456", requestUser.getPassword())) {
            String message = "账号密码错误";
            System.out.println("test");
            return new Result(400);
        } else {
            return new Result(200);
        }
    }
}
```

引入数据库：MySQL，工具Navicat，创建数据库，表，导入数据

项目相关配置：修改 pom.xml(配置完之后要在maven中重新导入一遍)

把pojo包中的类重新改一遍，因为数据是从数据库里面拿了，要建立对数据库的映射 eg：

```java
package com.evan.wj.pojo;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

import javax.persistence.*;

@Entity    //表示一个实体
@Table(name = "user")    //表名
@JsonIgnoreProperties({"handler","hibernateLazyInitializer"})

public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    int id;

    String username;
    String password;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

```

在dao包中创建一个类来操作数据库对象，但是这里是创建一个接口，eg：

```java
package com.evan.wj.dao;

import com.evan.wj.pojo.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserDAO extends JpaRepository<User,Integer> {
    User findByUsername(String username);

    User getByUsernameAndPassword(String username,String password);
}
```

这里关键的地方在于方法的名字。由于使用了 JPA，无需手动构建 SQL 语句，而只需要按照规范提供方法的名字即可实现对数据库的增删改查。

在service包中创建一个类，提供相关服务，这里实际上是对 `UserDAO` 进行了二次封装，一般来讲，我们在 DAO 中只定义基础的增删改查操作，而具体的操作，需要由 Service 来完成。eg：

```java
package com.evan.wj.service;

import com.evan.wj.dao.UserDAO;
import com.evan.wj.pojo.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service
public class UserService {
    @Autowired
    UserDAO userDAO;

    public boolean isExist(String username) {
        User user = getByName(username);
        return null!=user;
    }

    public User getByName(String username) {
        return userDAO.findByUsername(username);
    }

    public User get(String username, String password){
        return userDAO.getByUsernameAndPassword(username, password);
    }

    public void add(User user) {
        userDAO.save(user);
    }
}

```

controller包里面写一个类，引入pojo，result，service，dao，处理前端发回来的数据和后端的数据，完成业务逻辑

```java
package com.evan.wj.controller;

import com.evan.wj.pojo.User;
import com.evan.wj.result.Result;
import com.evan.wj.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.util.HtmlUtils;

@Controller
public class LoginController {

    @Autowired
    UserService userService;

    @CrossOrigin
    @PostMapping(value = "/api/login")
    @ResponseBody
    public Result login(@RequestBody User requestUser) {
        String username = requestUser.getUsername();
        username = HtmlUtils.htmlEscape(username);

        User user = userService.get(username, requestUser.getPassword());
        if (null == user) {
            return new Result(400);
        } else {
            return new Result(200);
        }
    }
}

```



简单的三层架构（DAO + Service + Controller）

- DAO 用于与数据库的直接交互，定义增删改查等操作
- Service 负责业务逻辑，跟功能相关的代码一般写在这里，编写、调用各种方法对 DAO 取得的数据进行操作
- Controller 负责数据交互，即接收前端发送的数据，通过调用 Service 获得处理后的数据并返回





### 前端使用element

安装：npm i element-ui -S ，引入import  Element-UI from 'element-ui' ,  import 'element-ui/lib/theme-chalk/index.css'  ,  接下来就是根据例子改自己组件的部分了



### 前端路由

前端路由：作用改变前端的URL却不用的请求后端，在前端实现页面的整体变化

#后面的地址成为hash，有#号成为hash模式，另一种常用的前端模式是history模式，模式的原理是先把页面的状态保存到一个对象（`state`）里，当页面的 URL 变化时找到对应的对象，从而还原这个页面

使用history模式：修改 `router\index.js`，加入 `mode: 'history`

前端打包部署到后端（不推荐的做法）：先npm run build，这时在项目的 `dist` 文件夹下生成了 `static` 文件夹和 `index.html` 文件，把这两个文件，拷贝到我们后端项目的 `wj\src\main\resources\static` 文件夹下

为什么要这里做呢，因为我们实际上只有index.html，后端想要方位其它路径是没有的，所以使用这种build之后copy的方式



### 后端拦截器

使用session来判断用户是否已经登录，加一条语句 `session.setAttribute("user", user);` 

新建一个拦截器的包 `interceptor`,然后新建类，在 Springboot 我们可以直接继承拦截器的接口，然后实现 `preHandle` 方法。`preHandle` 方法里的代码会在访问需要拦截的页面时执行。eg：

```java
package com.evan.wj.interceptor;

import com.evan.wj.pojo.User;
import org.apache.commons.lang.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

public class LoginInterceptor  implements HandlerInterceptor{

    @Override
    public boolean preHandle (HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        HttpSession session = httpServletRequest.getSession();
        String contextPath=session.getServletContext().getContextPath();
        String[] requireAuthPages = new String[]{
                "index",
        };

        String uri = httpServletRequest.getRequestURI();

        uri = StringUtils.remove(uri, contextPath+"/");
        String page = uri;

        if(begingWith(page, requireAuthPages)){
            User user = (User) session.getAttribute("user");
            if(user==null) {
                httpServletResponse.sendRedirect("login");
                return false;
            }
        }
        return true;
    }

    private boolean begingWith(String page, String[] requiredAuthPages) {
        boolean result = false;
        for (String requiredAuthPage : requiredAuthPages) {
            if(StringUtils.startsWith(page, requiredAuthPage)) {
                result = true;
                break;
            }
        }
        return result;
    }
}


```

写完拦截器之后要将它配置到项目中：

新建config包，再新建类：

```java
package com.evan.wj.config;

import com.evan.wj.interceptor.LoginInterceptor;
import org.springframework.boot.SpringBootConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.web.servlet.config.annotation.*;

@SpringBootConfiguration
public class MyWebConfigurer implements WebMvcConfigurer {

    @Bean
    public LoginInterceptor getLoginIntercepter() {
        return new LoginInterceptor();
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry){
        // 这条语句是关键
        registry.addInterceptor(getLoginIntercepter()).addPathPatterns("/**").excludePathPatterns("/index.html");
    }
}


```

前端还要进行相应的设置，感觉我现在用不上，就不看了



### 数据库

## review到此结束





上传文件的逻辑很简单：前端向后端发送post请求，后端对接受到的数据进行处理（压缩、格式转换、重命名等），并保存再服务器中的指定位置，再把该位置对应的url返回给前端即可