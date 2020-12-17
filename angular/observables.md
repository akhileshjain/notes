# Observables
---

![Observable](/images/observable.png "Observables")

The Observer has three ways of handling data from an Observable which are shown above.
- Handle the data
- Handle the error
- Handle the completion of Observable.

An observable is like a continuous stream of data. If there is an error, no further emitting of values (not even completion runs).
If no error, observable keeps sending value. Hence, we need to unsubscribe the observable, otherwise memory leaks will be there.


## Difference between a Promise and an Observable
---
**Promise**

A Promise handles a **single event** when an async operation completes or fails.

Note: There are `Promise` libraries out there that support cancellation, but `ES6 Promise` doesn't so far.

**Observable**

An `Observable` is like a `Stream` (in many languages) and allows to pass zero or more events where the callback is called for each event.

Often `Observable` is preferred over `Promise` because it provides the features of `Promise` and more. With `Observable` it doesn't matter if you want to handle 0, 1, or multiple events. You can utilize the same API in each case.

`Observable` also has the advantage over `Promise` to be cancellable. If the result of an HTTP request to a server or some other expensive async operation isn't needed anymore, the `Subscription` of an Observable allows to cancel the subscription, while a `Promise` will eventually call the success or failed callback even when you don't need the notification or the result it provides anymore.

While a `Promise` starts immediately, an `Observable` only starts if you subscribe to it. This is why Observables are called lazy.

Observable provides operators like `map, forEach, reduce, ...` similar to an array

There are also powerful operators like `retry()`, or `replay()` , ... that are often quite handy. A list of operators shipped with rxjs

Lazy execution allows to build up a chain of operators before the observable is executed by subscribing, to do a more declarative kind of programming.

## Operators 

![Operators](/images/operators.png "Operators")

For more on operators, keep practicing.

## Difference between an Observable and Subject
---

In stream programming there are two main interfaces: Observable and Observer.

**Observable** is for the consumer, it can be transformed and subscribed:

`observable.map(x => ...).filter(x => ...).subscribe(x => ...)`

**Observer** is the interface which is used to feed an observable source:

`observer.next(newItem)`

We can create new **Observable** with an **Observer**:

    var observable = Observable.create(observer => { 
        observer.next('first'); 
        observer.next('second'); 
        ... 
    });
    observable.map(x => ...).filter(x => ...).subscribe(x => ...)

Or, we can use a **Subject** which implements both the **Observable** and the **Observer** interfaces:

    var source = new Subject();
    source.map(x => ...).filter(x => ...).subscribe(x => ...)
    source.next('first')
    source.next('second')

![Subject](/images/subject.png "Subject")

`Observables` are **passive**, in the sense that they are triggered by something like a DOM event or an HTTP request (internally using the `next()`). However, if we want to actively want a value from the `Observable`, we need a `Subject` (a special kind of `Observable`) which can be triggered on our own manually from outside using the `next()`.