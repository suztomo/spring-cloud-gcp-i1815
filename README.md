# Example Project to reproduce issue in spring-cloud-gcp#1815

https://github.com/spring-cloud/spring-cloud-gcp/issues/1815

# To reproduce the issue

Ensure appengine-web.xml has following configuration:

```
  <class-loader-config>
    <priority-specifier filename="endpoints-management-control-appengine-all-1.0.11.jar"/>
  </class-loader-config>
```

Step 1. Run dev server

```
mvn appengine:run
...
[INFO] GCLOUD: INFO: Dev App Server is now running
```

Step 2. Access the hello URL. You get the error.

```
suztomo@suxtomo24:~$ curl localhost:8080/hello
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=utf-8"/>
<title>Error 500 Server Error</title>
</head>
<body><h2>HTTP ERROR 500</h2>
<p>Problem accessing /hello. Reason:
<pre>    Server Error</pre></p><h3>Caused by:</h3><pre>java.lang.NoSuchMethodError: io.opencensus.tags.TagContextBuilder.putPropagating(Lio/opencensus/tags/TagKey;Lio/opencensus/tags/TagValue;)Lio/opencensus/tags/TagContextBuilder;
	at com.example.appengine.java8.HelloAppEngine.doGet(HelloAppEngine.java:65)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:687)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:790)
	at org.eclipse.jetty.servlet.ServletHolder.handle(ServletHolder.java:873)
...
```

# To fix the issue

Step 1. Give priority to opencensus-api-0.23.0.jar

```
  <class-loader-config>
    <priority-specifier filename="opencensus-api-0.23.0.jar"/>
  </class-loader-config>
```

Step 2. Run dev server

(restart if you already have one running)

```
mvn appengine:run
...
[INFO] GCLOUD: INFO: Dev App Server is now running
```

Step 3. Access the hello URL. You don't get the error.

```
suztomo@suxtomo24:~$ curl localhost:8080/hello
Hello App Engine - Standard using 1.9.76 Java 1.8
```

