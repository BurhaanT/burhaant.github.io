---
layout: post
title: When to Use a Thread-Safe Collection
date: 2015-12-22 05:09
author: burhaan
comments: true
categories: [Uncategorized]
---
The .NET Framework 4 introduces five new collection types that are specially designed to support multi-threaded add and remove operations. To achieve thread-safety, these new types use various kinds of efficient locking and lock-free synchronization mechanisms. Synchronization adds overhead to an operation. The amount of overhead depends on the kind of synchronization that is used, the kind of operations that are performed, and other factors such as the number of threads that are trying to concurrently access the collection.In some scenarios, synchronization overhead is negligible and enables the multi-threaded type to perform significantly faster and scale far better than its non-thread-safe equivalent when protected by an external lock. In other scenarios, the overhead can cause the thread-safe type to perform and scale about the same or even more slowly than the externally-locked, non-thread-safe version of the type.
The following article provide general guidance about when to use a thread-safe collection versus its non-thread-safe equivalent

<a href='https://msdn.microsoft.com/en-us/library/dd997373(v=vs.110).aspx' target='_blank'>When to Use a Thread-Safe Collection</a>.
