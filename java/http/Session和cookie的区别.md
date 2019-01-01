# Session 和 Cookie的区别 #

## Session ##

> 存在于服务器中，记录一次请求动作   
> 当访问增多的时候，会比较占用服务器的资源，影响服务器的性能，为了减轻对服务器性能的影响，应该使用Cookie。

### 分布式环境下的Session ###

> 原理：任何一个服务器上的session发生改变（增删改），该节点会把这个 session的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要session，以此来保证Session同步。   
> 优点：可容错，各个服务器间的Session能够实时响应。   
> 缺点：会对网络负荷造成一定压力，如果session量大的话可能会造成网络堵塞，拖慢服务器性能。    

### session共享机制 ###

> 使用分布式缓存方案比如memcached、redis，但是要求Memcached或Redis必须是集群。   

## Cookie ##

> 存在于客户端，浏览器中，记录客户端的动作和行为   
> Cookie是不安全的，外人可以分析存放在本地的Cookie进行Cookie的欺骗，考虑到安全应该使用Session。   
> 单个Cookie不超过4K，很多浏览器最多存储20个Cookie。

