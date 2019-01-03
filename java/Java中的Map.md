# 认识Java中的Map的数据结构 #

## Map在Java的实现 ##

> 在Java中，有HashMap,LinkedHashMap,TreeMap等，实现Map接口，继承AbstractMap抽象类的数据结构的实现。   
> Map是一种key,value对应的键值对的一种数据结构，在Java中的Map接口中，存在获取Map大小size()，判断Map是否为空isEmpty(),   
> 是否包含特定键值的containsKey(),是否包含特定值constainsValue()，获取第n个键对应的value值get(),存储put(),移除remove()，    
> 以及putAll,clear,ketSet,values,entrySet等方法。   
> 而Map中，键值的结构体entry也包含了getKey,getvalue,setValue,equals,hashCode,comparingByKey,comparingByvalue等方法。   


## HashMap介绍 ##

### HashMap继承和实现的接口 ###

> HashMap继承了AbstractMap，实现Map接口，Cloneable和Serializable接口。

### HashMap总初始化的值 ###

> capacity的初始化的大小是16，且必须是2的幂；最大capacity的大小是2的30次方；默认的加载因子是0.75。

### HashMap中的方法 ###

> 实现Map接口中的方法，增加了resize()，treeifyBin()，removeNode()等方法。    
> 其中，resize()方法的策略是初始化HashMap的大小，或者是Table size的两倍，即使用二次幂扩展，每个二叉树中的元素必须保持相同的索引   
> 或移动在新表中具有两个偏移的幂。