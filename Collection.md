### 概况
集合<i>collection hierarchy</i>的根节点，JDK中不对此根节点直接进行实现，而是实现它的子接口如<tt>Set</tt> 和 <tt>List</tt>，这个根节点的存在可以创造最大程度的泛型。   
JDK中任何实现该interface的类都至少包含一个返回空的constructor和一个返回一个子元素的constructor。  
会改动结构的操作根据具体实现“可能”会抛出<tt>UnsupportedOperationException</tt>。  
optional-restrictions：一些实现类可以根据自己的情况对加入，query内部的元素有一定的限制，比如不允许null之类的，当此类操作发生时可以抛出对应的Exception，比如<tt>NullPointerException</tt>，<tt>ClassCastException</tt>等。  
多线程下同步执行的性能取决于每个类的实现，如果没做这一块就不能进行多线程操作，不然会导致未定义行为。
对于Collection的实现中要尽量避免使用<em>equals</em>（从javadoc来看，<em>equals</em>安全性较好，但似乎效率不高），但对应的实现必须是合理的。  
通过循环方式实现一些方法时可能会出现self-referencial的Exception，比如{@code clone()}, {@code equals()}, {@code hashCode()} and {@code toString()}，<font color=red><code>这里还不是很懂</code></font>  
### 方法
