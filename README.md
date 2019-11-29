<img src="https://cesarsotovalero.github.io/img/logos/depclean_logo.png" height="100px" />

[![Build Status](https://travis-ci.org/castor-software/royal-debloat.svg?branch=master)](https://travis-ci.org/castor-software/royal-debloat)


### What is `Depclean`?

`Depclean` is a tool to automatically remove dependencies that are included in the Maven dependency tree but are not actually used in Java projects. `Depclean` detects and removes all the unused dependencies declared in the `pom.xml` file of a project or imported from its parent. For that, it relies on bytecode static analysis and extends the `maven-dependency-analyze` plugin. The tool does not modify the original source code of the application nor its original `pom.xml`. It can be executed as a Maven goal through the command line or integrated directly into the Maven build lifecycle.

### How it works?

`Depclean` runs before executing the `package` phase of the Maven build lifecycle. It statically collects all the types referenced in the project under analysis as well as in its declared dependencies. Then, it compares the types that the project actually use in the bytecode with respect to the class members belonging to its dependencies.

With this usage information, `Depclean` constructs a new `pom.xml` based on the following steps:

1. add all used transitive dependencies as direct dependencies
2. remove all unused direct dependencies
3. exclude all unused transitive dependencies

If all the tests pass and the project builds correctly after this changes, then it means that the identified as bloated dependencies can be removed. `Depclean` produces a file named `pom-debloated.xml`, located in the root of the project, which is a clean version of the original `pom.xml` without bloated dependencies.

## Usage

### Prerequisites

- [Java OpenJDK 8](https://openjdk.java.net) or above
- [Apache Maven](https://maven.apache.org/)

### Installing and building from source

In a terminal clone the repository and switch to the cloned folder:

```bash
git clone https://github.com/castor-software/depclean.git
cd jdbl-pom
```
Then run the following Maven command to build the application and install the plugin locally:

```bash
mvn clean install
```
Once the plugin is installed, you can execute the plugin directly in the command line:

```shell script
mvn se.kth.depclean:depclean-maven-plugin:1.0.0:depclean
```

Alternatively, you can configure the `pom.xml` file of your Maven project to use `Depclean` as part of the build:

```xml
<plugin>
    <groupId>se.kth.depclean</groupId>
    <artifactId>depclean-maven-plugin</artifactId>
    <version>1.0.0</version>
    <executions>
        <execution>
            <goals>
                <goal>depclean</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## License

Distributed under the MIT License. See [LICENSE](https://github.com/castor-software/royal-debloat/blob/master/LICENSE) for more information.
