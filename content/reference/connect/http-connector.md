---

title: 'HTTP Connector'
weight: 10

menu:
  main:
    identifier: "connect-ref-http-connector"
    parent: "connect-ref"

---

In Camunda Connect a `Connectors` class exists which automatically detects
every connector in the classpath. It can be used to get the HTTP connector
instance by its connector ID, which is `http-connector`.

```java
HttpConnector http = Connectors.getConnector(HttpConnector.ID);
```


# Configure Apache HTTP Client

Camunda Connect HTTP client uses the Apache HTTP client to make HTTP requests. Accordingly, it supports the same configuration options.

## Default Configuration

By default, the HTTP client uses Apache's default configuration and respects the [system properties that are supported by HTTP client](https://hc.apache.org/httpcomponents-client-5.3.x/current/httpclient5/apidocs/org/apache/hc/client5/http/impl/classic/HttpClientBuilder.html).

## Custom Configuration

If you want to reconfigure the client going beyond the default configuration options, e.g. you want to configure another connection manager, the easiest way is to register
a new connector configurator.

```java
package org.camunda.connect.example;

import org.apache.hc.client5.http.impl.classic.CloseableHttpClient;
import org.apache.hc.client5.http.impl.classic.HttpClients;
import org.apache.hc.client5.http.impl.io.PoolingHttpClientConnectionManager;
import org.camunda.connect.httpclient.HttpConnector;
import org.camunda.connect.httpclient.impl.AbstractHttpConnector;
import org.camunda.connect.spi.ConnectorConfigurator;

public class HttpConnectorConfigurator implements ConnectorConfigurator<HttpConnector> {

  public Class<HttpConnector> getConnectorClass() {
    return HttpConnector.class;
  }

  public void configure(HttpConnector connector) {
    PoolingHttpClientConnectionManager connectionManager = new PoolingHttpClientConnectionManager();
    connectionManager.setDefaultMaxPerRoute(10);
    connectionManager.setMaxTotal(200);
    
    CloseableHttpClient client = HttpClients.custom()
      .setConnectionManager(connectionManager)
      .build();
    ((AbstractHttpConnector) connector).setHttpClient(client);
  }

}
```

To enable auto detection of your new configurator please add a file called
`org.camunda.connect.spi.ConnectorConfigurator` to your
`resources/META-INF/services` directory with class name as content. For more
information see the [extending Connect]({{< ref "/reference/connect/extending-connect.md" >}}) section.

```
org.camunda.connect.example.HttpConnectorConfigurator
```

{{< note title="Apache HTTP Client 5.x Migration" class="info" >}}
Starting from Camunda 7.24, the Connect HTTP connector uses Apache HTTP Client 5.x. While the high-level Connect API remains the same, custom configurations and extensions need to use the new HttpClient 5.x packages (`org.apache.hc.client5.*` and `org.apache.hc.core5.*`) instead of the previous 4.x packages (`org.apache.http.*`).
{{< /note >}}

# Requests

## Create a Simple HTTP Request

The HTTP connector can be used to create a new request, set a HTTP method, URL,
content type and payload.

A simple GET request:

```java
http.createRequest()
  .get()
  .url("http://camunda.org")
  .execute();
```

A POST request with a content type and payload set:

```java
http.createRequest()
  .post()
  .url("http://camunda.org")
  .contentType("text/plain")
  .payload("Hello World!")
  .execute();
```

The HTTP methods PUT, DELETE, PATCH, HEAD, OPTIONS, TRACE
are also available.


## Adding HTTP Headers to a Request

To add own headers to the HTTP request the method `header` is
available.

```java
HttpResponse response = http.createRequest()
  .get()
  .header("Accept", "application/json")
  .url("http://camunda.org")
  .execute();
```

## Enabling HTTP Response Error Handling

By default, the HTTP connector does not seamlessly handle 4XX and 5XX related response errors during HTTP call.
To activate the handling of these errors without additional scripting, set the `throw-http-error` property to `TRUE` via the `configOption` method. Once enabled, the client will throw an exception in case of http response errors (status code 400-599).

```java
HttpResponse response = http.createRequest()
  .get()
  .configOption("throw-http-error", "TRUE")
  .url("http://camunda.org")
  .execute();
```

## Using the Generic API

Besides the configuration methods also a generic API exists to
set parameters of a request. The following parameters are
available:

<table class="table table-striped">
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>method</td>
    <td>Sets the HTTP method of the request</td>
  </tr>
  <tr>
    <td>url</td>
    <td>Sets the URL of the request</td>
  </tr>
  <tr>
    <td>headers</td>
    <td>Contains a map of the configured HTTP headers of the request</td>
  </tr>
  <tr>
    <td>payload</td>
    <td>Sets the payload of the request</td>
  </tr>
</table>

This can be used as follows:

```java
HttpRequest request = http.createRequest();
request.setRequestParameter("method", "GET");
request.setRequestParameter("url", "http://camunda.org");
request.setRequestParameter("payload", "hello world!");
```

# Response

A response contains the status code, response headers and body.

```java
Integer statusCode = response.getStatusCode();
String contentTypeHeader = response.getHeader("Content-Type");
String body = response.getResponse();
```

After the response was processed it should be closed.

```java
response.close()
```

## Using the Generic API

Besides the response methods a generic API is provided
to gather the response parameters. The following parameters
are available:

<table class="table table-striped">
  <tr>
    <th>Parameter</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>statusCode</td>
    <td>Contains the status code of the response</td>
  </tr>
  <tr>
    <td>headers</td>
    <td>Contains a map with the HTTP headers of the response</td>
  </tr>
  <tr>
    <td>response</td>
    <td>Contains the response body</td>
  </tr>
</table>

This can be used as follows:

```java
response.getResponseParameter("statusCode");
response.getResponseParameter("headers");
response.getResponseParameter("response");
```
