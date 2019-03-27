---
title: Instantánea en un momento dado de Azure App Configuration | Microsoft Docs
description: Introducción sobre cómo funciona una instantánea en un momento dado en Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: na
ms.topic: overview
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: e833146d05f0c35449915c1d1293873258a7b7eb
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226783"
---
# <a name="point-in-time-snapshot"></a>Instantánea en un momento dado

Azure App Configuration mantiene registros de las horas exactas en las que se crea un nuevo par clave-valor y en las que se modifica este posteriormente. Estos registros forman una escala de tiempo completa de los cambios de pares clave-valor. Un almacén de configuración de aplicaciones puede reconstruir el historial de cualquier par clave-valor y reproducir sus valores en cualquier momento del pasado hasta el presente. Con esta característica puede "viajar en el tiempo" en el pasado y recuperar un antiguo par clave-valor. Por ejemplo, puede obtener los valores de configuración de ayer, justo antes de la implementación más reciente para recuperar una configuración anterior en caso de que deba revertir la aplicación.

## <a name="key-value-retrieval"></a>Recuperación de pares clave-valor

Para recuperar pares clave-valor, especifique una hora a la que se realice una instantánea de los pares clave-valor en el encabezado de HTTP de una llamada a la API REST. Por ejemplo: 

        GET /revisions HTTP/1.1
        Accept-Datetime: Sat, 1 Jan 2019 02:10:00 GMT

Actualmente, App Configuration conserva siete días del historial de cambios.

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación de una aplicación web de ASP.NET](quickstart-aspnet-core-app.md)  
