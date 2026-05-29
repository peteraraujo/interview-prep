## Java Dynamic Proxies

### The Baseline: Static Proxy Pattern
A proxy is an object that acts as a controlled intermediary for another object (the target).

In the static proxy approach, a developer manually creates a new class that implements the same interface as the target. The proxy contains additional logic (such as logging or timing) and then delegates the call to the real implementation.

**Limitation**: Each interface requires its own dedicated proxy class. This leads to significant boilerplate code that must be written, tested, and maintained. The approach does not scale well when many interfaces need the same cross-cutting behavior.

### Dynamic Proxies – The Solution
Dynamic proxies allow the JVM to generate proxy classes at runtime. The interception logic is written only once, and the JVM creates a class that implements the required interface(s) on demand.

#### Core Components

**InvocationHandler (Interception Logic)**
```java
public class TimingInterceptor implements InvocationHandler {
    private final Object target;

    public TimingInterceptor(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        long start = System.currentTimeMillis();
        
        Object result = method.invoke(target, args);
        
        long time = System.currentTimeMillis() - start;
        System.out.println(method.getName() + " took " + time + "ms");
        
        return result;
    }
}
```

**Proxy Creation**
```java
Calculator realCalc = new AdvancedCalculator();

Calculator proxyCalc = (Calculator) Proxy.newProxyInstance(
    Calculator.class.getClassLoader(),
    new Class[]{Calculator.class},
    new TimingInterceptor(realCalc)
);

proxyCalc.add(5, 10);
```

When a method is called on the proxy, the JVM routes the call through the `invoke()` method of the registered `InvocationHandler`.

### Common Use Cases
Dynamic proxies serve as the foundation for several important patterns:

- Mocking in unit tests (Mockito generates proxies that record calls and return stubbed values).
- Remote Procedure Calls (RPC/RMI): Proxies marshal arguments over the network and return results transparently.
- Lazy Loading: A proxy is returned immediately; the expensive object loads only on first method invocation.
- Security and Access Control: Proxies check permissions before delegating.
- Cross-cutting concerns such as logging, auditing, and transaction management.

### Fundamental Limitation
The standard JDK dynamic proxy (`java.lang.reflect.Proxy`) can only proxy interfaces. 

The generated proxy extends `java.lang.reflect.Proxy` and implements the provided interfaces. Because Java does not support multiple class inheritance, it cannot extend concrete classes.

For proxying classes, libraries such as CGLIB or ByteBuddy are used. These libraries generate subclasses through bytecode manipulation. Spring AOP uses JDK proxies for interfaces and CGLIB for classes.

