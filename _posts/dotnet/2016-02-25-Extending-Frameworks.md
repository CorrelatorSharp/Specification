---
layout: page
title: "Adding frameworks support"
category: dotnet
order: 2
---

### Adding support for other framework

Getting the current activity scope:

```csharp
ActivityScope.Current.Id
ActivityScope.Current.Name
ActivityScope.Current.ParentId
```

Getting the name of the http correlation header:

```csharp
CorrelatorSharp.Headers.CorrelationId
```

Creating a new scope:

```csharp
using CorrelatorSharp;

using (ActivityScope scope = new ActivityScope("Main Operation")) {
    DoWork();
}

```