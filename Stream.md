## 概览
StreamApi算是java对函数式编程对支持，可以写出类似于如下代码：
int sum = widgets.stream()
                     .filter(w -> w.getColor() == RED)
                     .mapToInt(w -> w.getWeight())
                     .sum();
其中，widgets应该是一个Collection。Collection是Stream的来源之一。
`IntStream`，`LongStream`，`DoubleStream`（这些并非子类）等也被视为`Stream`，满足`Stream`的限制条件
