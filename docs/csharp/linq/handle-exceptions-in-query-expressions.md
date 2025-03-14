---
title: Handle exceptions in query expressions (LINQ in C#)
description: Learn how to handle exceptions in LINQ query expressions in C#.
ms.date: 12/01/2016
ms.assetid: 2bf0c397-13fb-4f68-bc2b-531c6c88a167
---
# Handle exceptions in query expressions

It's possible to call any method in the context of a query expression. However, we recommend that you avoid calling any method in a query expression that can create a side effect such as modifying the contents of the data source or throwing an exception. This example shows how to avoid raising exceptions when you call methods in a query expression without violating the general .NET guidelines on exception handling. Those guidelines state that it's acceptable to catch a specific exception when you understand why it's thrown in a given context. For more information, see [Best Practices for Exceptions](../../standard/exceptions/best-practices-for-exceptions.md).

The final example shows how to handle those cases when you must throw an exception during execution of a query.

## Example 1

The following example shows how to move exception handling code outside a query expression. This is only possible when the method does not depend on any variables local to the query. It is easier to deal with exceptions outside of the query expression.

:::code language="csharp" source="../../../samples/snippets/csharp/concepts/linq/LinqSamples/Exceptions.cs" id="exceptions_1":::

Note that in the `catch (InvalidOperationException)` in the example above, handle (or don't handle) the exception in the way that is appropriate for your application.

## Example 2

In some cases, the best response to an exception that is thrown from within a query might be to stop the query execution immediately. The following example shows how to handle exceptions that might be thrown from inside a query body. Assume that `SomeMethodThatMightThrow` can potentially cause an exception that requires the query execution to stop.

Note that the `try` block encloses the `foreach` loop, and not the query itself. This is because the `foreach` loop is the point at which the query is actually executed. For more information, see [Introduction to LINQ queries](get-started/introduction-to-linq-queries.md).
The runtime exception will only be thrown when the query is executed.
Therefore they must be handled in the `foreach` loop.

:::code language="csharp" source="../../../samples/snippets/csharp/concepts/linq/LinqSamples/Exceptions.cs" id="exceptions_2":::

Remember to catch whatever exception you expect to raise and/or do any necessary cleanup in a finally block.

## See also

- [Language Integrated Query (LINQ)](index.md)
