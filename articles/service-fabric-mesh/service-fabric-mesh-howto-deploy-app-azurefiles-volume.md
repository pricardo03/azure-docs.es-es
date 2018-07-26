---
title: Almacenar el estado montando el volumen basado en Azure Files dentro del contenedor en la aplicación Service Fabric Mesh | Microsoft Docs
description: Aprenda a almacenar el estado montando el volumen basado en Azure Files dentro del contenedor en la aplicación Service Fabric Mesh mediante la CLI de Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: 94a4e17e6285893520a2f6482b32a69b1229e2fa
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090639"
---
# <a name="store-state-by-mounting-azure-files-based-volume-in-service-fabric-mesh-application"></a>Almacenar el estado montando el volumen basado en Azure Files en una aplicación Service Fabric Mesh

En este artículo se muestra cómo almacenar el estado en Azure Files mediante el montaje de un volumen dentro del contenedor de una aplicación de Service Fabric Mesh. En este ejemplo, la aplicación de contador tiene un servicio de ASP.NET Core con una página web que muestra el valor del contador en un explorador. 

El valor `counterService` lee regularmente un valor de contador de un archivo, lo incrementa y lo reescribe en el archivo. El archivo se almacena en una carpeta que está montada en el volumen del cual el recurso compartido de Azure Files hizo una copia de seguridad. 

## <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh 
Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta tarea. Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure y configure la suscripción.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-file-share"></a>Creación de un recurso compartido de archivos 
Cree un recurso compartido de archivos de Azure; para ello, siga estas [instrucciones](/azure/storage/files/storage-how-to-create-file-share). En las instrucciones siguientes el nombre de la cuenta de almacenamiento, la clave de la cuenta de almacenamiento y el nombre del recurso compartido de archivos se denominan `<storageAccountName>`, `<storageAccountKey>` y `<fileShareName>` respectivamente.

## <a name="create-resource-group"></a>Creación de un grupo de recursos
Cree un grupo de recursos en el que implementar la aplicación. Puede usar un grupo de recursos existente y omitir este paso. 

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

Cree la aplicación y los recursos relacionados mediante el siguiente comando, y proporcione los valores correspondientes a `storageAccountName`, `storageAccountKey` y `fileShareName` del paso anterior.

El parámetro `storageAccountKey` de la plantilla es un valor `securestring`. No se mostrará en el estado de implementación y los comandos `az mesh service show`. Asegúrese de que está especificado correctamente en el siguiente comando.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

El comando anterior implementa una aplicación Linux con la [plantilla mesh_rp.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.linux.json). Si quiere implementar una aplicación Windows, use la [plantilla mesh_rp.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/mesh_rp.windows.json). Las imágenes de contenedor de Windows son más grandes que las imágenes de contenedor de Linux y pueden tardar más en implementarse.

Pasados unos minutos, el comando debe devolver lo siguiente:

`counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abrir la aplicación
Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala en un navegador. Muestra una página web con el valor del contador que se actualiza cada segundo.

El comando de implementación devuelve la dirección IP pública del punto de conexión de servicio. Si lo desea, también puede consultar el recurso de red para buscar la dirección IP pública del punto de conexión de servicio. 
 
El nombre de los recursos de red de esta aplicación es `counterAppNetwork`; puede obtener información sobre él mediante el siguiente comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Comprobar que la aplicación es capaz de usar el volumen
La aplicación crea un archivo denominado `counter.txt` en el recurso compartido del archivo dentro de la carpeta `counter/counterService`. El contenido de este archivo es el valor del contador que se muestra en la página web.

Puede descargar el archivo con cualquier herramienta que le permita navegar por el recurso compartido de los archivos de Azure Files. Por ejemplo, puede usar el [Explorador de Microsoft Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Eliminación de los recursos

Para conservar los recursos limitados que se asignaron al programa de vista previa, le recomendamos que elimine los recursos con frecuencia. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Pasos siguientes

- Compruebe la aplicación de ejemplo del volumen de Azure Files en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).
