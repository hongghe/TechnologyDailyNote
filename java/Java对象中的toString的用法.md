# Java中的toString的使用 #

## toString的介绍 ##

> toString是Object里面已经有了的方法，而所有类都是继承Object，所以“所有对象都有这个方法”。它通常只是为了方便输出，比如System.out.println(xx)，括号里面的“xx”如果不是String类型的话，就自动调用xx的toString()方法。总而言之，它只是sun公司开发java的时候为了方便所有类的字符串操作而特意加入的一个方法。    
> 通常，toString 方法会返回一个“以文本方式表示”此对象的字符串。结果应是一个简明但易于读懂的信息表达式。建议所有子类都重写此方法。   
Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于：   
getClass().getName() + '@' + Integer.toHexString(hashCode())
