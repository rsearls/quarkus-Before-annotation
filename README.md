# quarkus-Before-annotation
quarkus improper handling of annotation @Before

quarkus-arquillian does call @Before unless @RunAsClient is present

Arquillian processes all @Before annotations (org.junit.Before) when 
@RunAsClient annotation (org.jboss.arquillian.container.test.api.RunAsClient)
is not present and when present. quarkus-arquillian only processes the
@Before annotation when @RunAsClient is present.


Before running the test install this archive in your local repo.
Here is the cmd to do it.

mvn install:install-file \
   -Dfile=___FIX_THIS___/lib/utils-arquillian-utils-4.5.0-SNAPSHOT.jar \
   -DgroupId=org.jboss.resteasy \
   -DartifactId=utils-arquillian-utils \
   -Dversion=4.5.0-SNAPSHOT \
   -Dpackaging=jar 

Execution env.
    jdk 11.0.2
    mvn 3.6.0
    quarkus 1.2.0.Final
    
  The pom.xml file contains the minimum archives required to compile and run the tests.
  
  
 Test AsyncPostProcessingTest has @Before and no @RunAsClient.  Test fails
 because client is null.  The method is never called.
 
    @Before
    public void init() {
       client = (ResteasyClient)ClientBuilder.newClient();
    }
 
 Test JaxrsAsyncServletTest has the @Before and @RunAsClient annotations and
 the same method definition as above. The method is called.  The client is assigned.
 
  See pom.xml file lines 300 and 302.
  
  Debugging
  Set breakpoint at line 70 in AsyncPostProcessingTest.  Run test
  with debugger 
     mvn test -Dmaven.surefire.debug
  Breakpoint is never taken.
  
  
  Insert annotation  @RunAsClient after line 37.  Run test with debugger.
  Breakpoint is taken. 
  
  
  
  
  
  