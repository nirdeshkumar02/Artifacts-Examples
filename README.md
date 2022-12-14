Lang with their Build Tools
==================================
1. Java = Gradle/Maven
2. Node = NPM
3. Python = Pip
..... more

Build Java Project
===================
For Gradle 
--------------

Directory - java-app

The build command for gradle project is `./gradlew build`.
It will create a build folder and inside the folder under the lib folder there is a jar file.
This Jar file contains all your code with required dependencies.

The dependencies for gradle project is store in `build.gradle` file

For Maven
---------------

Directory - java-maven-app

You need to download and install the maven for build java project.
You need to create POM.xml which has all dependencies to run this maven project,
So for building the project you need to run `mvn install` which will download all the
dependencies written in pom.xml.
It will create a target folder and inside this there is a jar file. This Jar file contains all your code with required dependencies.

The dependencies for maven project is store in `pom.xml` file

Note - In both projects (Gradle or Maven) You will get a jar file and if you want to run this jar file then you need to execute a command "java -jar <your jar file>"

Build Node Project
=====================

Directory - react-nodejs-example

NPM and Yarn are package manager not build tools as Gradle and Maven are.
`Package.json` works like as dependency holder where project dependency are stored as `pom.xml` or `build.gradle`.
Run `npm pack` to create a tgz file
For React there is "webpack" which build the code and make it compress.
Run `npm run build` to build and convert the code into simple html, css and js

Common Pattern in all these tools
=====================================
1. Dependency file - package.json, pom.xml, build.gradle
2. Repo for dependency
3. Command line tool
4. Package Manager

To Upload Gradle Artifects to Nexus
======================================

 Change all the values to your values like Nexus User Creds, Nexus Repo Url

Create a file gradle.properties in gradle directory and add this data 

Nexus User Credential
```
repoUser = nirdesh
repoPassword = Nirdesh@123
```
Add this code to your build.gradle file

```
apply plugin: 'maven-publish'

publishing {
    publications {
        maven(MavenPublication) {
            artifact("build/libs/java-app-$version"+".jar") {
                extension 'jar'
            }
        }
    }

    repositories {
        maven {
            name 'nexus'
            url "http://13.232.237.54:8081/repository/maven-snapshots/"  # Nexus Repository Url
            allowInsecureProtocol true
            credentials {
                username project.repoUser  # Refrencing the Nexus User Credential
                password project.repoPassword
            }
        }
    }
}

```

Build the project - `./gradlew build`
Publish to Nexus - `./gradlew publish`

You will get the artifect in the above mention url.

To Upload Maven Artifects to Nexus
======================================

Create a file inside your window directory c/user/nksaini/.m2 => settings.xml and add this there

```
<settings>
    <servers>
        <server>
            <id>nexus-snapshots</id>   # Nexus Profile Name
            <username>nirdesh</username>  # Nexus User Credentials
            <password>Nirdesh@123</password>
        </server>
    </servers>
</settings>
```

Add this to your pom.xml file under build

```

        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-deploy-plugin</artifactId>
                    <version>2.8.2</version>
                </plugin>
            </plugins>
        </pluginManagement>

        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
            </plugin>
        </plugins>

```

Add this to your pom.xml under project

```

    <distributionManagement>
        <snapshotRepository>
            <id>nexus-snapshots</id> # Nexus Creds Profile Refrence
            <url>http://13.232.237.54:8081/repository/maven-snapshots/</url> # Nexus Repo Url
        </snapshotRepository>
    </distributionManagement>

```

Build the jar - `mvn package`
Publish to Nexus - `mvn deploy`

You will get the artifect in the above mention url.
Or go to nexus dashboard, browse on header, Browse, maven-snapshots, you will find your artifect 