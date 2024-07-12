<img src="https://wcm.io/images/favicon-16@2x.png"/> AEM Cloud Service Dependencies - Mixin for JSONP 1.1
======
[![Build](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/workflows/Build/badge.svg?branch=develop)](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/actions?query=workflow%3ABuild+branch%3Adevelop)
[![Maven Central](https://img.shields.io/maven-central/v/io.wcm.maven/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11)](https://repo1.maven.org/maven2/io/wcm/maven/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/)

Maven dependencies for the AEM Cloud Service SDK

Documentation: https://wcm.io/tooling/maven/aem-dependencies.html<br/>
Issues: https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/issues<br/>
Wiki: https://wcm-io.atlassian.net/wiki/<br/>
Continuous Integration: https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/actions<br/>
Commercial support: https://wcm.io/commercial-support.html


## Usage

This POM defines Maven dependencies for AEM Cloud Service, including those that are not defined in the `aem-sdk-api` JAR provided by Adobe. Additionally, the POM includes Sling-internal dependencies required for [AEM Mocks](https://wcm.io/testing/aem-mock/) in exactly the versions included in the AEM version.

The version number of the artifact matches with the version of the `aem-sdk-api` artifact, plus a last part (4 digits) which is the revision number added by the wcm.io project, and is incremented if a fix or update of the POM needs to be published for the same SDK API version.

To import the dependencies in your AEM project use:

```xml
<dependency>
  <groupId>io.wcm.maven</groupId>
  <artifactId>io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11</artifactId>
  <version><!-- latest version, see above --></version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```

## Build from sources

If you want to build wcm.io from sources make sure you have configured all [Maven Repositories](https://wcm.io/maven.html) in your settings.xml.

See [Maven Settings](https://github.com/wcm-io/io.wcm.maven.aem-cloud-dependencies-mixin-jsonp11/blob/develop/.maven-settings.xml) for an example with a full configuration.

Then you can build using

```
mvn clean install
```
