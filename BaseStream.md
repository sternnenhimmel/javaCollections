### 概览
Stream API的基本接口，是一连串支持聚合和并行的元素
### 方法
#### Iterator<T> iterator();
接口，返回一个Iterator。是一个terminal operation
#### Spliterator<T> spliterator();
#### boolean isParallel();
接口，该函数需要在调用terminal operation前被调用，用来判断是否可以并行运行，如果在调用terminal operation之后调用该函数，则会产生未定义行为。
#### S sequential();
接口，返回一个有序的Stream，是一个intermediate operation，比较lazy，不会立即执行。如果当前Stream已经是有序的了，则可能返回自身。
#### S parallel();
接口，返回一个并行的Stream，是一个intermediate operation，比较lazy，不会立即执行。如果当前Stream已经是并行的了，则可能返回自身。
#### S unordered();
#### void close();
对AutoCloseable对实现方法，调用所有当前Stream的Close handler。
#### S onClose(Runnable closeHandler);
把一个closeHandler加入到当前对Stream，在调用<em>void close()</em>时被调用，其中Runnable是一个functional interface
