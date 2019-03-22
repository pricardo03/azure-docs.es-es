---
title: Importar máquinas virtuales de laboratorio otro en Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo importar las máquinas virtuales de laboratorio otro en el laboratorio actual.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: spelluru
ms.openlocfilehash: 8b2eee0bfd32b58cd751f8bf70aff1d4f460a353
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340145"
---
# <a name="import-virtual-machines-from-another-lab-in-azure-devtest-labs"></a>Importar máquinas virtuales de laboratorio otro en Azure DevTest Labs
En este artículo se proporciona información acerca de cómo importar las máquinas virtuales de laboratorio otro en su laboratorio. 

## <a name="scenarios"></a>Escenarios
Estos son algunos escenarios donde es necesario para importar las máquinas virtuales de un laboratorio en otro laboratorio: 

- Un usuario individual en el equipo se mueve a otro grupo dentro de la empresa y desea tomar el escritorio del desarrollador en DevTest Labs del nuevo equipo.
- Ha llegado al grupo un [cuota de nivel de suscripción](../azure-subscription-service-limits.md) y desea dividir los equipos en varias suscripciones
- La empresa se traslada a Express Route (o algunos otro nueva topología de red) y el equipo que desea mover las máquinas virtuales para usar esta nueva infraestructura

## <a name="solution-and-constraints"></a>Solución y restricciones
Esta característica permite importar las máquinas virtuales en un laboratorio (origen) a otro laboratorio (destino). Si lo desea, puede dar un nombre nuevo para la máquina virtual de destino en el proceso. El proceso de importación incluye todas las dependencias, como discos, programaciones, configuración de red y así sucesivamente.

El proceso lleva tiempo y se ve afectado por los siguientes factores:

- Cantidad y el tamaño de los discos que están conectados a la máquina de origen (ya que es una operación de copia y no una operación de movimiento) 
- Distancia al destino (por ejemplo, región Este de Estados Unidos a Asia Suroriental).  

Una vez completado el proceso, la máquina Virtual de origen permanece apagado y la nueva uno se está ejecutando en el laboratorio de destino.

Hay dos restricciones de clave a tener en cuenta al planear importar las máquinas virtuales de un laboratorio en a otro laboratorio:

- Se admiten las importaciones de máquina virtual a través de suscripciones y regiones, pero las suscripciones deben asociarse al mismo inquilino de Azure Active Directory.
- Las máquinas virtuales no debe estar en un estado en el laboratorio de origen sea reclamable.
- Es el propietario de la máquina virtual en el laboratorio de origen y el propietario del laboratorio en el laboratorio de destino.
- Actualmente, esta característica se admite a través de Powershell y API de REST.

## <a name="use-powershell"></a>Uso de PowerShell
Descargue el archivo ImportVirtualMachines.ps1 desde el [GitHub](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/ImportVirtualMachines/ImportVirtualMachines.ps1). Puede usar la secuencia de comandos para importar todas las máquinas virtuales en el laboratorio de origen o una sola máquina virtual en el laboratorio de destino. 

### <a name="use-powershell-to-import-a-single-vm"></a>Usar PowerShell para importar una sola máquina virtual
Ejecutar este script de powershell requiere la identificación de la máquina virtual de origen y el laboratorio de destino y, opcionalmente, proporcionando un nombre nuevo que se usará para la máquina de destino:

```powershell 
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -SourceVirtualMachineName "<Name of the VM to be imported from the source lab> " `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>" `
                            -DestinationVirtualMachineName "<Optional: specify a new name for the imported VM in the destination lab>"
```

### <a name="use-powershell-to-import-all-vms-in-the-source-lab"></a>Usar PowerShell para importar todas las máquinas virtuales en el laboratorio de origen
Si no se especifica la máquina Virtual de origen, el script importa automáticamente todas las máquinas virtuales en DevTest Labs.  Por ejemplo: 
 
```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source lab>" `
                            -SourceDevTestLabName "<Name of the source lab>" `
                            -DestinationSubscriptionId "<ID of the subscription that contians the destination lab>" `
                            -DestinationDevTestLabName "<Name of the destination lab>"
```

## <a name="use-http-rest-to-import-a-vm"></a>Uso de REST de HTTP para importar una máquina virtual
La llamada REST es sencilla. Proporcionar información suficiente para identificar los recursos de origen y destino. Recuerde que la operación realiza en el recurso de laboratorio de destino.

```REST
POST https://management.azure.com/subscriptions/<DestinationSubscriptionID>/resourceGroups/<DestinationResourceGroup>/providers/Microsoft.DevTestLab/labs/<DestinationLab>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<SourceSubscriptionID>/resourcegroups/<SourceResourceGroup>/providers/microsoft.devtestlab/labs/<SourceLab>/virtualmachines/<NameofVMTobeImported>",
   destinationVirtualMachineName: "<NewNameForImportedVM>"
}
```

## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes: 

- [Set policies for a lab](devtest-lab-get-started-with-lab-policies.md) (Definición de directivas para un laboratorio)
- [Preguntas más frecuentes](devtest-lab-faq.md)
