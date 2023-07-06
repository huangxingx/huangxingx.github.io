## pool基本参数

### 基本参数

- lifo
   GenericObjectPool 提供了后进先出(LIFO)与先进先出(FIFO)两种行为模式的池。默认为true，即当池中有空闲可用的对象时，调用borrowObject方法会返回最近（`后进`）的实例
- fairness
   当从池中获取资源或者将资源还回池中时 是否使用java.util.concurrent.locks.ReentrantLock.ReentrantLock 的公平锁机制,默认为false

### 数量控制参数

- maxTotal
   链接池中最大连接数,默认为8
- maxIdle
   链接池中最大空闲的连接数,默认也为8
- minIdle
   连接池中最少空闲的连接数,默认为0

### 超时参数

- maxWaitMillis
   当连接池资源耗尽时，等待时间，超出则抛异常，默认为-1即永不超时
- blockWhenExhausted
   当这个值为true的时候，maxWaitMillis参数才能生效。为false的时候，当连接池没资源，则立马抛异常。默认为true

### test参数

- testOnCreate
   默认false，create的时候检测是有有效，如果无效则从连接池中移除，并尝试获取继续获取
- testOnBorrow
   默认false，borrow的时候检测是有有效，如果无效则从连接池中移除，并尝试获取继续获取
- testOnReturn
   默认false，return的时候检测是有有效，如果无效则从连接池中移除，并尝试获取继续获取
- testWhileIdle
   默认false，在evictor线程里头，当evictionPolicy.evict方法返回false时，而且testWhileIdle为true的时候则检测是否有效，如果无效则移除

### 检测参数

- timeBetweenEvictionRunsMillis
   空闲链接检测线程检测的周期，毫秒数。如果为负值，表示不运行检测线程。默认为-1.

commons-pool2-2.4.2-sources.jar!/org/apache/commons/pool2/impl/GenericObjectPool.java



```dart
public GenericObjectPool(PooledObjectFactory<T> factory,
            GenericObjectPoolConfig config) {

        super(config, ONAME_BASE, config.getJmxNamePrefix());

        if (factory == null) {
            jmxUnregister(); // tidy up
            throw new IllegalArgumentException("factory may not be null");
        }
        this.factory = factory;

        idleObjects = new LinkedBlockingDeque<PooledObject<T>>(config.getFairness());

        setConfig(config);

        startEvictor(getTimeBetweenEvictionRunsMillis());
    }
```

commons-pool2-2.4.2-sources.jar!/org/apache/commons/pool2/impl/BaseGenericObjectPool.java



```dart
/**
     * The idle object evictor {@link TimerTask}.
     *
     * @see GenericKeyedObjectPool#setTimeBetweenEvictionRunsMillis
     */
    class Evictor extends TimerTask {
        /**
         * Run pool maintenance.  Evict objects qualifying for eviction and then
         * ensure that the minimum number of idle instances are available.
         * Since the Timer that invokes Evictors is shared for all Pools but
         * pools may exist in different class loaders, the Evictor ensures that
         * any actions taken are under the class loader of the factory
         * associated with the pool.
         */
        @Override
        public void run() {
            ClassLoader savedClassLoader =
                    Thread.currentThread().getContextClassLoader();
            try {
                if (factoryClassLoader != null) {
                    // Set the class loader for the factory
                    ClassLoader cl = factoryClassLoader.get();
                    if (cl == null) {
                        // The pool has been dereferenced and the class loader
                        // GC'd. Cancel this timer so the pool can be GC'd as
                        // well.
                        cancel();
                        return;
                    }
                    Thread.currentThread().setContextClassLoader(cl);
                }

                // Evict from the pool
                try {
                    evict();
                } catch(Exception e) {
                    swallowException(e);
                } catch(OutOfMemoryError oome) {
                    // Log problem but give evictor thread a chance to continue
                    // in case error is recoverable
                    oome.printStackTrace(System.err);
                }
                // Re-create idle instances.
                try {
                    ensureMinIdle();
                } catch (Exception e) {
                    swallowException(e);
                }
            } finally {
                // Restore the previous CCL
                Thread.currentThread().setContextClassLoader(savedClassLoader);
            }
        }
    }
```

- numTestsPerEvictionRun
   在每次空闲连接回收器线程(如果有)运行时检查的连接数量，默认为3



```cpp
private int getNumTests() {
        int numTestsPerEvictionRun = getNumTestsPerEvictionRun();
        if (numTestsPerEvictionRun >= 0) {
            return Math.min(numTestsPerEvictionRun, idleObjects.size());
        } else {
            return (int) (Math.ceil(idleObjects.size() /
                    Math.abs((double) numTestsPerEvictionRun)));
        }
    }
```

- minEvictableIdleTimeMillis
   连接空闲的最小时间，达到此值后空闲连接将可能会被移除。默认为1000L * 60L * 30L
- softMinEvictableIdleTimeMillis
   连接空闲的最小时间，达到此值后空闲链接将会被移除，且保留minIdle个空闲连接数。默认为-1.
- evictionPolicyClassName
   evict策略的类名，默认为org.apache.commons.pool2.impl.DefaultEvictionPolicy



```java
public class DefaultEvictionPolicy<T> implements EvictionPolicy<T> {

    @Override
    public boolean evict(EvictionConfig config, PooledObject<T> underTest,
            int idleCount) {

        if ((config.getIdleSoftEvictTime() < underTest.getIdleTimeMillis() &&
                config.getMinIdle() < idleCount) ||
                config.getIdleEvictTime() < underTest.getIdleTimeMillis()) {
            return true;
        }
        return false;
    }
}
```

> 这里就用到了上面提到的两个参数

## doc

- [Apache commons-pool对象池原理分析](https://link.jianshu.com?t=http://shift-alt-ctrl.iteye.com/blog/1917782)
- [GenericObjectPool 避免泄漏](https://link.jianshu.com?t=https://segmentfault.com/a/1190000006889810)
- [apache-common-pool2（配置参数详解，以及资源回收，从池中获取资源，将资源返还给池 逻辑解析）](https://link.jianshu.com?t=http://blog.csdn.net/liang_love_java/article/details/50510753)