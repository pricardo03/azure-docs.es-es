---
title: ¿Qué le ha ocurrido a mi proyecto de servicio en la nube? | Microsoft Docs
description: Describe lo que sucede en un proyecto de servicios en la nube después de conectarse a una cuenta de almacenamiento de Azure mediante los servicios conectados de Visual Studio
services: storage
author: ghogen
manager: jillfra
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: d96e523b6e4e060eaef33de0f961e7020e5dfb57
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69510637"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>¿Qué le ha ocurrido a mi proyecto de servicios en la nube (servicio conectado a Azure Storage de Visual Studio)?
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
Se han creado elementos con la cadena y la clave de conexión de la cuenta de almacenamiento seleccionada. Se han hecho modificaciones en los archivos siguientes:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

