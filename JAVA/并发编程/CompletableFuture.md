背景：当一个业务接口需要调用下游N个资源时。这些下游资源可以是一次RPC调用、一次数据库操作或者是一次本地方法调用等。

问题：

- 同步调用：接口耗时长、性能差，接口响应时长T > T1+T2+T3+……+Tn
- NIO异步模型：减少线程池的调度开销和阻塞时间：

解决方案：

- 通过RPC NIO异步调用的方式可以降低线程数，从而降低调度（上下文切换）开销，如Dubbo的异步调用可以参考<<dubbo调用端异步>>一文。
- 通过引入CompletableFuture（下文简称CF）对业务流程进行编排，降低依赖之间的阻塞。本文主要讲述CompletableFuture的使用和原理。



什么是CompletableFuture

CompletableFuture是由Java 8引入的，在Java8之前我们一般通过Future实现异步。

- Future用于表示异步计算的结果，只能通过阻塞或者轮询的方式获取结果，而且不支持设置回调方法，Java 8之前若要设置回调一般会使用guava的ListenableFuture，回调的引入又会导致臭名昭著的回调地狱（下面的例子会通过ListenableFuture的使用来具体进行展示）。
- CompletableFuture对Future进行了扩展，可以通过设置回调的方式处理计算结果，同时也支持组合操作，支持进一步的编排，同时一定程度解决了回调地狱的问题。



简单使用示例

```
ExecutorService executor = Executors.newFixedThreadPool(5);
CompletableFuture<String> cf1 = CompletableFuture.supplyAsync(() -> {
    System.out.println("执行step 1");
    return "step1 result";
}, executor);
CompletableFuture<String> cf2 = CompletableFuture.supplyAsync(() -> {
    System.out.println("执行step 2");
    return "step2 result";
});
cf1.thenCombine(cf2, (result1, result2) -> {
    System.out.println(result1 + " , " + result2);
    System.out.println("执行step 3");
    return "step3 result";
}).thenAccept(result3 -> System.out.println(result3));
```

