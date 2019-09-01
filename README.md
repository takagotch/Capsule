### capsule
---
https://github.com/puniverse/capsule

http://www.capsule.io/

```java
// capsule/src/test/java/CapsuleTest.java

public class CapsuleTest {

  private final FileSystem fs = Jims.newFileSystem(Configuration.unix());
  private final Path cache = fs.getPath("/cache");
  private final Path tmp = fs.getPath("/tmp");
  private static final ClassLoader MY_CLASSLOADER = Capsule.class.getClassLoader();
  
  private Properties props;
  
  @Before
  public void setup() throws Exception {
    props = new Properties(System.getProperties());
    setProperties(props);
    setCahceDir(cache);
    resetOutputStreams();
    
    TestCapsule.reset();
  }
  
  @After
  public void tearDown() throws Exception {
    fs.close();
  }
  
  
  
  
  
  
  
  private static final String PS = System.getProperty("path.separator");
  
}

```

```
```

```
```


