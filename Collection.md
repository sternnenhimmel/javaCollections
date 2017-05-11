### 概况
集合<i>collection hierarchy</i>的根节点，JDK中不对此根节点直接进行实现，而是实现它的子接口如<tt>Set</tt> 和 <tt>List</tt>，这个根节点的存在可以创造最大程度的泛型。   
JDK中任何实现该interface的类都至少包含一个返回空的constructor和一个返回一个子元素的constructor。  
会改动结构的操作根据具体实现“可能”会抛出<tt>UnsupportedOperationException</tt>。  
optional-restrictions：一些实现类可以根据自己的情况对加入，query内部的元素有一定的限制，比如不允许null之类的，当此类操作发生时可以抛出对应的Exception，比如<tt>NullPointerException</tt>，<tt>ClassCastException</tt>等。  
多线程下同步执行的性能取决于每个类的实现，如果没做这一块就不能进行多线程操作，不然会导致未定义行为。
对于Collection的实现中要尽量避免使用<em>equals</em>（从javadoc来看，<em>equals</em>安全性较好，但似乎效率不高），但对应的实现必须是合理的。  
通过循环方式实现一些方法时可能会出现self-referencial的Exception，比如{@code clone()}, {@code equals()}, {@code hashCode()} and {@code toString()}，<font color=red><code>这里还不是很懂</code></font>  
### 方法
#### int size(); 
#### boolean isEmpty();
#### boolean contains(Object o);
#### Iterator<E> iterator();
接口，顺序仅有实现类保证。
#### Object[] toArray();
接口，注意这个返回的array一定是新创建的，所以一定是safe的，但是应该蛮费时间的。
#### <T> T[] toArray(T[] a);
接口，把当前Collection里的东西填入给定的array，如果大小不够则会创建新的array，如果a是null，或者T不是但前Collection元素的子类，则会丢异常。
#### boolean add(E e);
接口，一个Modification操作。根据子类是否允许插入相同的Element，如果插入成功则返回true（List），如果已包含且不允许重复则返回false（Set）。如果子类因为除了已包含当前元素以外的原因拒绝插入，则必须抛出异常，此种机制保证了子类在执行了插入操作以后一定会包含当前元素。可能抛出的异常有：UnsupportedOperationException，ClassCastException，NullPointerException，IllegalArgumentException，IllegalStateException
#### boolean remove(Object o);
接口，把当前Element移除掉，如果包含一个或多个，即执行此操作后当前Collection产生变化，则返回true，否则返回false。
#### boolean containsAll(Collection<?> c);
接口，判断当前Collection是否包含了传入的Collection的所有元素。从说明来看，似乎可以进行元素的cast，不过应该要看子类具体的实现
#### boolean addAll(Collection<? extends E> c);
接口，会丢出和<em>boolean add(E e)</em>类似的Exception，另外，该操作不是原子操作，如果插入过程中c变化的话会引发未定义行为。
#### boolean removeAll(Collection<?> c);
接口，移除所有c中包含的元素，行为和<em>boolean remove(Object o)</em>类似
#### default boolean removeIf(Predicate<? super E> filter)
默认实现方法。默认会用Iterater遍历并且对匹配的元素调用<em>boolean remove(Object o)</em>，可以看出filter的反向操作。
#### boolean retainAll(Collection<?> c);
接口，只保留c中包含的元素，似乎是用<em>boolean contains(Object o)</em>和<em>boolean remove(Object o)</em>来实现的，即移除不包含的元素，但具体还是看子类。
#### void clear();
#### boolean equals(Object o);
接口，如果直接实现Collection要对这个接口非常小心，最好还是依赖于Object自带的equals，由于List和Set的限制，List只能和List相等，Set只能和Set相等，一个Collection如果没有实现List和Set则不可能和他们相等，同样，也无法创建一个Class同时实现List和Set
#### int hashCode();
接口，返回hashCode，<tt>Object.hashCode</tt>，一个实现了<em>boolean equals(Object o)</em>也必须实现当前实现类
#### default Spliterator<E> spliterator()
默认实现为带late-binding特性的一个Spliterator，fail-fast特性继承自子类的Iterator。默认实现需要被改写以实现高效的拆分。子类实现中，创建Spliterator时需要加入该子类的各种特性。比如SIZED，IMMUTABLE，CONCURRENT等。
#### default Stream<E> stream()
默认实现为使用本集合的元素创建一个Stream，当本集合不包含late-binding，IMMUTABLE，CONCURRENT中的任何一个特性时，当前函数需要被重写。
#### default Stream<E> parallelStream()
创建一个并行的Stream，当本集合不包含late-binding，IMMUTABLE，CONCURRENT中的任何一个特性时，当前函数需要被重写。
