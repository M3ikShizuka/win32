---
Description: 'Destructor method.'
ms.assetid: '678085c2-5999-4e62-8749-99b783787cc6'
title: 'CSourceStream.~CSourceStream destructor'
---

# CSourceStream.~CSourceStream destructor

Destructor method.

## Syntax


```C++
~CSourceStream();
```



## Remarks

The destructor automatically removes the pin from the owning filter, by calling [**CSource::RemovePin**](csource-removepin.md).

## Requirements



|                    |                                                                                                                                                                                            |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Header<br/>  | <dl> <dt>Source.h (include Streams.h)</dt> </dl>                                                                                    |
| Library<br/> | <dl> <dt>Strmbase.lib (retail builds); </dt> <dt>Strmbasd.lib (debug builds)</dt> </dl> |



## See also

<dl> <dt>

[**CSourceStream Class**](csourcestream.md)
</dt> </dl>

�

�



