# Pbbl (pr. _pebble_, pɛbəl)
A thread-safe [Buffer](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/nio/Buffer.html) pool that allows for the automatic reuse of `Buffer` objects (including [`ByteBuffer`](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/nio/ByteBuffer.html), [`CharBuffer`](https://docs.oracle.com/en/java/javase/14/docs/api/java.base/java/nio/CharBuffer.html), etc.), which can be over 30x faster than having to allocate a new `Buffer`.

# Maven/Gradle Dependency
 1. Add Pbbl as a dependency using either Maven or Gradle:

Maven:
```xml
<dependency>
  <groupId>com.github.antideveloppeur</groupId>
  <artifactId>Pbbl</artifactId>
  <version>1.0.2-j8</version>
</dependency>
```
Gradle:
```groovy
implementation 'com.github.antideveloppeur:Pbbl:1.0.2-j8'
```

# Example(s)
1. Create `ByteBufferPool` and manually close it:
```java
// Create a ByteBufferPool (used to pool non-direct ByteBuffer objects).
ByteBufferPool pool = new ByteBufferPool();

// Take a non-direct ByteBuffer (with 8 available bytes) from the pool.
// If the pool is empty, a new ByteBuffer will be created.
ByteBuffer buffer = pool.take(Long.BYTES);

// Do something with the ByteBuffer.
byte[] array = buffer.putLong(42).array();

// Give the ByteBuffer back to the pool for re-use.
pool.give(buffer);

// Close the pool when finished.
pool.close();
```
2. Create `DirectFloatBufferPool` and automatically close it:
```java
// Create a DirectFloatBufferPool (used to pool direct FloatBuffer objects).
try (DirectFloatBufferPool pool = new DirectFloatBufferPool()) {
    // Take a direct FloatBuffer (with 8 available floats) from the pool.
    FloatBuffer buffer = pool.take(8);
    
    // Do something with the FloatBuffer.
    buffer.put(42f);
    
    // Give the FloatBuffer back to the pool for re-use.
    pool.give(buffer);
}
```
