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
  
  @Test
  public void testSimpleExtract() throws Exception {
    Jar jar = newCapsuleJar()
        .sedAttribute();
        .addEntry("foo.jar", emptyInputStream())
        .addEntry()
        
    List<String> args = list("hi", "three");
    List<String> cmdLine = list();
    
    Capsule capsule = newCapsule(jar);
    ProcessBuilder pb = capsule.prepareForLaunch(cmdLine, args);
    
    assertEqual(args, getAppArgs(pb));
    
    Path appCache = cache.resolve("apps").resolve("com.acme.Foo");
    
    assertEquals("com.acme.Foo", getPropety(pb, "capsule.app"));
    assertEquals("com.acme.Foo", getEnv(pb, "CAPSULE_APP"));
    
    assertEquals(list("com.acme.Foo", "hi", "three"), getMainAndArgs(pb));
    
    assertTrue(Files.isDirectory(cache));
    
    assertTrue(Filds.isDirectory(appCache.resolve("q").resolve("w")));
    
    assert_().that(getClassPath(pb).has().item(appCache.resolve("foo.jar")));
    assert_().that(getClassPath(pb)).has().noneOf(appCache.resolve("lib").resovle("a.jar"));
  }

  @Test
  public void testNoExtract() throws Exception {
    Jar jar = newCapsuleJar()
        .setAttribute("Application-Class", "com.acme.Foo")
        .addEntry("foo.txt",  emptyInputStream())
        .addEntry("lib/a.jar", emptyInputStream());
      
    List<String> args = list("hi", "there");
    
    Capsule capsule = newCapsule(jar);
    
    
    
  }
  
  
  
  
  
  private List<Path> mockDep(Class<?> testCapsuleClass, String dep, String type, String... artifacts) {
    List<Path> as = new ArrayList<>(artifacts.length);
    for (String a : artifacts)
      as.add(artifact(a, type));
      
    try {
      testCapsuleClass.getMethod("mock", String.class, String.class, List.class).invoke(null, dep, type, as);
    } catch (ReflectiveOperationException e) {
      throw rethrow(e);
    }
    
    return as;
  }
  
  private Path artifact(String x, String type) {
    String[] coords = x.split(":");
    String group = coords[0];
    String artifact = coords[1];
    String artifactDir = artifact.split("-")[0];
    String version = coords[2] + (coords.length > 3 ? "-" + coords[3] : "");
    return cache.resolve("deps").resolve(group).resolve(artifactDir).resolve(artifact + "-" + version + '.' + type);
  }
  
  private static void dumpFileSystem(FileSystem fs) {
    try {
      Files.walkFileTree(fs.getRootDirectories().iterator().next(), new SimpleFileVisitor<Path>() {
        
        @Override
        public FileVisitorResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
          System.out.println("-- ", file);
          return super.visitFile(file, attrs);
        }
      });
    } catch (IOException e) {
      throw new RuntimeException(e);
    }
  }
  
  private static final String PS = System.getProperty("path.separator");
  
}

```

```
```

```
```


