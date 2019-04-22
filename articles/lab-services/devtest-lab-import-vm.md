---
title: Importación de máquinas virtuales de otro laboratorio de Azure DevTest Labs | Microsoft Docs
description: Obtenga información sobre cómo importar máquinas virtuales de otro laboratorio en el laboratorio actual de Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 9d5b7f32cb298315a5816562f548bcdafbdeb5cf
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682315"
---
# <a name="import-vms-from-another-lab-in-azure-devtest-labs"></a>Importación de máquinas virtuales de otro laboratorio de Azure DevTest Labs
El servicio Azure DevTest Labs mejora considerablemente la administración de máquinas virtuales (VM) para las actividades de desarrollo y pruebas. Permite mover una máquina virtual de un laboratorio a otro para responder a los cambios en las necesidades de los equipos o las infraestructuras. Estos son algunos escenarios comunes en los que puede ser necesario hacer el cambio: 

- Una persona del equipo cambia a otro grupo dentro de la misma empresa y desea llevarse sus máquinas virtuales de desarrollo al laboratorio del nuevo equipo.
- El grupo ha alcanzado la cuota de su nivel de suscripción y desea dividir los equipos en varias suscripciones.
- La empresa tiene previsto cambiar a Express Route (o a cualquier otra topología de redes) y el equipo quiere mover las máquinas virtuales para que usen esta nueva infraestructura.

## <a name="solution-and-constraints"></a>Solución y restricciones
Azure DevTest Labs permite a los propietarios de laboratorios importar las máquinas virtuales de un laboratorio de origen a un laboratorio de destino. Durante el proceso, el propietario del laboratorio puede proporcionar un nombre nuevo para la máquina virtual de destino. El proceso de importación incluye todas las dependencias, como discos, programaciones, configuración de red, etc. El proceso lleva cierto tiempo y se ve afectado por la cantidad y el tamaño de los discos conectados a la máquina de origen, así como a la distancia del destino (por ejemplo, región East US para la región de Asia Suroriental). Una vez completado el proceso de importación, la máquina virtual de origen permanece apagada y la nueva se ejecuta en el laboratorio de destino.

Hay dos restricciones clave que deben tenerse en cuenta al planificar la importación de máquinas virtuales a otro laboratorio:

- Es posible importar máquinas virtuales a distintas suscripciones y regiones, pero las suscripciones deben estar asociadas al mismo inquilino de Azure Active Directory.
- Las máquinas virtuales no deben estar en un estado reclamable en el laboratorio de origen.

Además, para poder importar una máquina virtual de un laboratorio a otro, deberá ser el propietario de la máquina virtual en el laboratorio de origen y el propietario del laboratorio de destino.

## <a name="steps-to-import-a-vm-from-another-lab"></a>Pasos para importar una máquina virtual a otro laboratorio
Actualmente, es necesario usar Azure PowerShell y la API de REST para importar una máquina virtual desde un laboratorio a otro solo.

### <a name="use-powershell"></a>Uso de PowerShell
Descargue en su unidad local el archivo de script de PowerShell ImportVirtualMachines.ps1 desde el [repositorio de Git de Azure DevTest Lab](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts/ImportVirtualMachines). 

#### <a name="import-a-single-vm"></a>Importación de una sola máquina virtual
Ejecute el script ImportVirtualMachines.ps1 para importar una sola máquina virtual de un laboratorio de origen a un laboratorio de destino. Puede especificar un nombre nuevo para la máquina virtual que se está copiando. Para ello, use el parámetro DestinationVirtualMachineName. 

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -SourceVirtualMachineName "<Name of the machine. Optional. If not specified, all VMs are copied>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VM is copied>" `
                            -DestinationVirtualMachineName "<New name for the VM. Optional>"
```


#### <a name="importing-all-vms"></a>Importación de todas las máquinas virtuales
Al ejecutar el script ImportVirtualMachines.ps1, si no se especifica una máquina virtual del laboratorio de origen, el script importa todas las máquinas virtuales del laboratorio de origen en el laboratorio de destino. 

```powershell
./ImportVirtualMachines.ps1 -SourceSubscriptionId "<ID of the subscription that contains the source VM>" `
                            -SourceDevTestLabName "<Name of the lab that contains the source VM>" `
                            -DestinationSubscriptionId "<ID of the target/destination subscription>" `
                            -DestinationDevTestLabName "<Name of the lab to which the VMs are copied>"
```

### <a name="use-rest-api"></a>Use la API de REST
Invoque la API de REST en el laboratorio de destino y pase como parámetros el laboratorio de origen, la suscripción y la información de la máquina virtual, tal como se muestra en el ejemplo siguiente: 

```json
POST https://management.azure.com/subscriptions/<ID of the target/destination subscription>/resourceGroups/<Name of the resource group that contains the destination lab>/providers/Microsoft.DevTestLab/labs/<Name of the lab to which the VMs are copied>/ImportVirtualMachine?api-version=2017-04-26-preview
{
   sourceVirtualMachineResourceId: "/subscriptions/<ID of the subscription that contains the source VM>/resourcegroups/<Name of the resource group that contains the source lab>/providers/microsoft.devtestlab/labs/<Name of the lab that contains the source VM>/virtualmachines/MyVm",
   destinationVirtualMachineName: "MyVmNew"
}
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo cambiar el tamaño de una máquina virtual, consulte [Cambio del tamaño de una VM](devtest-lab-resize-vm.md).
- Para obtener información sobre cómo volver a implementar una máquina virtual, consulte [Reimplementación de una máquina virtual](devtest-lab-redeploy-vm.md).


