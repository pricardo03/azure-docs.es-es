---
title: ¿Qué le ha ocurrido a mi proyecto ASP.NET? | Microsoft Docs
description: Describe lo que sucede después de agregar Azure Storage a un proyecto ASP.NET con servicios conectados de Visual Studio.
services: storage
author: ghogen
manager: jillfra
ms.assetid: e1fe1b6d-4e3d-476d-8b2f-f7ade050515e
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 74db2e1e15c282f4393715c67561b2ed61bbcdfb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512069"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>¿Qué le ha ocurrido a mi proyecto ASP.NET (servicio conectado a Azure Storage de Visual Studio)?
## <a name="references-added"></a>Se han agregado referencias
El paquete NuGet de Azure Storage se agregó al proyecto de Visual Studio.  
Este paquete agrega las siguientes referencias. NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Se ha agregado la cadena de conexión para Azure Storage.
En el archivo web.config de su proyecto, se ha creado un elemento con la cadena y la clave de conexión de la cuenta de almacenamiento seleccionada.

Para obtener más información, consulte [ASP.NET](https://www.asp.net).

