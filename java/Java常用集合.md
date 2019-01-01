# Java常用集合 #

> Java常用的集合大致可以分为两种:   
> 一种是实现了Set接口的，另一种是实现List的接口的。

## 实现Set接口 ##

> TreeSet,HashSet


## 实现List接口 ##

> ArrayList, LinkedList, Vector(线程安全)

## List排序 ##

> JDK7之前使用是collections.sort(list, Comparator)   
> JDK8使用的是List.sort(Comparator)

## 常见List的特点和应用的场景 ## 

### ArrayList ###

> ArrayList是数组结构，多用与查询和修改，少用于中间的增删。每次增删元素顺序都会操作每个元素。   
 ### LinkedList ###

 > 是链表结构：多用于中间，开头增删。少用查询，修改。查询时会遍历大量元素。   
  