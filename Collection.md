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

