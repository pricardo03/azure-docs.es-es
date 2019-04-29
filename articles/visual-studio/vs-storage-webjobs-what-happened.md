---
title: ¿Qué le ha ocurrido a mi proyecto WebJob (servicio conectado a Azure Storage de Visual Studio)? | Microsoft Docs
description: Describe qué ha ocurrido en un proyecto de WebJob de Azure después de conectarse a una cuenta de almacenamiento mediante Visual Studio los servicios conectados
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: fa152d8b88254a35d00b91537bf1001ea1130e57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60722626"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>¿Qué le ha ocurrido a mi proyecto WebJob (servicio conectado a Azure Storage de Visual Studio)?
## <a name="references-added"></a>Se han agregado referencias
El paquete de NuGet de Azure Storage se agregó al proyecto de Visual Studio o se actualizó en él.  
Este paquete agrega las siguientes referencias. NET:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Se ha agregado la cadena de conexión para Azure Storage.
En el archivo App.config del proyecto, se actualizaron las entradas **AzureWebJobsStorage** y **AzureWebJobsDashboard** con la cadena y la clave de conexión de la cuenta de almacenamiento seleccionada.

Para obtener más información, consulte [Recursos de documentación de Azure WebJobs](https://go.microsoft.com/fwlink/?linkid=390226).

