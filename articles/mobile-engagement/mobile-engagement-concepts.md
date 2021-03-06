---
title: Conceptos de Mobile Engagement | Microsoft Docs
description: Conceptos de Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 8d19abd1-0a6c-4772-9fa5-5e99980ac5da
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 1bc09de37c1b2aca35ef1ea74227df770f15baf5
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2018
---
# <a name="azure-mobile-engagement-concepts"></a>Conceptos de Azure Mobile Engagement
> [!IMPORTANT]
> Azure Mobile Engagement se retira el 31 de marzo de 2018. Esta página se eliminará poco después.
> 

Mobile Engagement define algunos conceptos comunes para todas las plataformas compatibles. En este artículo se describen brevemente dichos conceptos.

Este artículo es un buen comienzo si no está familiarizado con Mobile Engagement. Además, no olvide leer la documentación específica de la plataforma que usa ya que en ella se definirán más detalladamente y con ejemplos los conceptos descritos en este artículo, así como las posibles limitaciones.

## <a name="devices-and-users"></a>Dispositivos y usuarios
Mobile Engagement identifica a los usuarios mediante la generación de un identificador único para cada dispositivo, Este identificador se denomina identificador de dispositivo (o `deviceid`). Se genera de tal forma que todas las aplicaciones que se ejecutan en el mismo dispositivo comparten el mismo identificador de este tipo.

Esto significa, implícitamente, que Mobile Engagement considera que un dispositivo pertenece exactamente a un usuario y, por lo tanto, usuarios y dispositivos son conceptos equivalentes.

## <a name="sessions-and-activities"></a>Sesiones y actividades
Una sesión consiste en el uso individual de la aplicación por parte de un usuario, desde el momento en que comienza a usarla hasta que se detiene.

Una actividad es un uso de una subparte determinada de la aplicación por un usuario (suele ser una pantalla, pero puede ser cualquier elemento adecuado para la aplicación).

Un usuario solo puede realizar una actividad a la vez.

Una actividad se identifica mediante un nombre (limitado a 64 caracteres) y, opcionalmente, puede insertar algunos datos adicionales (con un límite de 1024 bytes).

Las sesiones se calculan automáticamente a partir de la secuencia de actividades realizadas por los usuarios. Una sesión comienza cuando el usuario inicia su primera actividad y se detiene cuando acaba su última actividad. Esto significa que no es necesario iniciar o detener las sesiones explícitamente. En cambio, las actividades se inician y detienen explícitamente. Si no se notifica ninguna actividad, tampoco se notifica ninguna sesión.

## <a name="events"></a>Eventos
Los eventos se usan para notificar acciones instantáneas (como un botón que se presiona o la lectura de artículos por parte de los usuarios).

Un evento puede estar relacionado con la sesión actual o con un trabajo en ejecución, o bien ser independiente.

Se identifica mediante un nombre (limitado a 64 caracteres) y, opcionalmente, puede insertar algunos datos adicionales (con un límite de 1024 bytes).

## <a name="error"></a>Error
Los errores se usan para notificar problemas detectados correctamente por la aplicación (como acciones de usuario incorrectas o errores de llamada a la API).

Un error puede estar relacionado con la sesión actual o con un trabajo en ejecución, o bien ser independiente.

Se identifica mediante un nombre (limitado a 64 caracteres) y, opcionalmente, puede insertar algunos datos adicionales (con un límite de 1024 bytes).

## <a name="job"></a>Trabajo
Los trabajos se usan para notificar acciones que tienen una duración (como la duración de las llamadas API, el tiempo que se muestran los anuncios, la duración de las tareas en segundo plano o de las acciones del usuario).

Un trabajo no se relaciona con una sesión, porque una tarea se puede ejecutar en segundo plano sin interacción del usuario.

Se identifica mediante un nombre (limitado a 64 caracteres) y, opcionalmente, puede insertar algunos datos adicionales (con un límite de 1024 bytes).

## <a name="crash"></a>Bloqueo
El SDK de Mobile Engagement emite bloqueos automáticamente para notificar errores de la aplicación (es decir, problemas no detectados por la aplicación que hacen que esta se bloquee).

## <a name="application-information"></a>Información de la aplicación
La información de la aplicación se usa para etiquetar a los usuarios, es decir, para asociar determinados datos a los usuarios de una aplicación (de forma similar a las cookies web, con la excepción de que la información de la aplicación se almacena en el lado del servidor en la plataforma Azure Mobile Engagement).

Para registrar información de la aplicación se puede usar la API del SDK de Mobile Engagement o la API de dispositivos de la plataforma Mobile Engagement.

Este tipo de información es un par clave-valor asociado a un dispositivo. La clave es el nombre de la información de la aplicación(limitado a 64 letras [a-zA-Z], números [0-9] y caracteres de subrayado [_] ASCII). El valor (limitado a 1024 caracteres) puede ser cualquier cadena, entero, fecha (aaaa-MM-dd) o valor booleano (true o false).

Se puede asociar un número cualquiera de informaciones de la aplicación a un dispositivo dentro de los límites definidos por los términos de precios de Mobile Engagement. Para una clave determinada, Mobile Engagement solo realiza un seguimiento del conjunto de valores más reciente (sin historial). Al establecer o cambiar el valor de una información de aplicación, se fuerza a Mobile Engagement a volver a evaluar los criterios de audiencia establecidos en esta (si los hay), lo que significa que esa información de la aplicación puede usarse para desencadenar inserciones en tiempo real.

## <a name="extra-data"></a>Datos adicionales
Los datos adicionales (o extras) son datos arbitrarios que pueden asociarse a los eventos, errores, actividades y trabajos.

Su estructura es similar a la de los objetos JSON: se componen de un árbol de pares clave-valor. Las claves tienen un límite de 64 letras [a-zA-Z], números [0-9] y caracteres de subrayado [_] ASCII y el tamaño total de los extras se limita a 1024 caracteres (una vez codificados en JSON por el SDK de Mobile Engagement).

El árbol completo de pares clave-valor se almacena como objeto JSON. Sin embargo, solo se descompone el primer nivel de claves-valores para que puedan tener acceso directamente determinadas funciones avanzadas, como Segmentos (por ejemplo, puede definir sin problemas un segmento “fans de ciencia ficción” compuesto por todos los usuarios que durante el último mes enviaron al menos 10 veces el evento denominado “contenido_visualizado” con la clave extra “tipo_contenido” establecida en el valor “ciencia ficción”). Por tanto, se recomienda enviar solamente extras formados por listas sencillas de pares clave-valor con valores escalares (por ejemplo, cadenas, fechas, enteros o valores booleanos).

## <a name="next-steps"></a>Pasos siguientes
* [Introducción al SDK de Windows Universal para Azure Mobile Engagement](mobile-engagement-windows-store-sdk-overview.md)
* [Introducción al SDK de Windows Phone Silverlight para Azure Mobile Engagement](mobile-engagement-windows-phone-sdk-overview.md)
* [SDK de iOS para Azure Mobile Engagement](mobile-engagement-ios-sdk-overview.md)
* [SDK de Android para Azure Mobile Engagement](mobile-engagement-android-sdk-overview.md)

