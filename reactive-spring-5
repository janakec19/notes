https://stackify.com/reactive-spring-5/

What and Why Reactive Programming?
1. It’s a non-blocking alternative to traditional programming solutions.
2. This allow data changes in one part of the system to automatically update other parts of the system.
3. As reactive streams are non-blocking, the rest of the application doesn’t have to be waiting while the data is being processed. 
 –So they’re typically free to handle more incoming requests
 
 -------------------------------------------------------------------
 In Traditional way:
 List<User> users = userDao.getUsers();
 List<String> names = new ArrayList<String>();
 for (int i = 0; i < users.size(); ++i) {
   names.add(users.get(i).getName());
 }
 ----------------------------------------------------------------------
 In Reactive way:
 Flux<String> names  = reactiveUserDao.getUsers().map(user -> user.getName());
 --------------------------------------------------------------------------------
 For example, if this was a reactive web server then the thread handling the request will be immediately free to handle other requests, 
 and as the data appears from the database – it will be sent down to the client automatically.
 
 ------------------------------------------------------------------------------
 Back Pressure:
 The real key that makes Reactive programming a significant improvement over more traditional code is backpressure.
 This is the concept by which the producing end of the stream understands how much data the consuming end is capable of receiving,
 and is able to adjust its throughput accordingly.
 
 For example,  if the client connecting to our handler(using reactive repo) is running slow, then it can not consume data as quickly.
 This will cause backpressure down the reactive stream, which in turn will indicate to the database layer to stop sending the data as quickly.
 This can cause a slow client to reduce the load on the database server, all the way through the application layer, 
 which, in turn, can allow the database server to handle requests for other clients, making the entire system more efficient.
 ---------------------------------------------------------------------------------------------
 
 Reactive Flux and Mono
 Mono<T> is a stream of at most 1 value.
 Flux<T> is a stream of potentially infinite values.
 block() ,blockFirst() , blockLast() ,subscribe() ?
 
 Enabling support for Reactive MongoDB Repositories within Spring Data is done using 
  @EnableReactiveMongoRepositories annotation instead of the normal @EnableMongoRepositories.
