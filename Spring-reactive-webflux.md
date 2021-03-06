https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/
https://spring.io/guides/gs/reactive-rest-service/

Spring -Webflux

RESTful web service with Spring Webflux and a WebClient consumer of that service. 

https://github.com/spring-guides/gs-reactive-rest-service.git

WebFlux Handler:
Reactive handler uses 2 functions ServerRequest and ServerResponse

------------------------------------------------------------------
package hello;
import org.springframework.web.reactive.function.BodyInserters;
import org.springframework.web.reactive.function.server.ServerRequest;
import org.springframework.web.reactive.function.server.ServerResponse;
import reactor.core.publisher.Mono;

@Component
public class GreetingHandler {
  public Mono<ServerResponse> hello(ServerRequest request) {
    return ServerResponse.ok().contentType(MediaType.TEXT_PLAIN)
      .body(BodyInserters.fromValue("Hello, Spring!"));
  }
}
---------------------------------------------------------------------

Router:
Router used to define the request and handler mapping for the request. When you invoke the req "localhost:8080/hello" , 
this router will be called.
-----------------------------------------------------------------------
package hello;
import org.springframework.web.reactive.function.server.RequestPredicates;
import org.springframework.web.reactive.function.server.RouterFunction;
import org.springframework.web.reactive.function.server.RouterFunctions;
import org.springframework.web.reactive.function.server.ServerResponse;

@Configuration
public class GreetingRouter {

  @Bean
  public RouterFunction<ServerResponse> route(GreetingHandler greetingHandler) {
    return RouterFunctions
      .route(RequestPredicates.GET("/hello").and(RequestPredicates.accept(MediaType.TEXT_PLAIN)), greetingHandler::hello);
  }
}
-------------------------------------------------------------

WebClient:
Spring MVC RestTemplate class is, by nature, blocking. Consequently, we don’t want to use it in a reactive application. 
For reactive applications, Spring offers the WebClient class, which is non-blocking

WebClient can be used to communicate with non-reactive, blocking services, too.
----------------------------------------------------------------------------------
package hello;

import org.springframework.web.reactive.function.client.ClientResponse;
import org.springframework.web.reactive.function.client.WebClient;
import reactor.core.publisher.Mono;

public class GreetingWebClient {
  private WebClient client = WebClient.create("http://localhost:8080");

  private Mono<ClientResponse> result = client.get()
      .uri("/hello")
      .accept(MediaType.TEXT_PLAIN)
      .exchange();

  public String getResult() {
    return ">> result = " + result.flatMap(res -> res.bodyToMono(String.class)).block();
  }
}
------------------------------------------------------------------------------
exchange() method will immediately return a Mono<ClientResponse>, which will eventually represent the response from the server.
flatMap() call here is used to extract and convert the body of the response – using the standard Jackson ObjectMapper functionality. 
map() call is then used as we’d expect, to convert one value into another one.
Note: flatMap() call will not execute until the HTTP Response comes back, and the likewise the map() handler will not execute until the JSON has been parsed into a Map object.
