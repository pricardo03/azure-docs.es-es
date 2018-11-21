---
title: Almacenamiento del estado en una aplicación Azure Service Fabric Mesh mediante el montaje de un volumen basado en Azure Files dentro del contenedor | Microsoft Docs
description: Aprenda a almacenar el estado en una aplicación Azure Service Fabric Mesh montando un volumen basado en Azure Files en el contenedor mediante la CLI de Azure.
services: service-fabric-mesh
documentationcenter: .net
author: rwike77
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: azure-cli
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: cb5b421c1bcfe888d65335f3ab7f67bed80eec34
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51614266"
---
# <a name="store-state-in-an-azure-service-fabric-mesh-application-by-mounting-an-azure-files-based-volume-inside-the-container"></a>Almacenamiento del estado en una aplicación Azure Service Fabric Mesh mediante el montaje de un volumen basado en Azure Files dentro del contenedor

En este artículo se muestra cómo almacenar el estado en Azure Files mediante el montaje de un volumen dentro del contenedor de una aplicación de Service Fabric Mesh. En este ejemplo, la aplicación Counter tiene un servicio de ASP.NET Core con una página web que muestra el valor del contador en un explorador. 

`counterService` lee regularmente un valor de contador de un archivo, lo incrementa y lo vuelve a escribir en el archivo. El archivo se almacena en una carpeta que está montada en el volumen del cual el recurso compartido de Azure Files hizo una copia de seguridad.

## <a name="prerequisites"></a>Requisitos previos

Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta tarea. Para usar la CLI de Azure con este artículo, asegúrese de que `az --version` devuelve al menos `azure-cli (2.0.43)`.  Instale (o actualice) el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure y configure la suscripción.

```azurecli-interactive
az login
az account set --subscription "<subscriptionID>"
```

## <a name="create-a-file-share"></a>Creación de un recurso compartido de archivos

Cree un recurso compartido de archivos de Azure; para ello, siga estas [instrucciones](/azure/storage/files/storage-how-to-create-file-share). En las instrucciones siguientes el nombre de la cuenta de almacenamiento, la clave de la cuenta de almacenamiento y el nombre del recurso compartido de archivos se denominan `<storageAccountName>`, `<storageAccountKey>` y `<fileShareName>` respectivamente. Estos valores están disponibles en Azure Portal:
* <storageAccountName> -En **Cuentas de almacenamiento**, es el nombre de la cuenta de almacenamiento que usó al crear el recurso compartido de archivos.
* <storageAccountKey> -Seleccione la cuenta de almacenamiento en **Cuentas de almacenamiento** y después seleccione **Claves de acceso** y utilice el valor de **key1**.
* <fileShareName> - Seleccione la cuenta de almacenamiento en **Cuentas de almacenamiento** y después seleccione **Archivos**. El nombre que se usa es el del recurso compartido de archivo que acaba de crear.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos en el que implementar la aplicación. El siguiente comando crea un grupo de recursos denominado `myResourceGroup` en una ubicación en la parte oriental de Estados Unidos.

```azurecli-interactive
az group create --name myResourceGroup --location eastus 
```

## <a name="deploy-the-template"></a>Implementación de la plantilla

Cree la aplicación y los recursos relacionados mediante el siguiente comando, y especifique los valores de `storageAccountName`, `storageAccountKey` y `fileShareName` del [Creación de un recurso compartido de archivos](#create-a-file-share) anterior.

El parámetro `storageAccountKey` de la plantilla es una cadena segura. No se mostrará en el estado de implementación y los comandos `az mesh service show`. Asegúrese de que está especificado correctamente en el siguiente comando.

El siguiente comando implementa una aplicación Linux mediante la [plantilla counter.azurefilesvolume.linux.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json). Para implementar una aplicación Windows, use la [plantilla counter.azurefilesvolume.windows.json](https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.windows.json). Tenga en cuenta que las imágenes de contenedor más grandes pueden tardar más en implementarse.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/counter/counter.azurefilesvolume.linux.json  --parameters "{\"location\": {\"value\": \"eastus\"}, \"fileShareName\": {\"value\": \"<fileShareName>\"}, \"storageAccountName\": {\"value\": \"<storageAccountName>\"}, \"storageAccountKey\": {\"value\": \"<storageAccountKey>\"}}"
```

Pasados unos minutos, el comando debe devolver `counterApp has been deployed successfully on counterAppNetwork with public ip address <IP Address>`

## <a name="open-the-application"></a>Abrir la aplicación

El comando de implementación devolverá la dirección IP pública del punto de conexión de servicio. Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala desde un navegador. Mostrará una página web con el valor del contador que se actualiza cada segundo.

El nombre del recurso de red de esta aplicación es `counterAppNetwork`. Para información acerca de la aplicación, como su descripción, ubicación, grupo de recursos, etc. use el comando siguiente:

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name counterAppNetwork
```

## <a name="verify-that-the-application-is-able-to-use-the-volume"></a>Comprobar que la aplicación es capaz de usar el volumen

La aplicación crea un archivo denominado `counter.txt` en el recurso compartido del archivo dentro de la carpeta `counter/counterService`. El contenido de este archivo es el valor del contador que se muestra en la página web.

El archivo se puede descargar con cualquier herramienta que permita navegar por un recurso compartido de los archivos de Azure Files como el [Explorador de Microsoft Azure Storage](https://azure.microsoft.com/features/storage-explorer/).

## <a name="delete-the-resources"></a>Eliminación de los recursos

Elimine periódicamente los recursos que no use en Azure. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron (con lo que se elimina todo el contenido asociado con él) con el siguiente comando:

```azurecli-interactive
az group delete --resource-group myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

- Compruebe la aplicación de ejemplo del volumen de Azure Files en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).