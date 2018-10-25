---
title: Actualización de imágenes del sistema con conjuntos de escalado de máquinas virtuales de Azure | Microsoft Docs
description: Aprenda a actualizar automáticamente la imagen del sistema operativo en instancias de máquinas virtuales con un conjunto de escalado.
services: virtual-machine-scale-sets
documentationcenter: ''
author: rajsqr
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2018
ms.author: rajraj
ms.openlocfilehash: cf25d08fc9a0e1ae458d350be93af31447928ecb
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49069461"
---
# <a name="azure-virtual-machine-scale-set-automatic-os-image-upgrades"></a>Actualización automática de imágenes del sistema operativo en un conjunto de escalado de máquinas virtuales de Azure

La actualización automática de imágenes del sistema operativo es una característica de los conjuntos de escalado de máquinas virtuales de Azure que actualiza automáticamente todas las máquinas virtuales a la imagen más reciente del sistema operativo.

La actualización automática del sistema operativo tiene las siguientes características:

- Una vez configurada, la imagen del sistema operativo más reciente publicada por los editores de la imagen se aplica automáticamente al conjunto de escalado sin la intervención del usuario.
- Actualiza lotes de instancias de forma gradual cada vez que el editor publica una nueva imagen de plataforma.
- Se integra con el sondeo de estado de la aplicación.
- Funciona con todos los tamaños de máquina virtual y puede usarse con imágenes de la plataforma Windows y de la plataforma Linux.
- Puede rechazar las actualizaciones automáticas en cualquier momento (las actualizaciones de sistema operativo también se pueden iniciar manualmente).
- El disco del sistema operativo de una máquina virtual se reemplaza por el nuevo disco de sistema operativo creado con la versión más reciente de la imagen. Las extensiones configuradas y los scripts de datos personalizados se ejecutan, mientras se conservan los discos de datos persistentes.
- Actualmente, no se admite Azure Disk Encryption (en versión preliminar).  

## <a name="how-does-automatic-os-image-upgrade-work"></a>¿Cómo funciona la actualización automática de imágenes del sistema operativo?

Una actualización funciona reemplazando el disco del sistema operativo de una máquina virtual por otro nuevo creado con la versión más reciente de la imagen. Las extensiones configuradas y los scripts de datos personalizados se ejecutan, mientras se conservan los discos de datos persistentes. Para minimizar el tiempo de inactividad de la aplicación, las actualizaciones se realizan por lotes de máquinas, aunque nunca se actualiza más del 20 % del conjunto de escalado a la vez. Si lo desea, puede integrar un sondeo de estado de aplicaciones de Azure Load Balancer. Se recomienda encarecidamente incorporar un latido de la aplicación y comprobar si la actualización se realizó correctamente en cada lote del proceso de actualización. Los pasos de ejecución son: 

1. Antes de comenzar el proceso de actualización, el orquestador se asegurará de que no hay más del 20 % de las instancias en mal estado. 
2. Identifique el lote de instancias de máquinas virtuales que quiere actualizar, donde un lote puede tener como máximo el 20 % de la cantidad total de instancias.
3. Actualice la imagen del sistema operativo de este lote de instancias de máquinas virtuales.
4. Si el cliente ha configurado los sondeos de estado de aplicación, la actualización espera hasta 5 minutos para que los sondeos devuelvan un estado correcto antes de pasar a la actualización del siguiente lote. 
5. Si quedan instancias por actualizar, vaya al paso 1) para el siguiente lote; en caso contrario, la actualización está completa.

El orquestador de la actualización del sistema operativo del conjunto de escalado comprueba el estado global de la instancia de la máquina virtual antes de actualizar cada lote. Mientras se actualiza un lote, es posible que estén teniendo lugar de manera simultánea otras tareas de mantenimiento planeadas o no planeadas en los centros de datos de Azure, lo cual podría afectar a la disponibilidad de sus máquinas virtuales. Por lo tanto, es posible que más del 20% de instancias estén temporalmente fuera de servicio. En tales casos, la actualización del conjunto de escalado se detiene al final del lote actual.

