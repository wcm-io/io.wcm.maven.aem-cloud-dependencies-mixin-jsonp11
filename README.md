<img src="https://wcm.io/images/favicon-16@2x.png"/> AEM Cloud Service Dependencies - Mixin for JSONP 1.1
======
[![Build](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/workflows/Build/badge.svg?branch=develop)](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/actions?query=workflow%3ABuild+branch%3Adevelop)
[![Maven Central](https://img.shields.io/maven-central/v/io.wcm.maven/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11)](https://repo1.maven.org/maven2/io/wcm/maven/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/)

Overrides dependencies from AEM Cloud Service Dependencies to downgrade to JSONP 1.1 (using javax.json) instead of latest version (which uses jakarta.json).

Documentation: https://wcm.io/tooling/maven/aem-dependencies.html<br/>
Issues: https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/issues<br/>
Wiki: https://wcm-io.atlassian.net/wiki/<br/>
Continuous Integration: https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/actions<br/>
Commercial support: https://wcm.io/commercial-support.html


## Usage

This is a POM that can be used together with [AEM Cloud Dependencies][aem-cloud-dependencies]. It downgrades a couple of dependencies to switch back to Apache Johnzon 1.2.x and JSONP 1.1 (using `javax.json` instead of `jakarta.json`).

To use it, define an dependency to this POM *before* the declaration of the `io.wcm.maven.aem-cloud-dependencies` dependency. Example:

```xml
<dependency>
  <groupId>io.wcm.maven</groupId>
  <artifactId>io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11</artifactId>
  <version><!-- latest version, see above --></version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
<dependency>
  <groupId>io.wcm.maven</groupId>
  <artifactId>io.wcm.maven.aem-cloud-dependencies</artifactId>
  <version><!-- latest version --></version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```

## Background

Thw switch from JSONP 1.1 to JSONP 2.1 is a hard one, as it changes the package name of the API from `javax.json` instead of `jakarta.json` (besides this, functionality did not change). The problem is, that within AEM this changes does not happen at the same time for all modules, but individual modules do the switch in their own pace. At runtime in AEM this is not problem, as AEMaaCS has both Johnzon 2.x (JSONP 2.1) and Johnzon 1.2.x (JSONP 1.1) deployed in parallel, and each bundle imports what is required.

But when running unit tests with code that directly accesses either `javax.json` or `jakarta.json`, this is a problem when this code additionally depends on the Johnzon implementation. Johnzon 1.2.x and Johnzon 2.x use the same package names, so in the test classpath only one version can be present - and this either supports `javax.json` or `jakarta.json`, but not both at the same time.

As several Sling modules already switched from `javax.json` to `jakarta.json`, Sling Mocks already comes with dependencies both JSONP 1.1 and JSONP 2.1 API, and additionally brings in an alternative JSONP 2.1 implementation using `org.glassfish:jakarta.json`. With this, code using both JSONP API version can run in parallel, using Johnzon for JSONP 1.1 and Glassfish for JSONP 2.1. But if code in the test classpath depends on JSONP 2.1 and additionally on Johnzon-specific extensions at the same time, this breaks.

Since version `2024.7.17098.20240711T134106Z-240600.0000` the [AEM Cloud Dependencies][aem-cloud-dependencies] have switched to JSONP 2.1 and Johnzon 2.x by default, to ensure new code uses latest versions of the JSON implementation. However, if you have code, or depend on code still relying on JSONP1.1 and the old Johnzon version, you have to additionally define the `io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11` dependency as described above. This is especially true for the [AEM WCM Core Components][aem-core-wcm-components] implementation, which depends on JSONP 1.1 in some places (for compatibility with AEM 6.5), esp. in the implementation of the Image component.

The implementation of all wcm.io modules are not affected by this problem - it uses different JSON parser implementations (e.g. Jackson) to avoid this problem. This is also a recommendation for project code to stay away from both `javax.json` and `jakarta.json` until the migration is finally settled for all modules and implementations.


## Build from sources

If you want to build wcm.io from sources make sure you have configured all [Maven Repositories](https://wcm.io/maven.html) in your settings.xml.

See [Maven Settings](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/blob/develop/.maven-settings.xml) for an example with a full configuration.

Then you can build using

```
mvn clean install
```


[aem-cloud-dependencies]: https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies
[aem-core-wcm-components]: https://github.com/adobe/aem-core-wcm-components/