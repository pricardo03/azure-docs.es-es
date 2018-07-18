---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: tdykstra
manager: cfowler
ms.service: functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 97c56b07833f7d93541bb0b3747889f5a50a8203
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34675282"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Versión de SDK de Azure Storage en Functions 1.x

En Functions 1.x, los desencadenadores y enlaces de Storage usan la versión 7.2.1 del SDK de Azure Storage (paquete NuGet [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)) Si hace referencia a una versión diferente del SDK de Storage y enlaza a un tipo de SDK de Storage en la signatura de función, el runtime de Functions puede notificar que no se puede enlazar con ese tipo. La solución es asegurarse de que el proyecto hace referencia a [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
