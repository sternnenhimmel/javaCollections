## 概览
包：package java.util.stream;  
StreamApi算是java对函数式编程对支持，可以写出类似于如下代码：  
```java
int sum = widgets.stream()
                     .filter(w -> w.getColor() == RED)
                     .mapToInt(w -> w.getWeight())
                     .sum();
```
其中，widgets应该是一个Collection。Collection是Stream的来源之一。  
`IntStream`，`LongStream`，`DoubleStream`（这些并非子类）等也被视为`Stream`，满足`Stream`的限制条件。  
`Stream`和`BaseStream`一样，包含一个数据源以及各种<em>intermediate operations</em>和<em>terminal operation</em>。只有在遇到<em>terminal operation</em>才会真正执行并得到结果，其他时候都是比较lazy的。  
`Stream`和`Collection`有相似之处，但目的不同。`Collection`主要用于对于其元素的存储及高效访问，而`Stream`则是对一个数据源进行各种描述性的操作。当默认的运算符无法满足需求时，可以用`iterator()`或者`spliterator()`来进行循环操作。  
对`Stream`的各种操作本质上可以理解为<em>query</em>，除非`Stream`自身被设计成可以concurrent访问的形式，若在其进行访问时改变了数据源，就会导致未定义结果。  
#### Stream是可以被消耗的!每个成员只能被使用一次（这个和`javaslang`很不一样）。
除非`Stream`来源于IO接口，不然一般不需要调用`close()`去关闭，很多Stream是基于Collection的，它们基本上不需要去close

## 方法
#### Stream<T> filter(Predicate<? super T> predicate);
接口，<em>intermediate operations</em>，过滤子元素，predicate为labmda表达式
#### <R> Stream<R> map(Function<? super T, ? extends R> mapper);
接口，<em>intermediate operations</em>，子元素转换操作，mapper为labmda表达式
#### IntStream mapToInt(ToIntFunction<? super T> mapper);
接口，<em>intermediate operations</em>，mapper是一个输入为T，返回为int的function
#### LongStream mapToLong(ToLongFunction<? super T> mapper);
类似于上一个
#### DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper);
类似于上一个
#### <R> Stream<R> flatMap(Function<? super T, ? extends Stream<? extends R>> mapper);
拆分用的函数。一对多，把一个Stream的Stream的子元素的子元素全部拿出来搞成一个新的Stream，层级少了一层。如果某一个元素返回的Stream为null，则视为空Stream，不影响执行。
#### IntStream flatMapToInt(Function<? super T, ? extends IntStream> mapper);
#### LongStream flatMapToLong(Function<? super T, ? extends LongStream> mapper);
#### DoubleStream flatMapToDouble(Function<? super T, ? extends DoubleStream> mapper);
#### Stream<T> distinct();
一个很有趣的接口，返回无重复的`Stream`，对于有序的`Stream`，如果要保证顺序，即保留第一个出现的，应该使用单线程，在这里使用多线程会大大降低效率。如果不需要保证有序或者不需要保证留下的一定是第一个出现的，那么使用多线程计算会快很多。
#### Stream<T> sorted();
返回一个整理好的`Stream`，`Stream`是否有序决定了所谓的`Stability`，该函数依赖于子元素对于`Comparable`的实现，如果子元素不符合要求，可能丢出`ClassCastException`
#### Stream<T> sorted(Comparator<? super T> comparator);
`Stability`和上一个一样，该函数最简单的应用即为函数式编程的应用。其中`Comparator`挺复杂的，事后值得研究一下。
#### Stream<T> peek(Consumer<? super T> action);
这个很有意思，保持当前`Stream`不变，但同时执行一次ForEach，用法如下所示：
```java
Stream.of("one", "two", "three", "four")
    .filter(e -> e.length() > 3)
    .peek(e -> System.out.println("Filtered value: " + e))
    .map(String::toUpperCase)
    .peek(e -> System.out.println("Mapped value: " + e))
    .collect(Collectors.toList());
```
可以用来debug。
#### Stream<T> limit(long maxSize);
和`Stream<T> distinct()`一样，由于取的是前n个而不是任意n个，对于有序的`Stream`用串行很省资源而用并行效率极低，对于无序的`Stream`则取出任意n个，并行效率很高。
#### void forEach(Consumer<? super T> action);
对每个元素执行操作，顺序不确定，任何元素对应的操作都有可能在任何时间任何线程执行
#### void forEachOrdered(Consumer<? super T> action);
对每个元素执行对应的操作，永远按照源数据所规定的顺序来执行。可以用如下方法来测试：
```java
@Test
  public void testForEachOrdered() {
    java.util.stream.Stream.of("AAA", "BBB", "CCC").parallel().forEach(s -> System.out.println("Output:" + s));
    java.util.stream.Stream.of("AAA", "BBB", "CCC").parallel().forEachOrdered(s -> System.out.println("Output:" + s));
  }
```
#### Object[] toArray();
`terminal operation`，在java里面Array是一种确定的存在。默认只能产生java.lang.Object[]，没有什么用处
#### <A> A[] toArray(IntFunction<A[]> generator);
`terminal operation`，这个点不是很好理解，貌似就是要传一个new进去，不怎么好用
#### T reduce(T identity, BinaryOperator<T> accumulator);
有很好的并行性能，sum，avg等都是reduce的特殊情况
#### Optional<T> reduce(BinaryOperator<T> accumulator);
不需要初始值的reduce，其实就是直接把第一个值作为初始值开始聚合，由于前一个function中要求初始值和第一个值的聚合必须等于第一个值，所以其实两者是等价的。
#### <U> U reduce(U identity, BiFunction<U, ? super T, U> accumulator, BinaryOperator<U> combiner);
#### T reduce(T identity, BinaryOperator<T> accumulator
#### T reduce(T identity, BinaryOperator<T> accumulator);
