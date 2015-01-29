bin
===
scripts and other stuff to manage my computers

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

### Maven references
Maven by example http://books.sonatype.com/mvnex-book/
