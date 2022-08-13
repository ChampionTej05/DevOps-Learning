# Application Basics

### Types of languages

1. Compiled :
    * Java, C, C++.
    * Compilation + Decoded into m/c code.
    * They are built for m/c specific architecture
2. Interpreted :
    * Develop Source Code and Run using Interpreter
    * It is complied into Byte-Code before running
    * for Python, Compiler generates ByteCode(.pyc file) and interpreter converts m/c



## JAVA

1. To check java version `java -version`
2. Until version 8 it was named using "1.VersionNumber" and 9 onwards it is simply versionNumber
3. To set the PATH variable in JAVA for accessing java directly `export PATH=$PATH:/opt/jdk-13.0.2/bin` where /opt/jdk... is the path where binaries are stored

### What is JDK
1. JDK (Java Development Kit) helping develop and build
2. JDK provides tools for Develop , Build and Running JAVA Applications
  * Develop
    - JDB - to debug java application
    - javadoc - to write documentation
  * Build
    - javac - to compile java app
    - jar - to build those complied files into JAR files
  * Run
    - JRE - Java Runtime Environment to run the java application
    - java - command line utility to run app

### Building JAVA App

Steps to Build java app
  * `javac MyClass.java` compile
  * `javadoc MyClass.java` for generating documentation
  * `jar cf MyClass.jar MyClass.java..` for building the multiple class into package
  * `java -jar MyClass.jar` run the app or `java MyClass` if single class is present

1. Compiler converts the Source Code first into Intermediate Byte Code (.class) files
2. `.class` files are then run in JVM (JAVA Virtual Machine) to convert in m/c code.
3. JAR (Java Archive) helps combining JAVA multiple classes/dependencies into single package in the form of `.jar` file
4. For static files like html/css, JAR converts it into `.war` WAR (Web Archive) file
5. `jar cf outputFile listOFoffiles` --> listoffile is space separated.
6. Manifest file `META-INF/MANIFEST.INF` stores metadata of file.
  ```
  Manifest-Version : 1.0
  Created-By: 1.8.0-242 (Private Build)
  Main-Class: MyClass
  ```
  Main-Class property shows the entry point for the application
7. `java -jar MyApp.jar` to run complete APP or `java MyClass` to run single file
8. JAVADOC helps to build documentation of class using `javadoc -d doc Myclass.java`


### Build tools
Helps to automate build process like Maven, Gradle, ANT using config file
1. `ant` performs all specified steps
2. `ant compile jar` runs only compilation and building packing, skipping documentation
3. For Maven we use `pom.xml`
4. `sudo yum install -y ant`
5. `sudo yum install maven`
6. `mvn install` inside the app to build using MVN
7. `sudo mvn package` to package the app
8. To run using MVN `java -cp /opt/maven/my-app/target/my-app-1.0-SNAPSHOT.jar com.mycompany.app.App `


## Node JS

1. `node -v` displays the version
2. `node fileName` to run application
3. `npm -v`
4. `npm search fileName` lists all packages with names fileName
5. `npm install packageName`  install per project only
6. `npm install packageName --save-dev` only dev
7. `npm install packageName -g` global


## Python

1. `yum install python2` or `yum install python36`
2. `python3 -V` version
3. `pip show packagename` to show the package info including installation dir
4. `python2 -c "import sys; print(sys.path)"` shows the where python searches for package
5. `pip install flask --upgrade` upgrades the package
6. `pip uninstall flask` removes the package
7. `pip install -r requirement.txt` installs all the packages in file
