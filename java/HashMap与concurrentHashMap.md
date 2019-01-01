# HashMap与ConcurrentHashMap的区别 #

## HashMap ##

> HashMap是非线程安全的，


## ConcurrentHashMap ##

> ConcurrentHashMap采用锁分段技术，将整个Hash桶进行了分段segment，也就是将这个大的数组分成了几个小的片段segment，   
> 而且每个小的片段segment上面都有锁存在，那么在插入元素的时候就需要先找到应该插入到哪一个片段segment，然后再在这个片段上面进行插入，
> 而且这里还需要获取segment锁。