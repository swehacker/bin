## Maven

### Maven help
Getting help with maven from your command prompt.

List the profiles that are active for the build
```
help:active-profiles
```

Displays the effective POM with the active profiles factored in
```
help:effective-pom
```

Prints out the calculated settings w. any given profile.
```
help:effective-settings
```

Describes the attributes of a plugin (groupId and artifactoryId)
```
help:describe
```

#### Example of usage
Show full description fo a plugin (in this case the help plugin itself)
```
mvn help:describe -Dplugin=help -Dfull
```

Get information of a single goal
```
mvn help:describe -Dplugin=compiler -Dmojo=compile -Dfull
```

### Dependencies
List all dependencies in a project
```
mvn dependencies:resolve
```

Analyze the dependencies:
```
mvn dependency:analyze
```

Get a dependency tree
```
mvn dependency:tree
```

### Test
#### Ignoring test failures
If your are building software guided with test, then it can be annoying to have the test fail every time you build, this is how to ignore test failures.
```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <configuration>
        <testFailureIgnore>true</testFailureIgnore>
      </configuration>
    </plugin>
  <plugins>
</build>
```

You can also set it directly on the command line:
```
mvn test -Dmaven.test.failure.ignore=true
```

#### Skipping unit tests
```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
      <configuration>
        <skip>true</skip>
      </configuration>
    </plugin>
  <plugins>
</build>
```

or by adding it to the command line:
```
mvn test -Dmaven.test.skip=true
```
### Multi module project and versioning
To simplify the versioning in multi pom project you can use the built-in properties for the project version and groupId.
```
<dependency>
  <groupId>${project.groupId}</groupId>
  <artifactId>my-sub-module</artifactId>
  <groupId>${project.version}</groupId>
```

### Maven references
Maven by example http://books.sonatype.com/mvnex-book/
