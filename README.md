# Welcome to IATA Cargo-XML Java JAXB 

This repository contains Java JAXB classes generated for (some) IATA Cargo-XML messages.

Please note that the IATA Cargo-XML schema files are not part of this respository.

# What is IATA Cargo-XML?

The IATA Cargo-XML messaging is emerging as a preferred standard for the electronic communication between airlines and other air cargo stakeholders such as shippers, freight forwarders, ground-handling agents, and regulators, as well as customs and security agencies .
See [IATA Cargo-XML Standards](https://www.iata.org/en/programs/cargo/e/cargo-xml/) for more information.

# What is JAXB?

Jakarta XML Binding (short: JAXB) is a software framework that allows Java developers 
to map Java classes to XML representations and vice versa, especially parsing XML files
and retrieving their data in a structure way. JAXB is here used to provide a structure
of Java classes which reflects the XML-schema of the CargoXML messages.

# Developer information

## Requirements

This project is a Java Gradle project and only an installed JDK version 8 or younger.
Project has been tested with JDK versions 8 and OpenJDK 11 on Windows and MacOS but should
also run on Linux and younger Java JDK versions.
This project requires an internet connection to download required additional libraries.

## Usage of Package

Note that the generated classes contain references to classes from the
`javax.xml.bind.annotation` package. This package is included in Java 8 but no longer in Java 11.

### Version > 8.0
Versions younger than 8.0 contain the `jakarta.xml.bind-api` bundled within the library jar.
It is no longer required to declare a dependency when using the library.

### Version 8.0
When the generated classes are used with JDK 11, it is recommended to
add a dependency to the `jakarta.xml.bind-api`, e.g. in gradle with

    implementation 'jakarta.xml.bind:jakarta.xml.bind-api:2.3.3'

or for maven:

    <dependency>
        <groupId>jakarta.xml.bind</groupId>
        <artifactId>jakarta.xml.bind-api</artifactId>
        <version>2.3.3</version>
    </dependency>


## How to build from scratch

Building a JAR file with the JAXB classes requires access to the XML schema files 
which are not provided with this package but part of the 
[IATA Cargo-XML Toolkit](https://www.iata.org/en/publications/store/cargo-xml-toolkit/).

This project is designed to work with IATA Cargo-XML Toolkit 8 installed  
under Windows 10 64bit in the default directory.

Please note that the directory containing the schema files might change with different versions of 
the toolkit or with different versions of Windows.

**Important Note**: The schema files in the IATA Cargo-XML Toolkit are stored in ZIP archives.
These archives need to get unzipped prior the schema files can be processed. 
This only needs to be be done once and there is already a gradle task `unzipschema` provided for this.
After checkout / download of this package, one might want to trigger this once from commandline inside download directory via

    ./gradlew unzipschema

For the default directory containing the (unzipped!) CargoXML schema files, the JAR file with the generated
JAXB classes can be build by executing

    ./gradlew clean shadowJar

After successful building, the directory `build/libs` will contain a file `cargoxml-jaxb-$VERSION.jar` which contains only the JAXB classes and is ready to be published.  

## Building with non-default schema directory

The default schema directory is `%LOCALAPPDATA%\IATA\Reader\1.0.0.0\Library\CXML 8th\Downloads\Schemas`, 
which contains subdirectories for the different CargoXML messages, e.g. a
subdirectory `CXML-XFWB-3` containing the XML schema files for the XFWB-3.00 message.
By providing a property `schemadir` to the gradle process the default schema 
directory can be overwritten. 

Examples for schema subdirectories located under a different parent directory (with statement to unzip the schema files): 

### MacOS 
MacOS with schema subdirectories located under / copied into `$HOME/cargoxml-schema`:

    ./gradlew -Pschemadir=$HOME/cargoxml-schema unzipschema
    ./gradlew -Pschemadir=$HOME/cargoxml-schema clean shadowJar

### Windows 
Windows with Cargo-XML 4th Edition installed:

    gradlew -Pschemadir="%APPDATA%\IATA\IATA Cargo-XML Message Manual and Toolkit 4th Edition\Downloads\Schemas" unzipschema
    gradlew -Pschemadir="%APPDATA%\IATA\IATA Cargo-XML Message Manual and Toolkit 4th Edition\Downloads\Schemas" clean shadowJar


## Publishing the project
Publishing the generated JAR file from directory `build/libs` is required to 
provide the generated JAXB classes for 3rd partties.

### Publishing on GitHub
Easiest way of publishing is usage of publishing via [GitHub releases](../../releases)
on GitHub website and uploading the generated JAR file from directory `build/libs`.

Publishing is a **manual process** because building the jar requires local access to the schema directory.

Steps to perform:
1. create the version tag in `git`
2. execute `./gradlew -Pschemadir=$HOME/cargoxml-schema clean shadowJar` (or similar with adopted schema directory)
3. publish with generated JAR file from directory `build/libs` via [new Github releases](../../releases/new)


### Publishing to MavenLocal (for developers)
Project supports pubishing to local, which might be useful to generate a maven
`pom` file containing dependencies as required: 

    ./gradlew -Pschemadir=$HOME/cargoxml-schema clean publishToMavenLocal

### Versioning
Recommendation is to use a version pattern for releases which matches the versioning of the
underlying Cargo-XML Toolkit in the major release number, e.g. using version `8.0` to 
release a version which is based upon IATA Cargo-XML Toolkit 8. 
Minor and Patch releases should follow the [semantic versioning pattern](https://semver.org/)
with using version tags in git.

It is possible to overwrite the latest git version tag by providing the version directly to git, e.g.

    ./gradlew -Pversion=8.2-SNAPSHOT -Pschemadir=$HOME/cargoxml-schema clean shadowJar


### Publishing on MavenCentral
It's a great idea to make the generated JAR file available on 
[MavenCentral Repository](https://mvnrepository.com/repos/central)!
Sadly it's not included yet but feel free to add it to the build.gradle.
There are documentations out there about the details, e.g.
* https://www.albertgao.xyz/2018/01/18/how-to-publish-artifact-to-maven-central-via-gradle/
* https://madhead.me/posts/no-bullshit-maven-publish/


## FAQ

### Where to find the CargoXML Schema files in the official IATA XML Toolkit?

####   Cargo-XML 8th Edition:
In the Toolkit/IATA-Reader in the main navigation
bar `>Home< >Introduction< ..` at the rightmost place under `>Toolbox<`.
In the filesystem, the schema archives are available at
`"%LOCALAPPDATA%\IATA\Reader\1.0.0.0\Library\CXML 8th\Downloads\Schemas"`

####   Cargo-XML 4th Edition:
In the Toolkit in the main navigation bar `>Home< >Introduction< ..` at
the rightmost place under `>Download Section<`.
In the filesystem, the schema archives are available at
`"%APPDATA%\IATA\IATA Cargo-XML Message Manual and Toolkit 4th Edition\Downloads\Schemas"`

### Which Cargo-XML messages are supported?

The list of supported Cargo-XML messages is limited at the moment.
In section `xjc` of file `build.gradle` the different supported messages
types are registered for JAXB generation. The list could get enlarged by simply 
adding new groups with the matching schema file (relative path from $schemadir) 
and a matching defaultPackage.

This project is also prepared to be modified by copying schema details into `src/main/resources/schema` 
and enlarging the list accordingly. See also `build.gradle` for details.
