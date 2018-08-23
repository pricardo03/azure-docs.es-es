---
title: ¿Qué le ha ocurrido a mi proyecto de servicio en la nube? | Microsoft Docs
description: Describe lo que sucede en un proyecto de servicios en la nube después de conectarse a una cuenta de almacenamiento de Azure mediante los servicios conectados de Visual Studio
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e823bc16c500dfee44312a774d5e3bd6622e5fae
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "42142721"
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

