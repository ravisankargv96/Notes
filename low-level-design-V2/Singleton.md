> 7 ways to implement Singleton Design Pattern
> 1. Lazy Initialization


###### 1. Lazy Initialization
```java
class LazySingleton {
	// The single instance, initially null
	private static LazySingleton instance;

	// Private constructor: to preven instatiation by other classes
	private LazySingleton() {}

	// LazySingleton.getInstance() : LazySingleton
	// : Public static method to call from other classes
	public static LazySingleton getInstance() {
		// if !instance : instance = new LazySingleton()
		if(instance == null){
			instance = new LazySingleton();
		}
		return instance;
	}
}
```


###### 2. ThreadSafe Singleton
```java
class ThreadSafeSingleton {
	// intially null
	private static ThreadSafeSingleton instance;

	// Private constructor: to prevent instantiation
	private ThreadSafeSingleton() {}

	// ThreadSafeSingleton.getInstance()
	// synchronized: helps only oneThread to access; lowers performance.
	public static synchronized ThreadSafeSingleton getInstance() {
		// if !instance : return new ThreadSafeSingleton();
		if(instance == null){
			instance = new ThreadSafeSingleton();
		}
		return instance;
	}
}
```

###### 3. Double-Checked Locking
```java
class DoubleCheckedSingleton {
	// initially null, marked as volatile (resource value is across thread Memory)
	private static volatile DoubleCheckedSingleton instance;

	// Private Constructor: to prevent instantiation
	private DoubleCheckedSingleton() {}

	// DoubleCheckedSingleton.getInstance()
	// Public method to get instance from other classes
	public static DoubleCheckedSingleton getInstance(){
		// if !instance : Makethis class as synchronize & ... execute statements
		// FirstThread: executes synchronized(){} block, rest all the threads skips that block. Since instance is volatile, creating by any thread updates the resource value.
		if (instance == null){
			// Synchronize on the class object 
			synchronized (DoubleCheckedSingleton.class) {
				// Second check (synchronized)
				if (instance == null) {
					instance = new DoubleCheckedSingleton();
				}
			}
		}
		return instance;
	}
	
}
```


###### 4. Eager Initialization
```java
class EagerSingleton {
	// Makes as final, suchthat initalization should be done & declaration level or constructor level. Also prevents reinitializing.
	private static final EagerSingleton instance = new EagerSingleton();

	// Private constructor: to prevent instantiation
	private EagerSingleton() {}


	// Public method to get the instance
	public static EagerSingleton getInstance() {
		return instance;
	}
}
```

###### 5. BillPugh Singleton
```java
class BillPughSingleton {

	// Static inner class that holds the instance
	private static class SingletonHelper {
		private static final BillPughSingleton INSTANCE = new BillPughSingleton();
	}

	// Private constructor to prevent instantiation
	private BillPughSingleton(){}

	// Public method to get the instance
	public static BillPughSingleton getInstance() {
		return SingletonHelper.INSTANCE;
	}
}
```


###### 6. Enum Singleton
```java
public enum EnumSingleton {
	INSTANCE;

	// public method
	public void doSomething() {
		// Add any singleton logic here
	}
}
```

###### 7. Static Block Initialization
```java
class StaticBlockSingleton {
	
	private static StaticBlockSingleton instance;

	// Static block for initialization
	static {
		try {
			instance = new StaticBlockSingleton();
		} catch (Exception e){
			throw new RuntimeException("Exception occurred in creating singleton instance");
		}
	}

	// Private constructor: to prevent instatiation
	public static StaticBlockSingleton getInstance() {
		return instance;
	}
}
```
