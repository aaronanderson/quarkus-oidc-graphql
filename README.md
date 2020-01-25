# Quarkus 1.2.0.CR1 OIDC and GraphQL issue

## Keycloak Setup

start keycloak:

` docker run --name keycloak -e KEYCLOAK_USER=admin -e KEYCLOAK_PASSWORD=admin -e DB_VENDOR=h2 -p 8180:8080 jboss/keycloak

Download https://github.com/quarkusio/quarkus-quickstarts/blob/master/security-openid-connect-quickstart/config/quarkus-realm.json
log into http://localhost:8180 as admin/admin

In the top left realm drop down select Add Realm

Select the quarkus-realm.json file and click create

Navigate to Clients -> backend-service, and on the Settings tab in the Valid Redirect URIs field add `http://localhost:8080/` and click Save. 


## Running the Example

Build the project and start Quarkus devmode

`mvn clean quarkus:dev`

Access `http://localhost:8080`

Authenticate with the credentials admin/admin

The page will load but the results area will be empty. The following exception should be present in the console:


```
2020-01-24 23:57:27,246 ERROR [io.qua.ver.htt.run.QuarkusErrorHandler] (executor-thread-127) HTTP Request to /graphql failed, error id: 940cdc5b-8421-47bf-a80b-5f9a5e008e43-1: java.lang.IllegalStateException: Request has already been read
	at io.vertx.core.http.impl.HttpServerRequestImpl.checkEnded(HttpServerRequestImpl.java:600)
	at io.vertx.core.http.impl.HttpServerRequestImpl.handler(HttpServerRequestImpl.java:307)
	at io.vertx.core.http.HttpServerRequest.bodyHandler(HttpServerRequest.java:215)
	at io.vertx.ext.web.impl.HttpServerRequestWrapper.bodyHandler(HttpServerRequestWrapper.java:245)
	at io.vertx.ext.web.handler.graphql.impl.GraphQLHandlerImpl.handle(GraphQLHandlerImpl.java:86)
	at io.vertx.ext.web.handler.graphql.impl.GraphQLHandlerImpl.handle(GraphQLHandlerImpl.java:48)
	at io.quarkus.vertx.http.runtime.ResumeHandler.handle(ResumeHandler.java:19)
	at io.quarkus.vertx.http.runtime.ResumeHandler.handle(ResumeHandler.java:6)
	at io.vertx.ext.web.impl.RouteState.handleContext(RouteState.java:1034)
	at io.vertx.ext.web.impl.RoutingContextImplBase.iterateNext(RoutingContextImplBase.java:131)
	at io.vertx.ext.web.impl.RoutingContextImpl.next(RoutingContextImpl.java:130)
	at io.vertx.ext.web.handler.graphql.impl.ApolloWSHandlerImpl.handle(ApolloWSHandlerImpl.java:113)
	at io.vertx.ext.web.handler.graphql.impl.ApolloWSHandlerImpl.handle(ApolloWSHandlerImpl.java:49)
	at io.quarkus.vertx.http.runtime.ResumeHandler.handle(ResumeHandler.java:19)
	at io.quarkus.vertx.http.runtime.ResumeHandler.handle(ResumeHandler.java:6)
	at io.vertx.ext.web.impl.RouteState.handleContext(RouteState.java:1034)
	at io.vertx.ext.web.impl.RoutingContextImplBase.iterateNext(RoutingContextImplBase.java:131)
	at io.vertx.ext.web.impl.RoutingContextImpl.next(RoutingContextImpl.java:130)
	at io.quarkus.vertx.http.deployment.devmode.VertxHotReplacementSetup.handleHotReplacementRequest(VertxHotReplacementSetup.java:40)
	at io.vertx.ext.web.impl.RouteState.handleContext(RouteState.java:1034)
	at io.vertx.ext.web.impl.RoutingContextImplBase.iterateNext(RoutingContextImplBase.java:131)
	at io.vertx.ext.web.impl.RoutingContextImpl.next(RoutingContextImpl.java:130)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer.doPermissionCheck(HttpAuthorizer.java:120)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer.access$000(HttpAuthorizer.java:24)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer$3.apply(HttpAuthorizer.java:139)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer$3.apply(HttpAuthorizer.java:126)
	at java.base/java.util.concurrent.CompletableFuture.uniHandle(CompletableFuture.java:930)
	at java.base/java.util.concurrent.CompletableFuture.uniHandleStage(CompletableFuture.java:946)
	at java.base/java.util.concurrent.CompletableFuture.handle(CompletableFuture.java:2337)
	at java.base/java.util.concurrent.CompletableFuture.handle(CompletableFuture.java:143)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer.doPermissionCheck(HttpAuthorizer.java:126)
	at io.quarkus.vertx.http.runtime.security.HttpAuthorizer.checkPermission(HttpAuthorizer.java:93)
	at io.quarkus.vertx.http.runtime.security.HttpSecurityRecorder$2.handle(HttpSecurityRecorder.java:84)
	at io.quarkus.vertx.http.runtime.security.HttpSecurityRecorder$2.handle(HttpSecurityRecorder.java:76)
	at io.vertx.ext.web.impl.RouteState.handleContext(RouteState.java:1034)
	at io.vertx.ext.web.impl.RoutingContextImplBase.iterateNext(RoutingContextImplBase.java:131)
	at io.vertx.ext.web.impl.RoutingContextImpl.next(RoutingContextImpl.java:130)
	at io.quarkus.vertx.http.runtime.security.HttpSecurityRecorder$1$1.apply(HttpSecurityRecorder.java:67)
	at io.quarkus.vertx.http.runtime.security.HttpSecurityRecorder$1$1.apply(HttpSecurityRecorder.java:36)
	at java.base/java.util.concurrent.CompletableFuture.uniHandle(CompletableFuture.java:930)
	at java.base/java.util.concurrent.CompletableFuture$UniHandle.tryFire(CompletableFuture.java:907)
	at java.base/java.util.concurrent.CompletableFuture.postComplete(CompletableFuture.java:506)
	at java.base/java.util.concurrent.CompletableFuture.complete(CompletableFuture.java:2144)
	at io.quarkus.security.runtime.QuarkusIdentityProviderManagerImpl$1$1.run(QuarkusIdentityProviderManagerImpl.java:55)
	at io.quarkus.runtime.CleanableExecutor$CleaningRunnable.run(CleanableExecutor.java:224)
	at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515)
	at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
	at org.jboss.threads.ContextClassLoaderSavingRunnable.run(ContextClassLoaderSavingRunnable.java:35)
	at org.jboss.threads.EnhancedQueueExecutor.safeRun(EnhancedQueueExecutor.java:2011)
	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.doRunTask(EnhancedQueueExecutor.java:1535)
	at org.jboss.threads.EnhancedQueueExecutor$ThreadBody.run(EnhancedQueueExecutor.java:1426)
	at org.jboss.threads.DelegatingRunnable.run(DelegatingRunnable.java:29)
	at org.jboss.threads.ThreadLocalResettingRunnable.run(ThreadLocalResettingRunnable.java:29)
	at java.base/java.lang.Thread.run(Thread.java:830)
	at org.jboss.threads.JBossThread.run(JBossThread.java:479)



```



## Disabling OIDC resolves the issue

Edit the pom.xml file, comment out the OIDC extension

```
	<!-- <dependency>
			<groupId>io.quarkus</groupId>
			<artifactId>quarkus-oidc</artifactId>
			<version>${quarkus.version}</version>
		</dependency> -->
```


run the command `mv src/main/resources/application.properties src/main/resources/application-oidc.properties`

Restart Quarkus

Refresh the page and the GraphQL query succeeds.


## GraphQL UI Browser - Optional

Access http://localhost:8080/graphql-ui, paste in the request:

```
query{
  hello
}
```

And run the query. The result should be :

```
{
  "data": {
    "hello": "world"
  }
}

```
