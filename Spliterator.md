### 概要
为了多线程遍历而设计，是Iterator的变种。会把一个Array或者类似物拆掉，拆成几个片段。含有各种属性，比如SORTED，DISTINCT，SIZED，NONNULL，CONCURRENT等。可以提供剩下未处理元素的大小的估计，用于决定要不要继续进行splite。Spliterator默认并不是线程安全的，Spliterator只是数据源的一部分，默认情况下不能有多个线程在同一个Spliterator上操作。对于可变的源，在Spliterator bind到源上以及循环结束这段时间期间，如果源产生了变化，则会造成未定义的行为。  
JDK中很多源提供"late-binding"和"fail-fast"功能，即在方法开始循环或者估算剩余大小时才bind到源上，减小受到源变换影响的时间段，以及实时检测源是否变化，若在操作过程中源变化，则抛出Exception，避免未定义行为  
### 方法

#### boolean tryAdvance(Consumer<? super T> action);
接口，必须被实现。尝试对下一个元素执行<em>action</em>，失败或者没有下一个元素时返回false，成功返回true，若action为空则返回NullPointerException

#### default void forEachRemaining(Consumer<? super T> action)
含默认实现，遍历一遍什么都不做，只要有可能就应该在子类中实现。实现后应该对所有剩下的元素执行<em>action</em>。如果是<em>ORDERED</em>，则按顺序来。

#### Spliterator<T> trySplit();
接口，必须被实现。尝试拆分数据源，返回拆分后的结果，注意拆分后的数据应该基本维持平衡，不然很难从并行执行中获得什么好处。

#### long estimateSize();
结构，必须被实现。估计剩余数据源，可能不是很准，但是也可以大致用于之后的行为判断。必须随着每一次<em>trySplit</em>的调用减小

#### default long getExactSizeIfKnown()
含默认实现，如果该spliterator是<em>SIZED</em>的，返回<em>estimateSize</em>的值作为准确值，不然返回-1L

#### int characteristics();
结构，必须被实现。返回SORTED，DISTINCT，SIZED，NONNULL，CONCURRENT等属性，在该Spliterator使用的全过程必须保持恒定。

#### default boolean hasCharacteristics(int characteristics)
默认方法，看起来可以直接用的样子。

#### default Comparator<? super T> getComparator()
如果是SORTED，返回对应的Comparator，默认方法不可用。

#### 可能的属性
ORDERED，DISTINCT，SORTED，SIZED，NONNULL，IMMUTABLE，CONCURRENT，SUBSIZED

#### 内置的数据接口：
OfPremitive，OfDouble，OfInt，OfLong，他们实现了除<em>trySplit</em>以外的功能
