---

title: "Update a Tomcat Installation from 7.23 to 7.24"

menu:
  main:
    name: "Tomcat"
    identifier: "migration-guide-724-tomcat"
    parent: "migration-guide-724"

---

The following steps describe how to update the Camunda artifacts on a Tomcat server in a shared process engine setting.

Throughout the procedure, refer to the [update guide][update-guide]. If not already done, download the
[Camunda 7.24 Tomcat distribution][tomcat-distribution].

The update procedure takes the following steps:

1. Update the Camunda 7 core libraries.
2. Update optional Camunda 7 libraries.
3. Update web applications.

In each of the following steps, the identifier `$*_VERSION` refers to the current versions and the new versions of the artifacts.

# 1. Update the Camunda 7 core libraries

Replace the following libraries in the folder `$TOMCAT_HOME/lib/` with the new versions from the folder `$TOMCAT_DISTRIBUTION/lib/`:

* `camunda-engine-$PLATFORM_VERSION.jar`
* `camunda-bpmn-model-$PLATFORM_VERSION.jar`
* `camunda-cmmn-model-$PLATFORM_VERSION.jar`
* `camunda-dmn-model-$PLATFORM_VERSION.jar`
* `camunda-xml-model-$PLATFORM_VERSION.jar`
* `camunda-engine-dmn-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-api-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-juel-$PLATFORM_VERSION.jar`
* `camunda-engine-feel-scala-$PLATFORM_VERSION.jar`
* `camunda-juel-$PLATFORM_VERSION.jar`
* `camunda-commons-logging-$PLATFORM_VERSION.jar`
* `camunda-commons-typed-values-$PLATFORM_VERSION.jar`
* `camunda-commons-utils-$PLATFORM_VERSION.jar`
* `camunda-connect-connectors-all-$PLATFORM_VERSION.jar`
* `camunda-connect-core-$PLATFORM_VERSION.jar`
* `camunda-template-engines-freemarker-$PLATFORM_VERSION.jar`
* `feel-engine-$FEEL_ENGINE_VERSION-scala-shaded.jar`
* `freemarker-$FREEMARKER_VERSION.jar`
* `mybatis-$MYBATIS_VERSION.jar`

# 2. Update optional Camunda 7 libraries

In addition to the core libraries, there may be optional artifacts in `$TOMCAT_HOME/lib/` for LDAP integration, Camunda Connect, Camunda Spin, and scripting. If you use any of these extensions, the following update steps apply:

## LDAP integration

Copy the following library from `$TOMCAT_DISTRIBUTION/lib` to the folder `$TOMCAT_HOME/lib/`, if present:

* `camunda-identity-ldap-$PLATFORM_VERSION.jar`

## Camunda Connect plugin

Copy the following libraries from `$TOMCAT_DISTRIBUTION/lib` to the folder `$TOMCAT_HOME/lib/`, if present:

* `camunda-engine-plugin-connect-$PLATFORM_VERSION.jar`

## Camunda Spin

Copy the following libraries from `$TOMCAT_DISTRIBUTION/lib` to the folder `$TOMCAT_HOME/lib/`, if present:

* `camunda-spin-dataformat-all-$PLATFORM_VERSION.jar`
* `camunda-spin-core-$PLATFORM_VERSION.jar`
* `camunda-engine-plugin-spin-$PLATFORM_VERSION.jar`

## GraalVM JavaScript

Copy the following libraries from `$TOMCAT_DISTRIBUTION/lib` to the folder `$TOMCAT_HOME/lib/`, if present:

* `graal-sdk-$GRAAL_VERSION.jar`
* `icu4j-$ICU4J_VERSION.jar`
* `js-$GRAAL_VERSION.jar`
* `js-scriptengine-$GRAAL_VERSION.jar`
* `regex-$GRAAL_VERSION.jar`
* `truffle-api-$GRAAL_VERSION.jar`

## Groovy

Copy these libraries from `$TOMCAT_DISTRIBUTION/lib` to the folder `$TOMCAT_HOME/lib/`, if present:

* `groovy-$GROOVY_VERSION.jar`
* `groovy-jsr223-$GROOVY_VERSION.jar`
* `groovy-json-$GROOVY_VERSION.jar`
* `groovy-xml-$GROOVY_VERSION.jar`
* `groovy-templates-$GROOVY_VERSION.jar`

# 3. Update web applications

## Update REST API

The following steps are required to update the Camunda REST API on a Tomcat instance:

1. Undeploy an existing web application with a name like `camunda-engine-rest`.
2. Download the REST API web application archive from our [Artifact Repository][artifact-repository-restapi] Alternatively, switch to the private repository for the enterprise version (credentials from license required). 
2. Download the Camunda web application archive from our [Artifact Repository][artifact-repository-webapp]. Alternatively, switch to the private repository for the enterprise version (credentials from license required). Choose accordingly:
    * For [Tomcat 10](https://artifacts.camunda.com/ui/native/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-tomcat-jakarta/), the name of the artifact is `$PLATFORM_VERSION/camunda-webapp-tomcat-jakarta-$PLATFORM_VERSION.war`.
    * For [Tomcat 9](https://artifacts.camunda.com/ui/native/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-tomcat/), the name of the artifact is `$PLATFORM_VERSION/camunda-webapp-tomcat-$PLATFORM_VERSION.war`.

3. Deploy the web application archive to your Tomcat instance.

## Update Cockpit, Tasklist, and Admin

The following steps are required to update the Camunda web applications Cockpit, Tasklist, and Admin on a Tomcat instance:

1. Undeploy an existing web application with a name like `camunda-webapp`.
2. Download the Camunda web application archive from our [Artifact Repository][artifact-repository-webapp]. Alternatively, switch to the private repository for the enterprise version (credentials from license required). Choose accordingly:
    * For [Tomcat 10](https://artifacts.camunda.com/ui/native/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-tomcat-jakarta/), the name of the artifact is `$PLATFORM_VERSION/camunda-webapp-tomcat-jakarta-$PLATFORM_VERSION.war`.
    * For [Tomcat 9](https://artifacts.camunda.com/ui/native/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-tomcat/), the name of the artifact is `$PLATFORM_VERSION/camunda-webapp-tomcat-$PLATFORM_VERSION.war`.
3. Deploy the web application archive to your Tomcat instance.

[update-guide]: {{< ref "/update/minor/723-to-724/_index.md" >}}
[artifact-repository-restapi]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/camunda-engine-rest/7.24.0/camunda-engine-rest-7.24.0-tomcat.war
[artifact-repository-webapp]: https://artifacts.camunda.com/artifactory/camunda-bpm/org/camunda/bpm/webapp/camunda-webapp-tomcat/7.24.0/camunda-webapp-tomcat-7.24.0.war
[tomcat-distribution]: https://downloads.camunda.cloud/release/camunda-bpm/tomcat/7.24/
