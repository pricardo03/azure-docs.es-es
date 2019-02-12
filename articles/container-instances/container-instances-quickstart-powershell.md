---
title: 'Guía de inicio rápido: Ejecución de una aplicación en Azure Container Instances: PowerShell'
description: En esta guía de inicio rápido, va a usar Azure PowerShell para implementar una aplicación de contenedor de Docker en Azure Container Instances con Azure PowerShell
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: e8efcbe080cc33cb6153d97d4435bcb477587980
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55565860"
---
# <a name="quickstart-run-a-container-application-in-azure-container-instances-with-azure-powershell"></a>Guía de inicio rápido: Ejecución de una aplicación de contenedor en Azure Container Instances con Azure PowerShell

Use Azure Container Instances para ejecutar contenedores de Docker en Azure con sencillez y velocidad. No es necesario implementar máquinas virtuales ni usar una plataforma de orquestación de contenedores completa como Kubernetes. En esta guía de inicio rápido, va a usar Azure Portal para crear un contenedor de Windows en Azure y hacer que su aplicación esté disponible con un nombre de dominio completo (FQDN). Unos pocos segundos después de ejecutar un comando de implementación único, puede ir a la aplicación en ejecución:

![Aplicación implementada en Azure Container Instances vista en el explorador][qs-powershell-01]

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell de forma local, para este tutorial se requiere la versión 5.5 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Las instancias de contenedores de Azure, al igual que otros recursos de Azure, se deben implementar en un grupo de recursos. Los grupos de recursos le permiten organizar y administrar los recursos relacionados de Azure.

Primero, cree un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus* con el siguiente comando [New-AzureRmResourceGroup][New-AzureRmResourceGroup]:

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Crear un contenedor

Ahora que tiene un grupo de recursos, puede ejecutar un contenedor en Azure. Para crear una instancia de contenedor con Azure PowerShell, proporcione un nombre de grupo de recursos, un nombre de instancia de contenedor y una imagen de contenedor de Docker al cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. En este artículo de inicio rápido, usará la imagen de Windows `microsoft/iis:nanoserver` del registro público de Docker Hub. Esta imagen empaqueta Internet Information Services (IIS) para ejecutarlo en Nano Server.

Puede exponer los contenedores en Internet mediante la especificación para que se abran uno o varios puertos, o la especificación de una etiqueta de nombre DNS o ambas. En este artículo de inicio rápido, se implementa un contenedor con una etiqueta de nombre DNS para que IIS sea públicamente accesible.

Ejecute el siguiente comando para iniciar una instancia de contenedor. El valor `-DnsNameLabel` debe ser único dentro de la región de Azure en la que cree la instancia. Si recibe un mensaje de error "DNS name label not available" (La etiqueta de nombre DNS no está disponible), pruebe otra etiqueta de nombre DNS diferente.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

Al cabo de unos segundos, debe recibir una respuesta de Azure. El estado `ProvisioningState` del contenedor es inicialmente **En creación**, pero debería cambiar a **Correcto** en un minuto o dos. Compruebe el estado de la implementación mediante el cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

El estado de aprovisionamiento, el nombre de dominio completo (FQDN) y la dirección IP del contenedor aparecen en la salida del cmdlet:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Una vez que el estado `ProvisioningState` del contenedor es **Correcto**, vaya a su `Fqdn` en el explorador. Si ve una página web parecida a la siguiente, enhorabuena. Ha implementado correctamente una aplicación que se ejecuta en un contenedor de Docker en Azure.

![IIS implementado mediante Azure Container Instances visualizado en el explorador][qs-powershell-01]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con el contenedor, elimínelo con el cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una instancia de contenedor de Azure a partir de una imagen en un repositorio público de Docker Hub. Si quiere compilar una imagen de contenedor e implementarla desde un registro de contenedor privado de Azure, vaya al tutorial de Azure Container Instances.

> [!div class="nextstepaction"]
> [Tutorial de Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
