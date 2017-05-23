## 概览
StreamApi算是java对函数式编程对支持，可以写出类似于如下代码：
int sum = widgets.stream()
                     .filter(w -> w.getColor() == RED)
                     .mapToInt(w -> w.getWeight())
                     .sum();
其中，widgets应该是一个Collection。Collection是Stream的来源之一。
`IntStream`，`LongStream`，`DoubleStream`（这些并非子类）等也被视为`Stream`，满足`Stream`的限制条件。
`Stream`和`BaseStream`一样，包含一个数据源以及各种<em>intermediate operations</em>和<em>terminal operation</em>。只有在遇到<em>terminal operation</em>才会真正执行并得到结果，其他时候都是比较lazy的。
`Stream`和`Collection`有相似之处，但目的不同。`Collection`主要用于对于其元素的存储及高效访问，而`Stream`则是对一个数据源进行各种描述性的操作。当默认的运算符无法满足需求时，可以用`iterator()`或者`spliterator()`来进行循环操作。
对`Stream`的各种操作本质上可以理解为<em>query</em>，除非`Stream`自身被设计成可以concurrent访问的形式，若在其进行访问时改变了数据源，就会导致未定义结果。
#### Stream是可以被消耗的!每个成员只能被使用一次（这个和`javaslang`很不一样）。
除非`Stream`来源于IO接口，不然一般不需要调用`close()`去关闭，很多Stream是基于Collection的，它们基本上不需要去close

## 方法
