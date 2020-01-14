---
title: Instantánea en un momento dado de Azure App Configuration
description: Introducción sobre cómo funciona una instantánea en un momento dado en Azure App Configuration
services: azure-app-configuration
author: yegu-ms
ms.author: yegu
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.openlocfilehash: 4db52ce1897aa5a2b809cb7044b9764baffd0767
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495263"
---
# <a name="point-in-time-snapshot"></a>Instantánea en un momento dado

Azure App Configuration mantiene registros de las horas exactas en las que se crea un nuevo par clave-valor y en las que se modifica este posteriormente. Estos registros forman una escala de tiempo completa de los cambios de pares clave-valor. Los almacenes de App Configuration pueden reconstruir el historial del valor de cualquier clave y reproducir su valor pasado en cualquier momento del pasado hasta el presente. Con esta característica puede "viajar en el tiempo" en el pasado y recuperar un antiguo par clave-valor. Por ejemplo, puede obtener los valores de configuración de ayer, justo antes de la implementación más reciente para recuperar una configuración anterior en caso de que deba revertir la aplicación.

## <a name="key-value-retrieval"></a>Recuperación de pares clave-valor

Para recuperar pares clave-valor, especifique una hora a la que se realice una instantánea de los pares clave-valor en el encabezado de HTTP de una llamada a la API REST. Por ejemplo:

```rest
GET /kv HTTP/1.1
Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT
```

Actualmente, App Configuration conserva siete días del historial de cambios.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cree una aplicación web ASP.NET Core](./quickstart-aspnet-core-app.md)  
