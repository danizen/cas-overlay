CAS Overlay
============================

An overlay of Jasig CAS, built using Gradle on top of CAS 4.2.7

## Version

* CAS 4.2.7

## Requirements

* JDK (currently 1.8)
* SDKMAN with Gradle (currently 3.3)

## Configuration

The `etc` directory contains the configuration files that need to be copied to `/etc/cas`
and configured to satisfy local deployment environment configuration needs.

### Adding Modules

CAS modules may be specified under the `dependencies` block of the [CAS subproject](cas/build.gradle):

```gradle
dependencies {
    compile "org.apereo.cas:cas-server-webapp:${project.'cas.version'}@war"
    compile "org.apereo.cas:cas-server-some-module:${project.'cas.version'}"
    ...
}
```

Study material:

- https://docs.gradle.org/current/userguide/artifact_dependencies_tutorial.html
- https://docs.gradle.org/current/userguide/dependency_management.html

## Build

```bash
gradle clean build
```

To produce an exploded war directory (convenient during development, etc.)

```bash
gradle clean build explodeWar
```

## Deployment

### Embedded Jetty

- Create a Java keystore under `/etc/cas/jetty`
- Import your CAS server certificate inside this keystore.
- Edit your `~/.gradle/gradle.properties` to include:

```properties
jettySslKeyStorePath=/etc/cas/jetty/thekeystore
jettySslTrustStorePath=/etc/cas/jetty/thekeystore
jettySslTrustStorePassword=changeit
jettySslKeyStorePassword=changeit
```

Then run:

```bash
gradle clean build jettyRunWar
```

CAS will be available at:

- http://cas.server.name:8080/cas
- https://cas.server.name:8443/cas

If you do not specify a keystore configuration, CAS will simply run on port `8080`.

### External

Deploy resultant `cas/build/libs/cas.war` to a Servlet container of choice.

Remember to start your container with the following variables set with `-D`:

```properties
cas.properties.config.location=file:/etc/cas/cas.properties
log4j.configurationFile=/etc/cas/log4j2.xml
```
