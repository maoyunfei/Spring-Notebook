前段时间，Spring 5的发布给我们带来了[WebFlux模块](https://docs.spring.io/spring/docs/5.0.4.RELEASE/spring-framework-reference/web-reactive.html#spring-webflux)，其主要核心是Reactive Programming。

在介绍WebFlux之前，我们需要一些背景知识。


## Reactive Streams
Reactive Streams的目的是为非阻塞的异步流处理提供一个标准。

一些相关资料：

* [Reactive宣言](https://www.reactivemanifesto.org/zh-CN)
* [Reactive Streams的JVM规范](https://github.com/reactive-streams/reactive-streams-jvm)

### Reactive Streams定义的JVM规范如下：

* 处理可能无限数量的元素
* 按顺序处理
* 在组件之间异步传递元素
* 具有强制性的非阻塞的backpressure

### Reactive Streams规范由以下两部分组成：

* **API:** 定义要实现的类型，并实现不同类型之间的交互操作。
* **Technology Compatibility Kit (TCK):** 用于实现一致性测试的标准测试套件。

只要符合API要求并通过TCK中的测试，实现就可以自由地实现规范未涵盖的其他功能。

### API组件

* **Publisher:** 发布者将数据发送给一个或多个订阅者。

```java
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
```
* **Subscriber:** 订阅者将自己订阅发布者，指出发布者可以发送多少数据并处理数据。

```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s);
    public void onNext(T t);
    public void onError(Throwable t);
    public void onComplete();
}
```
* **Subscription:** 在发布者方面，订阅将被创建，其将与订阅者共享。

```java
public interface Subscription {
    public void request(long n);
    public void cancel();
}
```
* **Processor:** 处理器可以用于位于发布者和订阅者之间，这样就可以进行数据转换。

```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```
Reactive Streams规范被包含在Java 9的[Flow API](https://community.oracle.com/docs/DOC-1006738)中。

## RxJava项目
[RxJava](https://github.com/ReactiveX/RxJava)使用观察者模式，构建了一种异步且基于事件的实现，并且提供了处理数据流的丰富操作，是Reactive Streams的主流实现之一。

RxJava 2提供了几个基本类：

* [io.reactivex.Flowable](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Flowable.html): 返回N个元素，支持Reactive Streams和backpressure。
* [io.reactivex.Observable](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Observable.html): 返回N个元素，不支持backpressure。
* [io.reactivex.Single](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Single.html): 返回1个元素或者error。
* [io.reactivex.Completable](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Completable.html): 没有元素，只有完成和error信号。
* [io.reactivex.Maybe](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Maybe.html): 返回0个元素，1个元素或者error。

## Reactor项目
[Reactor](https://projectreactor.io/)是基于Reactive Streams规范提供的一个Reactive库，是Reactive Streams的另一主流实现。

Reactor提供两种类型：

* **Mono:** 实现了Publisher，返回0或1个元素。
* **Flux:** 实现了Publisher，返回N个元素。

API练习项目：

* [Reactive Programming with Reactor 3
](https://tech.io/playgrounds/929/reactive-programming-with-reactor-3)

* [Reactor API Hands-on](https://github.com/reactor/lite-rx-api-hands-on)

## Spring WebFlux
Spring WebFlux是完全非阻塞的，支持Reactive Streams的back pressure，可以运行在例如Netty、Undertow和Servlet 3.1+容器上。

<img src="http://upload-images.jianshu.io/upload_images/3424642-7922d13b6c20ee6e.png?imageMogr2/auto-orient/strip" width="60%" height="60%">

Spring Framework内部采用Reactor来支持响应式编程。在应用层，用户也可以选择使用Spring提供的对RxJava的全面支持。

更多WebFlux的用法请查看[官方文档](https://docs.spring.io/spring/docs/5.0.4.RELEASE/spring-framework-reference/web-reactive.html#spring-webflux)。

