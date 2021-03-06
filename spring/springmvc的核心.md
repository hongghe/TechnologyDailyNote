# SpringMVC的核心 #

## 核心 ##

> 控制反转和面向切面

## 请求处理流程 ##

> 1.首先用户发送请求到前端控制器，前端控制器根据请求信息（如URL）来决定选择哪一个页面控制器进行处理并把请求委托给它，即以前的控制器的控制逻辑部分；   
> 2.页面控制器接收到请求后，进行功能处理，首先需要收集和绑定请求参数到一个对象，并进行验证，然后将命令对象委托给业务对象进行处理；处理完毕后返回一个ModelAndView（模型数据和逻辑视图名）；   
> 3.前端控制器收回控制权，然后根据返回的逻辑视图名，选择相应的视图进行渲染，并把模型数据传入以便视图渲染；   
> 4.前端控制器再次收回控制权，将响应返回给用户。   

## 控制返回的实现 ##

> 我们每次使用spring框架都要配置xml文件，这个xml配置了bean的id和class。
spring中默认的bean为单实例模式，通过bean的class引用反射机制可以创建这个实例。
因此，spring框架通过反射替我们创建好了实例并且替我们维护他们。
A需要引用B类，spring框架就会通过xml把B实例的引用传给了A的成员变量。