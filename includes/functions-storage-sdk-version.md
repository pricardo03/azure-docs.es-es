---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: cfowler
ms.service: azure-functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: c2fff707dcaafac69efcad3dbf33446a7b797396
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608372"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Versión de SDK de Azure Storage en Functions 1.x

En Functions 1.x, los desencadenadores y enlaces de Storage usan la versión 7.2.1 del SDK de Azure Storage (paquete NuGet [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)) Si hace referencia a una versión diferente del SDK de Storage y enlaza a un tipo de SDK de Storage en la signatura de función, el runtime de Functions puede notificar que no se puede enlazar con ese tipo. La solución es asegurarse de que el proyecto hace referencia a [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1).
