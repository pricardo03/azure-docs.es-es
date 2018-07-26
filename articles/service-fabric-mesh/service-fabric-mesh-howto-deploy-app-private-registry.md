---
title: Implementar una aplicación desde un registro privado en Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información acerca de cómo implementar una aplicación que usa un registro de contenedor privado en Service Fabric Mesh mediante la CLI de Azure.
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
ms.date: 07/16/2018
ms.author: ryanwi
ms.custom: mvc, devcenter
ms.openlocfilehash: af92d3c6ea881d00ec687a5560bf4db35aa431c5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39089493"
---
# <a name="deploy-a-service-fabric-mesh-app-from-a-private-container-image-registry"></a>Implementar una aplicación de Service Fabric Mesh desde un registro de imágenes de contenedor privado

En este artículo se muestra cómo implementar una aplicación de Azure Service Fabric Mesh que utiliza un registro de imágenes de contenedor privado.

## <a name="prerequisites"></a>Requisitos previos

### <a name="set-up-service-fabric-mesh-cli"></a>Configuración de la CLI de Service Fabric Mesh 
Puede usar Azure Cloud Shell o una instalación local de la CLI de Azure para completar esta tarea. Instale el módulo de extensión de la CLI de Azure Service Fabric Mesh siguiendo estas [instrucciones](service-fabric-mesh-howto-setup-cli.md).

### <a name="install-docker"></a>Instalación de Docker

Instale Docker para admitir las aplicaciones de Service Fabric en contenedores que usa Service Fabric Mesh.

### <a name="windows-10"></a>Windows 10

Descargue e instale la versión más reciente de [Docker Community Edition para Windows][download-docker]. 

Durante la instalación, seleccione **Use Windows containers instead of Linux containers** (Usar contenedores de Windows en lugar de contenedores de Linux) cuando se le pida. A continuación, cierre la sesión e iníciela de nuevo. Después de iniciar sesión de nuevo, si no habilitó anteriormente Hyper-V, puede que se le pida hacerlo ahora. Debe habilitar Hyper-V y, a continuación, reiniciar el equipo.

Una vez reiniciado el equipo, Docker le solicitará que habilite la característica **Containers**. Hágalo y, a continuación, reinicie el equipo.

### <a name="windows-server-2016"></a>Windows Server 2016

Utilice los siguientes comandos de PowerShell para instalar Docker. Para obtener más información, consulte [Docker Enterprise Edition para Windows Server][download-docker-server].

```powershell
Install-Module DockerMsftProvider -Force
Install-Package Docker -ProviderName DockerMsftProvider -Force
Install-WindowsFeature Containers
```

Reinicie el equipo.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Iniciar sesión en Azure y establecer la suscripción de Azure:

```azurecli-interactive
az login
az account set --subscription "<subscriptionName>"
```

## <a name="create-a-container-registry-and-push-an-image-to-it"></a>Crear un registro de contenedor y agregar una imagen

