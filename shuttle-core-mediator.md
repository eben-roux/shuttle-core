---
title: Shuttle.Core.Mediator
layout: api 
---
# Shuttle.Core.Mediator

```
PM> Install-Package Shuttle.Core.Mediator
```

The Shuttle.Core.Mediator package provides a [mediator pattern](https://en.wikipedia.org/wiki/Mediator_pattern) implementation.

## IMediator

The core interface is the `IMediator` interface and the default implementation provided is the `Mediator` class.

This interface provides a synchronous calling mechanism and all `IParticipant` implementations need to be thread-safe singleton implementations that are added to the mediator at startup.  Any operations that require transient mechanisms should be handled by the relevant participant.

```c#
IMediator Add(object participant);
```

The `Add` method registers the given participant.

```c#
void Send(object message, CancellationToken cancellationToken = default);
```

The `Send` method will find all participants that implements the `IParticipant<T>` with the type `T` of the message type that you are sending.  Participants that are marked with the `BeforeObserverAttribute` filter will be executed first followed by all participants with no filters attributes and then finally all participants marked with the `AfterObserverAttribute` filter will be called.

#### Extensions

```c#
Task SendAsync(this IMediator mediator, object message, CancellationToken cancellationToken = default)
```
Sends a message asynchronously.

```c#
public static T Send<T>(this IMediator mediator, T message, CancellationToken cancellationToken = default)
```

The same as `Send` except that it returns the given message.

```c#
public static void RegisterMediatorParticipants(this IComponentRegistry registry, string assemblyName)
public static void RegisterMediatorParticipants(this IComponentRegistry registry, Assembly assembly)
```

Registers all types that implement the `IParticipant<T>` interface against the open generic type `IParticipant<>`.

```c#
public static void AddMediatorParticipants(this IComponentResolver resolver)
```

Adds any participant instances registered against the open generic `IParticipant` to the registered `IMediator` instance.

## IParticipant

```c#
public interface IParticipant<in T>
{
    void ProcessMessage(IParticipantContext<T> context);
}
```

A participant would handle the message that is sent on using the mediator.

There are no *request/response* semantics and the design philosophy here is that the message encapsulates the state that is passed along in a *pipes & filters* approach.

There may be any number of participants that process the message. 

# Considerations

The mediator may be as broad or narrow as is applicable.  You could create a new mediator and add only the relevant participants to solve a particular use-case or you could have a central mediator registered in your dependency injection container of choice to provide decoupling across your system.

If you have a rather predictable sequential workflow and you require something faster then you may wish to consider the [Shuttle.Core.Pipelines] package.  A performance testing application for your use-case would be able to indicate the more suitable option.

[Shuttle.Core.Pipelines]: {{ "/shuttle-core-pipelines" | relative_url }}  