# π­ Ktor Routing Extensions
> *Extension to Ktorβs routing system to add object-oriented routing and much more. π*

## Why?
This extension library was created to reduce the amount of boilerplate to install routing to Noelware's products and services. We use **Ktor** for our major platforms like [Tsubaki](https://github.com/arisuland/tsubaki), [charted-server](https://github.com/charted-dev/charted), [analytics-server](https://github.com/Noelware/analytics-server) and much more.

## Features
- Reflection loading - Since we have interfaces to extend routing, this adds loading with [Koin](https://insert-koin.io), Kodein, and with the **Reflections** library.
- OOP-based routing - By "OOP-based," I mean it adds a `AbstractEndpoint` interface to create routing with annotations.

## Example
```kotlin
@Serializable
class RequestBody(val id: Int, val email: String)

class MyEndpoint: AbstractEndpoint() {
  init {
    install(SomeRoutingPlugin) {
      // type-safe config
    }
  }

  @Get
  suspend fun get(call: ApplicationCall) {
    call.respond(HttpStatusCode.OK, "Hello, world!")
  }
  
  @Post
  suspend fun post(call: ApplicationCall) {
    val body by call.body<RequestBody>()
    call.respond(HttpStatusCode.OK, "Hello user with ID ${body.id} and email ${body.email}!")
  }
}

suspend fun main(args: Array<String>) {
  val server = embeddedServer(Netty, port = 9090) {
    module {
      // We need this to be enabled. :(
      routing {}
      
      install(NoelKtorRouting) {
        loader(ReflectionBasedLoader("org.noelware.ktor.tests"))
        endpoints += MyEndpoint()
      }
    }
  }
}
```

## License
**ktor-routing** is released under the **MIT License** with love (ΰΉγ»Ο-)ο½β₯β by Noelware. π
