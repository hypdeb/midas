# MIDAS
Write `C#` abstractions and get a `Protocol Buffer` model and `gRPC` implementations for free!
## Use Case
Define an interface for some feature:
```csharp
public sealed record RouteParameters(string Origin, string Destination);

public sealed record Route(List<string> Stops);

public interface RouteFinder
{
  Task<Route> FindBestRouteAsync(RouteParameters routeParameters);
}
```
There's two ways you wish to implement the above feature: a client such as a browser or an app will want to call an external service because finding this route can be computationally expensive, but the server receiving the requests from those clients will most likely do the calculations locally.

The remote implementation should be trivial using `gRPC` and `Protocol Buffers`. However, you still have to write a lot of boilerplate, starting with the `.proto`s themselves, conversions, the implementation of the service using those and the implementation of the server. I'm betting that it can all be generated.

### Other solution
Another path is to use the code generated from the `Protocol Buffers` directly, but these generated classes are usually not the greatest looking for application logic and awkward to use. They are however **extremely** efficient in serialization, and this is what we want to abuse here, without the downsides.