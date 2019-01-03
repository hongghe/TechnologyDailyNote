# Java中的Set即集合 #

> Set接口中有获取大小size()，是否包含contains()，containsAll()，转为数组toArray()，增加add()，addAll()，移除remove()等方法。   
> 继承了Collection接口。

## HashSet ##

> 1.HashSet中不能有相同的元素，可以有一个Null元素，存入的元素是无序的。  
2.HashSet如何保证唯一性？  
1).HashSet底层数据结构是哈希表，哈希表就是存储唯一系列的表，而哈希值是由对象的hashCode()方法生成。   
2).确保唯一性的两个方法：hashCode()和equals()方法。   
3.添加、删除操作时间复杂度都是O(1)。  
4.非线程安全  

## TreeSet ##

> 1.TreeSet是中不能有相同元素，不可以有Null元素，根据元素的自然顺序进行排序。  
2.TreeSet如何保证元素的排序和唯一性？  
底层的数据结构是红黑树(一种自平衡二叉查找树)  
3.添加、删除操作时间复杂度都是O(log(n))  
4.非线程安全  

## LinkedHashSet ##

> 1.LinkedHashSet中不能有相同元素，可以有一个Null元素，元素严格按照放入的顺序排列。    
2.LinkedHashSet如何保证有序和唯一性？    
1).底层数据结构由哈希表和链表组成。  
2).链表保证了元素的有序即存储和取出一致，哈希表保证了元素的唯一性。   
3.添加、删除操作时间复杂度都是O(1)。   
4.非线程安全