## <a name="supported-os-images"></a>Imágenes de sistema operativo compatibles
Actualmente se admiten solo determinadas imágenes de plataforma del sistema operativo. Actualmente no puede usar imágenes personalizadas que haya creado usted mismo. 

Actualmente se admiten las siguientes SKU (se agregarán más en el futuro):
    
| Publicador               | Sistema operativo      |  SKU               |
|-------------------------|---------------|--------------------|
| Canonical               | UbuntuServer  | 16.04-LTS          |
| Canonical               | UbuntuServer  | 18.04-LTS *        | 
| Rogue Wave (OpenLogic)  | CentOS        | 7.5 *              | 
| CoreOS                  | CoreOS        | Stable             | 
| Microsoft Corporation   | Windows Server | Centro de datos de 2012-R2 | 
| Microsoft Corporation   | Windows Server | 2016-Datacenter    | 
| Microsoft Corporation   | Windows Server | 2016-Datacenter-Smalldisk |
| Microsoft Corporation   | Windows Server | 2016-Datacenter-with-Containers |

* La compatibilidad con estas imágenes se está implantando actualmente y en breve estará disponible en todas las regiones de Azure. 

## <a name="requirements-for-configuring-automatic-os-image-upgrade"></a>Requisitos para configurar la actualización automática de imágenes del sistema operativo

- La propiedad *versión* de la imagen de plataforma debe establecerse en *más reciente*.
- Utilice sondeos de estado de aplicaciones con conjuntos de escalado que no sean de Service Fabric.
- Debe asegurarse de que los recursos a los que hace referencia el modelo del conjunto de escalado están disponibles y se mantienen actualizados. 
  El URI de Exa.SAS para la carga de arranque de las propiedades de extensiones de la máquina virtual (la carga de la cuenta de almacenamiento) hace referencia a los secretos del modelo. 

## <a name="configure-automatic-os-image-upgrade"></a>Configuración de la actualización automática de imágenes del sistema operativo
Para configurar la actualización automática de las imágenes del sistema operativo, asegúrese de que la propiedad *automaticOSUpgradePolicy.enableAutomaticOSUpgrade* está establecida en *true* en la definición del modelo del conjunto de escalado. 

