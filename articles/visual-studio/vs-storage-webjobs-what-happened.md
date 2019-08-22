---
title: ¿Qué le ha ocurrido a mi proyecto WebJob (servicio conectado a Azure Storage de Visual Studio)? | Microsoft Docs
description: Describe lo que ha ocurrido en un proyecto WebJob de Azure después de conectarse a una cuenta de almacenamiento de Azure mediante los servicios conectados de Visual Studio
services: storage
author: ghogen
manager: jillfra
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: c54214e0b919ddaa60403a1c986a144100143577
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510477"
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

