# Async Streams
### a.k.a. Asynchronous Enumerables

`Concurrency in C# Cookbook` - Stephen Cleary's book

## Why Async Streams?

### Why Asynchrony?

| Clients | Server |
|---|---|
Responsiveness | Scalability
| UI thread stays free | Minimize threads used to serve requests |
| Better UX | Faster response to bursting traffic |
|Required by many app stores||

### Async Streams

* Enumerables
* Tasks
* Observables

Tasks only produce a result once. Enumerables allow you to generate multiple values. We want to generate multiple results asynchronously. 

Observables are asynchronous and multi-valued. We want a more natural consumption (foreach or async subscriptions which enumerables and tasks already have).

### How they're used

Desired Access | Return Type |
|---|---|
| Single value, synchronous | T |
| Multiple values, synchronous | IEnumerable<T> |
| Single value, asynchronous | Task<T> |
| Multuple values, asynchronous | IAsyncEnumerable<T> |

## Demo Code

``` csharp
//Synchronous yield return
static void YieldReturnMain() {
    foreach (int item in YieldReturn()) {
        Console.WriteLine(item);
    }

    //same as
    using (IEnumerable<int> enumerator = YieldReturn().GetEnumerator()) {
        while (enumerator.MoveNext()) {
            int item = enumerator.Current;
            Console.WriteLine(item);
        }
    }
}

static IEnumerable<int> YieldReturn() {
    yield return 1;
    yield return 2;
    yield return 3;
}
```

``` csharp
// async enumerables
static async Task AwaitAndYieldReturnMain(){
    await foreach (int item in AwaitAndYieldReturn())
    {
        Console.WriteLine(item);
    }

    //same as
    await using (IAsyncEnumerator<int> enumerator = AwaitAndYieldReturn().GetAsyncEnumerator()){
        while (true) {}//missing code, but it's what the compiler turns it the previous block into
    }
}

static IAsyncEnumerable AwaitAndYieldReturn() {
    await Task.Delay();
    yield return 1;
    await Task.Delay();
    yield return 2;
}
```

## Async Streams in More Detail

* Enumerators/Generators/Iterables
   * Deferred execution, generated on demand
   * Pull-based sequence, like foreach
* Async streams
   * Still deferred execution (and pull-based)
   * Get Next Item is asynchronous (MoveNext, MoveNextAsync)
   * Allows asynchronous enumerators/generators/iterables
   * Requires asynchronous consumers

### ValueTask

*It's a more efficient Task<T>*, particularly if the result is commonly synchronous.

Usage restriction: only consume once.

Other properties may be behave differently than Task<T>
* Result is invalid until the value task has completed.
* Same for GetAwaiter().GetResult()

**Never use .Result**

## Creating Asynchronous Streams

Return type is `IAsyncEnumerable<T>`, use `await` and `yield`

For consuming in C#, use `await foreach` in an async method.

JS uses `foreach await` in an async function.

Exceptions propogate naturally, can be caught with `try/catch`

## Use Case: Paging API

You can make multiple calls to an API which has paging in a loop to get a full list of results you want, and return them as an IAsyncEnumerable so that they can be processed whenever the server returns the data.

## Anti-Use Case: Notification API

e.g. SingalR

Anything with a subscribe, multiple updates, and unsubscribe. This is a better fit for observables.

Async streams are pull based, you need to introduce an extra layer in order to get the subscription/reactionary functionality.

## Linq to Async Streams

NuGet: `System.Linq.Async`, community project, not Microsoft.

### Passing Async Lambdas to Linq

Linq-to-Streams has overloads for async lambdas:
* WhereAwait, etc.
* Await suffix, since they await their delegates

Return IAsyncEnumerable<T> so they chain naturally.

``` csharp
IAsyncEnumerable<int> query = SlowRange().WhereAwait(async x => {
    await Task.Delay(TimeSpan.FromSeconds(0.1));
    return x % 2 == 0
});

await foreach (int item in query) {
    Console.WriteLine(item);
}
```

"Terminal" operators end in Async, since they return awaitables. Example: CountAsync().

`int result = await SlowRange().CountASync(x => x % 2 == 0);`

Terminal operators also have overloads for async lambdas: CountAwaitAsync().

``` csharp
int result = await SlowRange().CountAwaitAsync(async x => {
    return x % 2 == 0;
});
```

All operators act on data values one at a time
* No async concurrency built in

### Supercharging Regular Linq

When you have an ordinary linq expression and you want to use an async lambda
* Use ToAsyncEnumerable (then you can use WhereAwait, etc.)

``` csharp
IEnumerable<int> query = Enumerable.Range(0, 10);

IAsyncEnumerable<int> asyncQuery = query.ToAsyncEnumerable().WhereAwait(async x => {
    return x % 2 == 0;
});
```

## Cancelling Async Streams

Takes a `CancellationToken`, apply the EnumeratorCancellation attribute in method argument list.

``` csharp
async IAsyncEnumerable<T> SomeMethod([EnumeratorCancellation] CancellationToken token)
```

* The simple way
   * Pass CancellationToken to the method returning an enumerable
* The complex way
   * Use .WithCancellation() when enumerating.
   * Because enumerators are cancellable, not enumerables

``` csharp
await foreach (int item in items.WithCancellation(token)){
    ...
}
```

## Custom/Future Operators

Semantic considerations with async streams
* Concurrency: serial, concurrent, MaxDegree, single-buffer?
   * when to call GetNextAsync?
* Ordering: original, as-completed?

Async Linq is always serial with original ordering.

