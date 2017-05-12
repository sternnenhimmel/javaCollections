### 概览
这是一个接口，实现了该接口的类通过<em>{@code try}-with-resources</em>block获得的resource，在出现Exception时会自动调用当前接口所包含的方法，用来释放资源，其效果类似于C++中的析构函数。但使用的场合有一定差别。
### 方法
#### void close() throws Exception
接口，用来释放资源，实现类需要自己改写当前方法。这个方法和{@link java.io.Closeable}不同，不要求是idempotent的，即不要求多次被调用时不出现问题，但是implementor被强烈建议实现该函数被多次调用时不出现问题的功能。
