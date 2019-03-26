## SpringMVC

### SpringMVC简介

#### 一、什么是MVC框架

　　Spring Web MVC是spring提供给Web应用的框架设计。实际上MVC框架是一个设计理念，它不仅存在于Java世界中，它的流程是各个组件的应用和改造SpringMVC的根本。所以要了解SpringMVC，首先是要了解SringMVC的流程和各个组件。

#### 二、SpringMVC架构

　　传统的MVC模式(Model-view-controller)是软件工程中的一种软件架构，即把软件系统分为了三个基本部分：模型（Model）、视图（View）和控制器(Controller)。

**控制器(Controller)** - 负责转发请求，对请求进行处理。  
**视图(View)** - 界面设计人员进行图形界面设计。  
**模型(Model)** -程序员编写程序应有的功能(实现算法等等)、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。  

　　传统的MVC框架还是存在着一定的耦合性，比如模型层夹杂着业务层和数据库的访问，所以SpringMVC把传统的模型层拆分为了业务层(Service)和数据访问层(DAO，Data Access Object)。在Service下可以通过Spring的声明式事务操作数据访问层，而在业务层上还允许我们访问NoSQL,这样能满足现代互联网系统的性能。Spring MVC其最大的特点就是结构松散，耦合度较低，对各个模块进行了分离，使得各个层负责各个层的逻辑，开发起来更加清晰。其架构简图如下所示：  
![SpringMVC 架构图](https://github.com/hello8817/codingThought/blob/master/images/SpringMVC架构图.png)


#### 三、Spring MVC组件

　　SpringMVC是一种基于Servlet的技术，其核心是它所提供的核心控制器DispatcherServlet以及相关的组件，并且制定了一套流程规则，其流程图如下：

![SpringMVC 组件图](https://github.com/hello8817/codingThought/blob/master/images/SpringMVC组件图.png)

　　首先要明白的是SpringMVC框架是围绕DispatcherServlet而工作的，他是一个Servlet，它可以拦截HTTP发送过来的请求，在Servlet初始化时，SpringMVC会根据配置，获取配置信息，从而得到统一资源标识符(URI)和处理器(Handler)之间的映射关系(HandlerMapping)，为了更加灵活和增强功能，SpringMVC还会给处理器加入拦截器，所以还可以在处理器执行前后加入自己的代码，这样就构成了一个处理器的执行链(HandlerExecutionChain)，并且根据上下文初始化视图解析器等内容，当处理器返回的时候就可以通过视图解析器定位视图，然后将数据模型渲染到视图中，用来相应用户的请求了。

#### 四、Spring MVC流程

　　当一个请求到来的时候，DispatcherServlet首先通过请求和事先解析好的HandlerMapping配置，找到对应的处理器(Handler)，这样就准备开始运行处理器和拦截器的执行链，运行处理器需要一个环境，这样它就有了一个处理器的适配器(HandlerAdapter),通过这个适配器就能运行对应的处理器及其拦截器，这里的处理器包含了控制器的内容和其他增强的空能，在处理器返回模型和视图给DispatcherServlet后，DispatcherServlet就会把对应的视图信息传给视图解析器(ViewResolver)，然后视图解析器做完处理后，最后响应用户的请求。这就SpringMVC的整个流程。
