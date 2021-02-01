This project showcases weird dependency behaviour as far as exclusions go.



There are two dependencies in this project:

* axual-platform-test-core (test scope)
* nimbus-jose-jwt



There is no additional source code, because we don't need it to showcase the issue.



When we comment out the `axual-platform-test-core` dependency, we get a dependency tree with three libs:

* nimbus-jose-jwt
* json-smart (dep of nimbus)
* accessors-smart (dep of nimbus)



When we undo the commen-out of "axual-platform-test-core" dependency, we get a dependency tree where the "json-smart" and "accessors-smart" deps have *dissappeared* from the Nimbus tree and are only dependencies of the "axual-platform-test-core".



Looking at the pom of "axual-platform-test-core", it excludes "accessors-smart" from its "json-smart" dependency and replaces it with another version:

 ```xml
<dependency>
<groupId>net.minidev</groupId>
<artifactId>json-smart</artifactId>
<version>2.2.1</version>
<scope>compile</scope>
<exclusions>
<exclusion>
<groupId>net.minidev</groupId>
<artifactId>accessors-smart</artifactId>
</exclusion>
</exclusions>
</dependency>
<dependency>
<groupId>net.minidev</groupId>
<artifactId>accessors-smart</artifactId>
<version>1.1</version>
<scope>compile</scope>
 ```



The real issue here is: **why does this have an impact on the compile scope dependency tree of my application?** Since I'm only using `axual-platform-test-core` in *test* scope, exclusions should have no impact on the *compile scope*, right?