Cree una instancia de Azure Container Registry siguiendo las instrucciones de [Create a private Docker registry in Azure with the Azure CLI](../container-registry/container-registry-get-started-azure-cli.md) (Crear un registro privado de Docker en Azure con la CLI de Azure). Realice los siguientes pasos hasta llegar al paso para [iniciar sesión en ACR](../container-registry/container-registry-get-started-azure-cli.md#log-in-to-acr). 

### <a name="push-image-to-acr"></a>Inserción de una imagen en ACR

Para insertar una imagen en Azure Container Registry, primero debe tener una imagen. Si aún no tiene ninguna imagen de contenedor local, ejecute el siguiente comando para extraer una imagen existente de Docker Hub.

```bash
docker pull seabreeze/azure-mesh-helloworld:1.1-alpine
```

Para poder insertar una imagen en el registro, debe etiquetarla con el nombre completo del servidor de inicio de sesión de ACR. Ejecute el siguiente comando para obtener el nombre completo del servidor inicio de sesión de la instancia de ACR.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Etiquete la imagen mediante el comando [docker tag][docker-tag]. Reemplace `<acrLoginServer>` por el nombre del servidor de inicio de sesión de la instancia de ACR.

```bash
docker tag seabreeze/azure-mesh-helloworld:1.1-alpine <acrLoginServer>/azure-mesh-helloworld:1.1-alpine
```
### <a name="list-container-images"></a>Lista de imágenes de contenedor

En el ejemplo siguiente se enumeran los repositorios de un registro:

```azurecli
az acr repository list --name <acrName> --output table
```

Salida:

```bash
Result
----------------
azure-mesh-helloworld
```

En el ejemplo siguiente se muestran las etiquetas del repositorio **azure-mesh-helloworld**.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-mesh-helloworld --output table
```

Salida:

```bash
Result
--------
1.1-alpine
```
La salida anterior confirma la presencia de `azure-mesh-helloworld:1.1-alpine` en el registro de contenedor privado.

## <a name="retrieve-credentials-for-the-registry"></a>Recuperar las credenciales para el registro

Para implementar una instancia del contenedor del registro que se creó, se deben proporcionar las credenciales durante la implementación. Habilite al usuario administrador en el registro con el siguiente comando:

```azurecli-interactive
az acr update --name <acrName> --admin-enabled true
```

Obtenga el nombre del servidor del registro, el nombre de usuario y la contraseña mediante los siguientes comandos:

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
az acr credential show --name <acrName> --query username
az acr credential show --name <acrName> --query "passwords[0].value"
```

Los valores que proporcionan los comandos anteriores se indican como `<acrLoginServer>`, `<acrUserName>` y `<acrPassword>` en el siguiente comando.


## <a name="deploy-the-template"></a>Implementación de la plantilla

Cree la aplicación y los recursos relacionados mediante el siguiente comando, y proporcione las credenciales del paso anterior.

El parámetro `registry-password` de la plantilla es un valor `securestring`. No se mostrará en el estado de implementación y los comandos `az mesh service show`. Asegúrese de que está especificado correctamente en el siguiente comando.

```azurecli-interactive
az mesh deployment create --resource-group myResourceGroup --template-uri https://sfmeshsamples.blob.core.windows.net/templates/helloworld/mesh_rp.private_registry.linux.json --parameters "{\"location\": {\"value\": \"eastus\"}, \"registry-server\": {\"value\": \"<acrLoginServer>\"}, \"registry-username\": {\"value\": \"<acrUserName>\"}, \"registry-password\": {\"value\": \"<acrPassword>\"}}" 
```

Pasados unos minutos, el comando debe devolver lo siguiente:

`helloWorldPrivateRegistryApp has been deployed successfully on helloWorldPrivateRegistryNetwork with public ip address <IP Address>` 

## <a name="open-the-application"></a>Abrir la aplicación
Una vez que la aplicación se ha implementado correctamente, obtenga la dirección IP pública del punto de conexión de servicio y ábrala en un navegador. Mostrará una página web con el logotipo de Service Fabric Mesh.

El comando de implementación devuelve la dirección IP pública del punto de conexión de servicio. Si lo desea, también puede consultar el recurso de red para buscar la dirección IP pública del punto de conexión de servicio. 
 
El nombre de los recursos de red de esta aplicación es `helloWorldPrivateRegistryNetwork`; puede obtener información sobre él mediante el siguiente comando. 

```azurecli-interactive
az mesh network show --resource-group myResourceGroup --name helloWorldPrivateRegistryNetwork
```

## <a name="delete-the-resources"></a>Eliminación de los recursos

Para conservar los recursos limitados que se asignaron al programa de vista previa, le recomendamos que elimine los recursos con frecuencia. Para eliminar los recursos relacionados con este ejemplo, elimine el grupo de recursos en el que se implementaron.

```azurecli-interactive
az group delete --resource-group myResourceGroup 
```

## <a name="next-steps"></a>Pasos siguientes
- Échele un vistazo a la aplicación de ejemplo Hola mundo en [GitHub](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld).
- Para obtener más información sobre el modelo de recursos de Service Fabric, consulte el [modelo de recursos de Service Fabric Mesh](service-fabric-mesh-service-fabric-resources.md).
- Para obtener más información sobre Service Fabric Mesh, consulte la [información general de Service Fabric Mesh](service-fabric-mesh-overview.md).

[download-docker-server]: https://docs.docker.com/install/windows/docker-ee/
[download-docker]: https://store.docker.com/editions/community/docker-ce-desktop-windows
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/