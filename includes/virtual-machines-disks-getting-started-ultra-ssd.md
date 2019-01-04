---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/24/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ec8fa6c06dff0091627a800c895d45fd3b0e778e
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53381676"
---
# <a name="enabling-azure-ultra-ssds"></a>Habilitación de discos SSD Ultra de Azure

SSD Ultra de Azure le ofrece un alto rendimiento, IOPS elevadas y un almacenamiento en disco consistente y de baja latencia para máquinas virtuales IaaS de Azure. En esta nueva oferta se proporciona un rendimiento exclusivo que se encuentra en los mismos niveles de disponibilidad que nuestras ofertas de discos existentes. Los beneficios adicionales de Ultra SSD incluyen la capacidad de cambiar dinámicamente el rendimiento del disco junto con sus cargas de trabajo sin tener que reiniciar las máquinas virtuales. Asimismo, SSD Ultra es adecuado para cargas de trabajo con grandes cantidades de datos, como SAP HANA, bases de datos de primer nivel y cargas de trabajo que admitan muchas transacciones.

Actualmente, SSD Ultra está en versión preliminar y se debe [inscribir](https://aka.ms/UltraSSDPreviewSignUp) en la vista previa con el fin de tener acceso a ellos.

Una vez aprobada, ejecute uno de los comandos siguientes para determinar en qué zona de Este de EE. UU. 2 va a implementar SSD Ultra para:

PowerShell: `Get-AzureRmComputeResourceSku | where {$_.ResourceType -eq "disks" -and $_.Name -eq "UltraSSD_LRS" }`

CLI: `az vm list-skus --resource-type disks --query “[?name==’UltraSSD_LRS’]”`

La respuesta será similar al formulario siguiente, donde X es la zona que se utilizará para la implementación en Este de EE. UU. 2. X podría ser 1, 2 o 3.

|ResourceType  |NOMBRE  |Ubicación  |Zones  |Restricción  |Capacidad  |Valor  |
|---------|---------|---------|---------|---------|---------|---------|
|disks     |UltraSSD_LRS         |eastus2         |X         |         |         |         |

Si no hubo respuesta del comando, eso significa que el registro de la característica está pendiente o todavía no se ha aprobado.

Ahora que sabe en qué zona realizar la implementación, siga los pasos de implementación de este artículo para obtener sus primeras máquinas virtuales implementadas con discos SSD Ultra.

## <a name="deploying-an-ultra-ssd"></a>Implementación de un disco SSD Ultra

En primer lugar, determine el tamaño de la máquina virtual que se va a implementar. Como parte de esta versión preliminar, se admiten solo las familias de máquinas virtuales DsV3 y EsV3. Consulte la segunda tabla de este [blog](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/) para obtener más información acerca de estos tamaños de máquina virtual.
Consulte también el ejemplo [Creación de una máquina virtual con varios discos SSD Ultra](https://aka.ms/UltraSSDTemplate), que explica este procedimiento.

A continuación se describen los cambios nuevos o modificados en la plantilla de Resource Manager: **apiVersion** para `Microsoft.Compute/virtualMachines` y `Microsoft.Compute/Disks` debe establecerse en `2018-06-01` (o posterior).

Especifique el disco Sku UltraSSD_LRS, capacidad de disco, IOPS y rendimiento en MBps para crear un disco SSD Ultra. El siguiente es un ejemplo que crea un disco con 1024 GiB (GiB = 2^30 Bytes), 80 000 IOPS y 1 200 MBps (MBps = 10^6 Bytes por segundo):

```json
"properties": {  
    "creationData": {  
    "createOption": "Empty"  
},  
"diskSizeGB": 1024,  
"diskIOPSReadWrite": 80000,  
"diskMBpsReadWrite": 1200,  
}
```

Agregue una funcionalidad adicional en las propiedades de la máquina virtual para indicar su disco SSD Ultra habilitado (consulte el [ejemplo](https://aka.ms/UltraSSDTemplate) para ver la plantilla completa de Resource Manager):

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines",
    "properties": {
                    "hardwareProfile": {},
                    "additionalCapabilities" : {
                                    "ultraSSDEnabled" : "true"
                    },
                    "osProfile": {},
                    "storageProfile": {},
                    "networkProfile": {}
    }
}
```

Cuando se aprovisiona la máquina virtual, puede realizar una partición y dar formato a los discos de datos y configurarlos para las cargas de trabajo.

## <a name="additional-ultra-ssd-scenarios"></a>Escenarios adicionales de SSD Ultra

- Durante la creación de máquinas virtuales, los discos SSD Ultra puede crearse de forma implícita también. Sin embargo, estos discos recibirán un valor predeterminado para IOPS (500) y para el rendimiento (8 MiB/s).
- Discos SSD Ultra adicionales pueden asociarse a máquinas virtuales compatibles.
- Los discos SSD Ultra admiten el ajuste de los atributos de rendimiento del disco (IOPS y rendimiento) en tiempo de ejecución sin necesidad de desasociar el disco de la máquina virtual. Cuando se ha enviado una operación de cambio de tamaño del rendimiento del disco en un disco, este cambio puede tardar hasta una hora en surtir efecto.
- El aumento de la capacidad del disco requiere que se anule la asignación de una máquina virtual.

## <a name="next-steps"></a>Pasos siguientes

Si desea probar el nuevo tipo de disco y aún no se ha registrado para la versión preliminar, [solicite acceso mediante esta encuesta](https://aka.ms/UltraSSDPreviewSignUp).
