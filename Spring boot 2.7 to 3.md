
## 1. Overview[](https://www.baeldung.com/spring-boot-3-migration#overview)

In this tutorial, we’ll learn how to migrate a Spring Boot application to [version 3.0](https://www.baeldung.com/spring-boot-3-spring-6-new). To successfully migrate an application to Spring Boot 3, we have to ensure that the current version of Spring Boot of the application we’re migrating is 2.7 and the Java version is 17.

## 2. Core Changes[](https://www.baeldung.com/spring-boot-3-migration#core-changes)

Spring Boot 3.0 marks a major milestone for the framework, bringing several important modifications to its core components.

### 2.1. Configuration Properties[](https://www.baeldung.com/spring-boot-3-migration#1-configuration-properties)

Some property keys have been modified:

- _spring.redis_ has moved to _spring.data.redis_
- _spring.data.cassandra_ has moved to _spring.cassandra_
- _spring.jpa.hibernate.use-new-id-generator_ is removed
- [_server.max.http.header.size_](https://www.baeldung.com/spring-boot-max-http-header-size) has moved to _server.max-http-request-header-size_
- _spring.security.saml2.relyingparty.registration.{id}.identity-provider_ support is removed

**To identify those properties, we can add the _spring-boot-properties-migrator_** in our _pom.xml_:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-properties-migrator</artifactId>
    <scope>runtime</scope>
</dependency>
```

The latest version of [spring-boot-properties-migrator](https://central.sonatype.com/artifact/org.springframework.boot/spring-boot-properties-migrator/3.0.3) is available from Maven Central.

This dependency generates a report, printed at start-up time, of deprecated property names, and temporarily migrates the properties at runtime.

### 2.2. Jakarta EE 10[](https://www.baeldung.com/spring-boot-3-migration#2-jakarta-ee-10)

The new version of Jakarta EE 10 brings updates to the related dependencies of Spring Boot 3:

- Servlet specification updated to version 6.0
- JPA specification updated to version 3.1

So if we’re managing those dependencies by excluding them from the _spring-boot-starter_ dependency, we should make sure to update them.

Let’s start by updating the JPA dependency:

```xml
<dependency>
    <groupId>jakarta.persistence</groupId>
    <artifactId>jakarta.persistence-api</artifactId>
    <version>3.1.0</version>
</dependency>
```

The latest version of [jakarta.persistence-api](https://central.sonatype.com/artifact/jakarta.persistence/jakarta.persistence-api/3.1.0) is available from Maven Central.

Next, let’s update the Servlet dependency:

```xml
<dependency>
    <groupId>jakarta.servlet</groupId>
    <artifactId>jakarta.servlet-api</artifactId>
    <version>6.0.0</version>
</dependency>
```

The latest version of [jakarta.servlet-api](https://central.sonatype.com/artifact/jakarta.servlet/jakarta.servlet-api/6.0.0) is available from Maven Central.

**In addition to changes in the dependency coordinates, Jakarta EE now uses “_jakarta_” packages instead of “_javax_“.** So after we update our dependencies, we may need to update _import_ statements.

### 2.3. Hibernate[](https://www.baeldung.com/spring-boot-3-migration#3-hibernate)

In the case that we’re managing the Hibernate dependency by excluding it from the _spring-boot-starter_ dependency, it’s important to make sure to update it:

```xml
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>6.1.4.Final</version>
</dependency>
```

The latest version of [hibernate-core](https://central.sonatype.com/artifact/org.hibernate.orm/hibernate-core/6.1.4.Final) is available from Maven Central.

### 2.4. Other Changes[](https://www.baeldung.com/spring-boot-3-migration#4-other-changes)

Moreover, there are also other significant changes at the core level included in this release:

- Image banner supports removed: to define a [custom banner](https://www.baeldung.com/spring-boot-custom-banners), only the _banner.txt_ file is considered a valid file
- Logging date formatter: the new default date format for Logback and Log4J2 is _yyyy-MM-dd’T’HH:mm:ss.SSSXXX_  
    If we want to restore the old default format, we need to set the value of the property _logging.pattern.dateformat_ on _application.yaml_ to the old value
- _@ConstructorBinding_ only at the constructor level:  It’s no longer necessary to have [@_ConstructorBinding_](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/context/properties/ConstructorBinding.html) at the type level for _[@ConfigurationProperties](https://www.baeldung.com/configuration-properties-in-spring-boot)_ classes, and it should be removed.  
    However, if a class or record has multiple constructors, must utilise _@ConstructorBinding_ on the desired constructor to specify which one will be used for property binding.

## 3. Web Application Changes[](https://www.baeldung.com/spring-boot-3-migration#web-application-changes)

Initially, assuming that our application is a web application, we should consider certain changes.

### 3.1. Trailing Slash Matching Configuration[](https://www.baeldung.com/spring-boot-3-migration#1-trailing-slash-matching-configuration)

**The new Spring Boot release deprecates the option to configure trailing slash matching and sets its default value to _false_.**

For instance, let’s define a controller with one simple GET endpoint:

```java
@RestController
@RequestMapping("/api/v1/todos")
@RequiredArgsConstructor
public class TodosController {
    @GetMapping("/name")
    public List<String> findAllName(){
        return List.of("Hello","World");
    }
}
```

Now the “_GET /api/v1/todos/name/_” doesn’t match anymore by default and will result in an HTTP 404 error.

We can enable the trailing slash matching for all the endpoints by defining a new configuration class that implements _[WebMvcConfigurer](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/config/annotation/WebMvcConfigurer.html)_ or _[WebFluxConfigurer](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/reactive/config/WebFluxConfigurer.html)_ (in case it’s a reactive service):

```java
public class WebConfiguration implements WebMvcConfigurer {

    @Override
    public void configurePathMatch(PathMatchConfigurer configurer) {
        configurer.setUseTrailingSlashMatch(true);
    }

}
```

### 3.2. Response Header Size

As we already mentioned, the property [_server.max.http.header.size_](https://www.baeldung.com/spring-boot-max-http-header-size) is deprecated in favour of _server.max-http-request-header-size,_ which checks only the size of the request header. To define a limit also for the response header, let’s define a new bean:

```java
@Configuration
public class ServerConfiguration implements WebServerFactoryCustomizer<TomcatServletWebServerFactory> {

    @Override
    public void customize(TomcatServletWebServerFactory factory) {
        factory.addConnectorCustomizers(new TomcatConnectorCustomizer() {
            @Override
            public void customize(Connector connector) {
                connector.setProperty("maxHttpResponseHeaderSize", "100000");
            }
        });
    }
}
```

If we’re using Jetty instead of Tomcat, we should change [_TomcatServletWebServerFactory_](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/embedded/tomcat/TomcatServletWebServerFactory.html) to [_JettyServletWebServerFactory_](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/embedded/jetty/JettyServletWebServerFactory.html). Furthermore, the others embedded web containers don’t support this feature.

### 3.3. Other Changes[](https://www.baeldung.com/spring-boot-3-migration#3-other-changes)

Moreover, there are also other significant changes at the web application level in this release:

- Phases on graceful shutdown: the SmartLifecycle implementations for graceful shutdown have updated the phases. Spring now initiates graceful shutdown in the phase _SmartLifecycle.DEFAULT_PHASE – 2048_ and stops the web server in the phase _SmartLifecycle.DEFAULT_PHASE – 1024_.
- _HttpClient_ upgrade on [_RestTemplate_](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html): Rest Template updates its version of Apache [HttpClient5](https://hc.apache.org/httpcomponents-client-5.2.x/)

## 4. Actuator Changes[](https://www.baeldung.com/spring-boot-3-migration#actuator-changes)

Some significant changes have been introduced in the [Actuator](https://www.baeldung.com/spring-boot-actuators) module.

### 4.1. Actuator Endpoints Sanitization[](https://www.baeldung.com/spring-boot-3-migration#1-actuator-endpoints-sanitization)

In previous versions, Spring Framework automatically masks the values for sensitive keys on the endpoints _/env_ and _/configprops_, which display sensitive information such as configuration properties and environment variables.  
In this release, Spring changes the approach to be more secure by default.

**Instead of only masking certain keys, it now masks the values for all keys by default.** We can change this configuration by setting the properties _management.endpoint.env.show-values_ (for the _/env_ endpoint) or _management.endpoint.configprops.show-values_ (for the /_configprops_ endpoint) with one of these values:

- _NEVER_: no values shown
- _ALWAYS_: all the values shown
- _WHEN_AUTHORIZED_: all the values are shown if the user is authorized. For [JMX](https://www.baeldung.com/java-management-extensions), all the users are authorized. For HTTP, only a specific role can access the data.

### 4.2. Other Change[](https://www.baeldung.com/spring-boot-3-migration#2-other-change)

Other relevant updates have occurred the on Spring Actuator Module:

- Jmx Endpoint Exposure: JMX handles only the health endpoint. By configuring the properties _management.endpoints.jmx.exposure.include_ and _management.endpoints.jmx.exposure.exclude_, we can customize it.
- _httptrace_ endpoint renamed: this release renamed the “_/httptrace_” endpoint to “_/httpexchanges_“
- Isolated _ObjectMapper_: this release now isolates the _ObjectMapper_ instance responsible for serializing the responses of the actuator endpoint. We may change this feature by setting the _management.endpoints.jackson.isolated-object-mapper_ property to _false_.

## 5. Spring Security[](https://www.baeldung.com/spring-boot-3-migration#spring-security)

**Spring Boot 3 is compatible only with Spring Security 6.  
**

Before upgrading to Spring Boot 3.0, we should first [upgrade our Spring Boot 2.7 application to Spring Security 5.8](https://docs.spring.io/spring-security/reference/5.8/migration/index.html).  
Afterwards, we can upgrade Spring Security to version 6 and Spring Boot 3.

A few important changes have been introduced in this version:

- _[ReactiveUserDetailsService](https://docs.spring.io/spring-security/site/docs/current/api/org/springframework/security/core/userdetails/ReactiveUserDetailsService.html)_ not autoconfigured: in presence of an _[AuthenticationManagerResolver](https://www.baeldung.com/spring-security-authenticationmanagerresolver),_ a _ReactiveUserDetailsService_ is no longer autoconfigured.
- [SAML2](https://www.baeldung.com/cs/saml-introduction) Relying Party Configuration: we previously mentioned that the new version of Spring boot no longer supports properties located under _spring.security.saml2.relyingparty.registration.{id}.identity-provider_.  
    Instead, we should use the new properties under _spring.security.saml2.relyingparty.registration.{id}.asserting-party_.

## 6. Spring Batch[](https://www.baeldung.com/spring-boot-3-migration#spring-batch)

Let’s see some significant changes introduced in the [Spring Batch](https://www.baeldung.com/spring-boot-spring-batch) module.

### 6.1. _@_E_nableBatchProcessing_ Discouraged[](https://www.baeldung.com/spring-boot-3-migration#1-enablebatchprocessing-discouraged)

Previously, we could enable [Spring Batch](https://www.baeldung.com/spring-boot-spring-batch)’s auto-configuration, annotating a configuration class with _[@EnableBatchProcessing](https://docs.spring.io/spring-batch/docs/current/api/org/springframework/batch/core/configuration/annotation/EnableBatchProcessing.html)_. **The new release of Spring Boot discourages the usage of this annotation if we want to use autoconfiguration.**

In fact, using this annotation (or defining a bean that implements [_DefaultBatchConfiguration_](https://docs.spring.io/spring-batch/docs/current/api/org/springframework/batch/core/configuration/support/DefaultBatchConfiguration.html)) tells the autoconfiguration to back off.

### 6.2. Running Multiple Jobs[](https://www.baeldung.com/spring-boot-3-migration#2-running-multiple-jobs)

Previously, it was possible to run multiple batch jobs at the same time using Spring Batch. However, this is no longer the case. **If the auto-configuration detects a single job, it will be executed automatically when the application starts.**

So if multiple jobs are present in the context, we’ll need to specify which job should be executed on startup by providing the name of the job using the _spring.batch.job.name_ property. Therefore, this means that if we want to run multiple jobs, we have to create a separate application for each job.

Alternatively, we can use a scheduler like Quartz, Spring Scheduler, or other alternatives for scheduling the jobs.

## 7. Conclusion[](https://www.baeldung.com/spring-boot-3-migration#conclusion)

In this article, we learned how to migrate a 2.7 Spring Boot app to version 3, focusing on the core components of the Spring environment. Some [changes have occurred in other modules,](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide) like Spring Session, Micrometer, Dependency management, etc.

As always, the complete source code of the examples can be found [over on GitHub](https://github.com/eugenp/tutorials/tree/master/spring-boot-modules/spring-boot-3).