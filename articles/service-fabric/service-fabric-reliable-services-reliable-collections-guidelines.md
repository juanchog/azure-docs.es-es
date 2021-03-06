---
title: Directrices y recomendaciones de Reliable Collections en Azure Service Fabric | Microsoft Docs
description: Directrices y recomendaciones para el uso de Reliable Collections de Service Fabric
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak,zhol
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 12/10/2017
ms.author: mcoskun
ms.openlocfilehash: b0eb6a5aab5e71c0a8ac8263e177c493a07dafc0
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Directrices y recomendaciones de Reliable Collections en Azure Service Fabric
Esta sección proporciona directrices para el uso de Reliable State Manager y Reliable Collections. El objetivo es ayudar a los usuarios a evitar problemas comunes.

Las instrucciones se organizan como recomendaciones sencillas precedidas por los términos *Haga*, *Considere*, *Evite* y *No haga*.

* No modifique un objeto de tipo personalizado que hayan devuelto las operaciones de lectura (por ejemplo, `TryPeekAsync` o `TryGetValueAsync`). Las colecciones fiables, igual que las colecciones simultáneas, devuelven una referencia a los objetos y no una copia.
* No haga una copia en profundidad del objeto devuelto de tipo personalizado antes de modificarlo. Como los tipos de estructuras e integrados son parámetros que se pasan a través de un valor, no es necesario realizar una copia en profundidad de ellos, a menos que contengan propiedades o campos tipo referencias que se pretendan modificar.
* No use `TimeSpan.MaxValue` para tiempos de espera. Los tiempos de espera se deben utilizar para detectar interbloqueos.
* No utilice una transacción una vez que se haya confirmado, anulado o eliminado.
* No utilice una enumeración fuera del ámbito de transacción en que se creó.
* No cree una transacción dentro de la instrucción `using` de otra transacción, ya que puede provocar interbloqueos.
* Asegúrese de que la implementación de `IComparable<TKey>` es correcta. El sistema asume la dependencia de `IComparable<TKey>` para combinar los puntos de control y las filas.
* Utilice el bloqueo de actualización al leer un elemento con la intención de actualizarlo para evitar que se produzca una clase determinada de interbloqueos.
* Considere la posibilidad de mantener el número de colecciones confiables por partición inferior a 1000. Priorice las colecciones confiables con más elementos ante las colecciones confiables con menos.
* Plantéese la posibilidad de mantener sus elementos (por ejemplo, TKey y TValue para el diccionario confiable) por debajo de los 80 kB: cuanto más pequeños, mejor. De este modo, reducirá el grado de uso del montón de objetos grandes, así como los requisitos de E/S de red y de disco. A menudo, disminuye la replicación de datos duplicados cuando solo se actualiza una pequeña parte del valor. La forma habitual de conseguir esto en el diccionario confiable pasa por dividir las filas en varias de ellas.
* Considere la posibilidad de utilizar la funcionalidad de copia de seguridad y restauración para la recuperación ante desastres.
* Evite mezclar operaciones de entidad única y de varias entidades (por ejemplo, `GetCountAsync` y `CreateEnumerableAsync`) en la misma transacción debido a los diferentes niveles de aislamiento.
* Controle la excepción InvalidOperationException. El sistema puede anular las transacciones de usuario pueden por diversos motivos. Por ejemplo, cuando Reliable State Manager cambia su rol de Principal o cuando una transacción de larga ejecución está bloqueando el truncamiento del registro transaccional. En tales casos, el usuario puede recibir la excepción InvalidOperationException, que indica que ya ha finalizado su transacción. Suponiendo que la finalización de la transacción no la ha solicitado el usuario, la mejor manera de controlar esta excepción es eliminando la transacción, comprobar si se ha señalado el token de cancelación (o si se ha cambiado el rol de la réplica) y, si no, crear una nueva transacción y volver a intentarlo.  

Algunos aspectos que debe tener en cuenta:

* El tiempo de espera predeterminado es de 4 segundos para todas las API de Reliable Collection. La mayoría de los usuarios debe utilizar el tiempo de espera predeterminado.
* El token de cancelación predeterminado es `CancellationToken.None` en todas las API de colecciones confiables.
* El parámetro de tipo de clave (*TKey`Equals()`) de un diccionario confiable debe implementar correctamente* y `GetHashCode()`. Las claves deben ser inmutables.
* Para lograr una alta disponibilidad para las colecciones confiables, cada servicio debe tener al menos un destino y un tamaño de conjunto de réplicas mínimo de 3.
* Las operaciones de lectura de la base de datos secundaria pueden leer versiones que no están confirmadas en el cuórum.
  Esto significa que una versión de datos leída desde una única base de datos secundaria podría progresar como false.
  Las lecturas de la base de datos principal siempre son estables, es decir, que nunca pueden progresar como false.

### <a name="next-steps"></a>Pasos siguientes
* [Trabajo con Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Transacciones y bloqueos](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Administración de datos
  * [Copia de seguridad y restauración](service-fabric-reliable-services-backup-restore.md)
  * [Notifications](service-fabric-reliable-services-notifications.md)
  * [Serialización y actualización](service-fabric-application-upgrade-data-serialization.md)
  * [Configuración del administrador de estado confiable](service-fabric-reliable-services-configuration.md)
* Otros
  * [Introducción a Reliable Services de Service Fabric de Microsoft Azure](service-fabric-reliable-services-quick-start.md)
  * [Referencia para desarrolladores de colecciones confiables](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
