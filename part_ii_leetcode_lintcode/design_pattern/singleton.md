# Singleton
Singleton is a most widely used design pattern. If a class has and only has one instance at every moment, we call this design as singleton.

## Eager Approach
Rely on JVM to create the unique instance of Singleton when the class is loaded. The instance is created before any threads are able to access it.

### Code
```java
class Singleton {
  private static final Singleton INSTANCE = new Singleton();

  private Singleton() {}

  public static Singleton getInstance() {
    return INSTANCE;
  }
}
```

## Lazy Approach
The instance is only initilizaed when it is first used. This can help to save resource usage if initializing the Singleton class is very expensive. 

Use **volatile** keyword to ensure that all threads can see the most recently written value. Refer to [Java Volatile Keyword](../../part_i_basics/java/java_volatile_keyword.md) for more information.

### Code
```java
class Singleton {
  private volatile static Singleton instance;

  private Singleton() {}

  public static Singleton getInstance() {
    if (instance == null) {
      synchronized (Singleton.class) {
        if (instance == null) {
          instance = new Singleton();
        }
      }
    }
    return instance;
  }
}
```