```
PUT or PATCH on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet?api-version=2018-10-01`
```

```json
{ 
  "properties": { 
    "upgradePolicy": { 
      "automaticOSUpgradePolicy": { 
        "enableAutomaticOSUpgrade":  true 
      } 
    } 
  } 
} 
```

En el ejemplo siguiente, se usa la CLI de Azure (2.0.47 o posterior) para configurar las actualizaciones automáticas del conjunto de escalado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```azurecli
az vmss update --name myVMSS --resource-group myResourceGroup --set UpgradePolicy.AutomaticOSUpgradePolicy.EnableAutomaticOSUpgrade=true
```
La compatibilidad para configurar esta propiedad con Azure PowerShell estará disponible muy pronto.

## <a name="using-application-health-probes"></a>Uso de sondeos de estado de aplicaciones 

Durante una actualización del sistema operativo, las instancias de máquina virtual de un conjunto de escalado se actualizan en lote de uno en uno. La actualización debe continuar solo si la aplicación cliente se encuentra en buen estado en las instancias de máquina virtual actualizadas. Se recomienda que la aplicación proporcione señales de estado al motor de actualización de sistema operativo del conjunto de escalado. De forma predeterminada, durante las actualizaciones del sistema operativo, la plataforma se fija en el estado de energía de la máquina virtual y el estado de aprovisionamiento de la extensión para determinar si una instancia de máquina virtual está en buen estado después de una actualización. Durante la actualización del sistema operativo de una instancia de máquina virtual, se reemplaza el disco del sistema operativo en una instancia de máquina virtual por un nuevo disco basado en la versión más reciente de la imagen. Una vez finalizada la actualización del sistema operativo, las extensiones configuradas se ejecutan en estas máquinas virtuales. Solo cuando todas las extensiones en una máquina virtual se han aprovisionado correctamente, la aplicación se considera en buen estado. 

Un conjunto de escalado puede configurarse opcionalmente con sondeos de estado de aplicación para proporcionar a la plataforma información precisa sobre el estado actual de la aplicación. Los sondeos de estado de aplicación son sondeos de Load Balancer personalizados que se usan como una señal de estado. La aplicación se ejecuta en una instancia VM del conjunto de escala puede responder a solicitudes HTTP o TCP externas que indica si es correcto. Para obtener más información sobre cómo funcionan los sondeos de Load Balancer personalizados, consulte [Descripción de los sondeos del equilibrador de carga](../load-balancer/load-balancer-custom-probe-overview.md). Un sondeo de estado de aplicación no es necesario para las actualizaciones automáticas del sistema operativo, pero su uso está muy recomendado.

Si el conjunto de escalado está configurado para usar varios grupos de selección de ubicación, es necesario utilizar sondeos con una instancia de [Load Balancer estándar](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

### <a name="configuring-a-custom-load-balancer-probe-as-application-health-probe-on-a-scale-set"></a>Configuración de un sondeo de Load Balancer personalizado como sondeo de estado de aplicación en un conjunto de escalado
Como práctica recomendada, cree un sondeo del equilibrador de carga explícitamente para el estado del conjunto de escalado. Puede utilizarse el mismo punto de conexión para un sondeo HTTP o un sondeo TCP existente, pero un sondeo de estado puede requerir un comportamiento diferente por parte de un sondeo de equilibrador de carga tradicional. Por ejemplo, un sondeo de equilibrador de carga tradicional puede devolver un estado incorrecto si la carga en la instancia es demasiado alta, mientras que puede no ser adecuado para determinar el estado de la instancia durante una actualización automática del sistema operativo. Configure el sondeo para que tenga una tasa de sondeo elevada de menos de dos minutos.

Se puede hacer referencia al sondeo de equilibrador de carga en el valor *networkProfile* del conjunto de escalado, y se puede asociar con un equilibrador de carga interno o público del modo siguiente:

```json
"networkProfile": {
  "healthProbe" : {
    "id": "[concat(variables('lbId'), '/probes/', variables('sshProbeName'))]"
  },
  "networkInterfaceConfigurations":
  ...
```
> [!NOTE]
> Al utilizar actualizaciones automáticas del sistema operativo con Service Fabric, la nueva imagen del sistema operativo se implanta de dominio de actualización en dominio de actualización para mantener una alta disponibilidad de los servicios que se ejecutan en Service Fabric. Para más información sobre las características de durabilidad de los clústeres de Service Fabric, consulte [esta documentación](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster).

### <a name="keep-credentials-up-to-date"></a>Credenciales siempre actualizadas
Si el conjunto de escalado usa credenciales para acceder a recursos externos (por ejemplo, si se configura una extensión de máquina virtual que usa un token de SAS para la cuenta de almacenamiento), debe asegurarse de que las credenciales se mantengan actualizadas. Si las credenciales, incluidos los certificados y los tokens, han expirado, se producirá un error en la actualización y el primer lote de máquinas virtuales se quedará en estado de error.

Los pasos recomendados para recuperar las máquinas virtuales y volver a habilitar la actualización automática del sistema operativo si se produce un error de autenticación de recursos son:

* Volver a generar el token (o cualquier otra credencial) pasada en las extensiones.
* Asegúrese de que las credenciales que se usan en la máquina virtual para comunicarse con entidades externas están actualizadas.
* Actualizar las extensiones en el modelo de conjunto de escala con los tokens nuevos.
* Implementar el conjunto de escala actualizada, lo que actualizará todas las instancias de máquina virtual, incluyendo las que dieran error. 

## <a name="get-the-history-of-automatic-os-image-upgrades"></a>Obtención del historial de actualizaciones automáticas de imágenes del sistema operativo 
Puede comprobar el historial de la actualización más reciente del sistema operativo realizada en el conjunto de escalado con Azure PowerShell, la CLI de Azure 2.0 o las API REST. Puede obtener el historial de los últimos cinco intentos de actualización del sistema operativo realizados en los últimos dos meses.

### <a name="azure-powershell"></a>Azure PowerShell
En el ejemplo siguiente, se usa Azure PowerShell para comprobar el estado del conjunto de escalado *myVMSS* del grupo de recursos *myResourceGroup*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroup -VMScaleSetName myVMSS -OSUpgradeHistory
```

### <a name="azure-cli-20"></a>CLI de Azure 2.0
En el ejemplo siguiente, se usa la CLI de Azure (2.0.47 o posterior) para comprobar el estado del conjunto de escalado *myVMSS* del grupo de recursos *myResourceGroup*:

```azurecli
az vmss get-os-upgrade-history --resource-group myResourceGroup --name myVMSS
```

### <a name="rest-api"></a>API DE REST
En el ejemplo siguiente se usa la API de REST para comprobar el estado del conjunto de escalado denominado *myVMSS* en el grupo de recursos denominado *myResourceGroup*:

```
GET on `/subscriptions/subscription_id/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet/osUpgradeHistory?api-version=2018-10-01`
```
Consulte la documentación de esta API aquí: https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/getosupgradehistory.

La llamada GET devuelve propiedades similares a la salida del ejemplo siguiente:

```json
{
  "value": [
    {
      "properties": {
        "runningStatus": {
          "code": "RollingForward",
          "startTime": "2018-07-24T17:46:06.1248429+00:00",
          "completedTime": "2018-04-21T12:29:25.0511245+00:00"
        },
        "progress": {
          "successfulInstanceCount": 16,
          "failedInstanceCount": 0,
          "inProgressInstanceCount": 4,
          "pendingInstanceCount": 0
        },
        "startedBy": "Platform",
        "targetImageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2016-Datacenter",
          "version": "2016.127.20180613"
        },
        "rollbackInfo": {
          "successfullyRolledbackInstanceCount": 0,
          "failedRolledbackInstanceCount": 0
        }
      },
      "type": "Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades",
      "location": "westeurope"
    }
  ]
} 
```

## <a name="how-to-get-the-latest-version-of-a-platform-os-image"></a>¿Cómo obtener la versión más reciente de una imagen del sistema operativo de una plataforma? 

Puede obtener las versiones de las imágenes para la actualización automática del sistema operativo compatibles con las SKU utilizando los ejemplos siguientes: 

```
GET on `/subscriptions/subscription_id/providers/Microsoft.Compute/locations/{location}/publishers/{publisherName}/artifacttypes/vmimage/offers/{offer}/skus/{skus}/versions?api-version=2018-10-01`
```

```powershell
Get-AzureRmVmImage -Location "westus" -PublisherName "Canonical" -Offer "UbuntuServer" -Skus "16.04-LTS"
```

```azurecli
az vm image list --location "westus" --publisher "Canonical" --offer "UbuntuServer" --sku "16.04-LTS" --all
```

## <a name="deploy-with-a-template"></a>Implementación con una plantilla

Puede usar la plantilla siguiente para implementar un conjunto de escalado que usa las actualizaciones automáticas <a href='https://github.com/Azure/vm-scale-sets/blob/master/preview/upgrade/autoupdate.json'>Actualizaciones graduales automáticas - Ubuntu 16.04-LTS</a>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fvm-scale-sets%2Fmaster%2Fpreview%2Fupgrade%2Fautoupdate.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Pasos siguientes
Para obtener más ejemplos sobre cómo usar las actualizaciones automáticas de sistema operativo con conjuntos de escalado, consulte el [repositorio de GitHub para las características en versión preliminar](https://github.com/Azure/vm-scale-sets/tree/master/preview/upgrade).
