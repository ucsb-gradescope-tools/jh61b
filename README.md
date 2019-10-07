# Repo to separately compile jh61b-1.0.jar

This repo pulls out the code for the jh61b-1.0 library
so that it can be separately compiled into a jar that
can then be pulled into other Maven or Ant based Java
project for Gradescope autograding.

See intlist in this same organization for an example of how to use it.

To build the jar, run `mvn package` then find the jar in the `target` directory.

# To install jar for jh61b, use:

Use:

```
mvn install:install-file -Dfile=../jh61b/target/jh61b-1.0.jar
mvn package
mvn -q exec:java
```

The install command is described here, and is the way you get a third party
JAR into Maven.

* <https://maven.apache.org/guides/mini/guide-3rd-party-jars-local.html>

The rest is described below.

You also need to adjust this part of the pom.xml.  The main class needs to be a `RunTests` class (sample source code below.)

```
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.4.0</version>
        <executions>
          <execution>
            <goals>
              <goal>java</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <mainClass>com.gradescope.intlist.tests.RunTests</mainClass>
          <arguments>
          </arguments>
        </configuration>
      </plugin>

```

Sample Source for `RunTests` (which can be in any package you like):

The part you have to modify is the list of test classes in
the `@Suite.SuiteClasses` annotation.  Only those classes get picked up
by the grading stuff.

Then you also need to annotate your classes as shown in the sample classes
in this repo.


```
package com.gradescope.intlist.tests;

import org.junit.runner.RunWith;
import org.junit.runners.Suite;
import org.junit.runner.JUnitCore;
import org.junit.runner.Result;
// import junit.tests.framework.TestListenerTest
import com.gradescope.intlist.tests.IntListTest;
import com.gradescope.jh61b.grader.GradedTestListenerJSON;

@RunWith(Suite.class)
@Suite.SuiteClasses({
        IntListTest.class,
        IntListPredicates.class,
})
public class RunTests {
    public static void main(String[] args) {
        JUnitCore runner = new JUnitCore();
        runner.addListener(new GradedTestListenerJSON());
        // runner.addListener(new TestListenerTest());
        Result r = runner.run(RunTests.class);
    }
}
```
