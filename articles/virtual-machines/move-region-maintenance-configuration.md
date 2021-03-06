---
title: Traslado de una configuración de mantenimiento a otra región de Azure
description: Obtenga más información sobre cómo mover una configuración de mantenimiento a otra región de Azure.
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm
ms.date: 03/04/2020
ms.author: shants
ms.openlocfilehash: fe03bead238d3fb7bda3ee685bd5587c3e0dbc58
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78304343"
---
# <a name="move-a-maintenance-control-configuration-to-another-region"></a>Traslado de una configuración de control de mantenimiento a otra región

Siga este artículo para mover una configuración de control de mantenimiento a otra región de Azure. Puede que quiera mover una configuración por una serie de motivos. Por ejemplo, para aprovechar una nueva región, para implementar características o servicios que solo están disponibles en una región concreta, para cumplir los requisitos de gobernanza y las directivas internas o en respuesta al planeamiento de capacidad.

El control de mantenimiento, con configuraciones de mantenimiento personalizadas, le permite controlar cómo se aplican las actualizaciones de plataforma a VM [Windows](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json) y [Linux](https://docs.microsoft.com/azure/virtual-machines/maintenance-control-cli?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json&bc=%2Fazure%2Fvirtual-machines%2Flinux%2Fbreadcrumb%2Ftoc.json&view=azure-java-stable) y a instancias de Azure Dedicated Host. Hay un par de escenarios para mover el control de mantenimiento entre regiones:

- Para mover la configuración de control de mantenimiento, pero no los recursos asociados a la configuración, siga las instrucciones de este artículo.
- Para mover los recursos asociados a una configuración de mantenimiento, pero no la propia configuración, siga [estas instrucciones](move-region-maintenance-configuration-resources.md).
- Para mover tanto la configuración de mantenimiento como los recursos asociados a ella, primero siga las instrucciones de este artículo. A continuación, siga [estas instrucciones](move-region-maintenance-configuration-resources.md).

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar a mover una configuración de control de mantenimiento:

- Las configuraciones de mantenimiento están asociadas a VM de Azure o instancias de Azure Dedicated Host. Antes de empezar, asegúrese de que los recursos de host o VM existen en la nueva región.
- Identifique: 
    - Las configuraciones de control de mantenimiento.
    - Los grupos de recursos en los que residen las configuraciones existentes actualmente. 
    - Los grupos de recursos a los que se agregarán las configuraciones tras moverlos a la región nueva. 
    - Los recursos asociados a la configuración de mantenimiento que quiere mover.
    - Compruebe que los recursos de la nueva región son los mismos que los asociados a las configuraciones de mantenimiento actuales. Las configuraciones pueden tener los mismos nombres en la nueva región que tenían en la antigua, pero no es necesario.

## <a name="prepare-and-move"></a>Preparación y traslado 

1. Recupere todas las configuraciones de mantenimiento de cada suscripción. Para ello, ejecute el comando [az maintenance configuration list](https://docs.microsoft.com/cli/azure/ext/maintenance/maintenance/configuration?view=azure-cli-latest#ext-maintenance-az-maintenance-configuration-list) de la CLI mediante la sustitución de $subId por el id. de suscripción.

    ```
    az maintenance configuration list --subscription $subId --query "[*].{Name:name, Location:location, ResGroup:resourceGroup}" --output table
    ```
2. Revise la lista de tablas de registros de configuración que se ha devuelto con la suscripción. Este es un ejemplo. La lista incluirá los valores de su entorno específico.

    **Nombre** | **Ubicación** | **Grupos de recursos**
    --- | --- | ---
    Skip Maintenance | eastus2 | configuration-resource-group
    IgniteDemoConfig | eastus2 | configuration-resource-group
    defaultMaintenanceConfiguration-eastus | estado | test-configuration
    

3. Guarde la lista para tenerla como referencia. A medida que mueve las configuraciones, le ayudará a comprobar que se ha movido todo el contenido.
4. Como referencia, asigne cada configuración o grupo de recursos al nuevo grupo de recursos en la región nueva.
5. Cree nuevas configuraciones de mantenimiento en la región nueva con [PowerShell](../virtual-machines/maintenance-control-powershell.md#create-a-maintenance-configuration) o la [CLI](../virtual-machines/maintenance-control-cli.md#create-a-maintenance-configuration).
6. Asocie las configuraciones a los recursos en la región nueva mediante [PowerShell](../virtual-machines/maintenance-control-powershell.md#assign-the-configuration) o la [CLI](../virtual-machines/maintenance-control-cli.md#assign-the-configuration).


## <a name="verify-the-move"></a>Comprobación del traslado

Tras mover las configuraciones, compárelas con los recursos en la región nueva con la lista de tablas que ha preparado.


## <a name="clean-up-source-resources"></a>Limpieza de los recursos de origen

Tras el traslado, considere la posibilidad de eliminar las configuraciones de mantenimiento en la región de origen con [PowerShell](../virtual-machines/maintenance-control-powershell.md#remove-a-maintenance-configuration) o la [CLI](../virtual-machines/maintenance-control-cli.md#delete-a-maintenance-configuration).


## <a name="next-steps"></a>Pasos siguientes

Siga [estas instrucciones](move-region-maintenance-configuration-resources.md) si necesita trasladar los recursos asociados a las configuraciones de mantenimiento. 
