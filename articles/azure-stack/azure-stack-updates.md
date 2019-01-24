---
title: Introducción a la administración de actualizaciones en Azure Stack | Microsoft Docs
description: Conozca sobre la administración de actualizaciones en sistemas integrados de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2019
ms.author: mabrigg
ms.openlocfilehash: d97642811d3c3422bd7cbe807303600304fafeba
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464065"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Introducción a la administración de actualizaciones en Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack*

Los paquetes de actualización de Microsoft para los sistemas integrados de Azure Stack normalmente se publican alrededor del cuarto martes de cada mes. Consulte a su OEM sobre su proceso de notificación específico para garantizar que su organización recibe notificaciones de actualización. También puede consultar esta biblioteca de documentación en **Información general** > **Notas de la versión** para información sobre las versiones que se encuentran en soporte activo. 

Cada lanzamiento de actualizaciones de software de Microsoft software viene como una única actualización. Como operador de Azure Stack, puede importar, instalar y supervisar el progreso de la instalación de estas actualizaciones desde el portal de administrador. 

El proveedor de hardware OEM (fabricante de equipo original) también lanzará actualizaciones, como las de controladores y firmware. Aunque es el fabricante del hardware OEM quien entrega estas actualizaciones como paquetes separados, se importan, instalan y administran de la misma forma que se importan, instalan y administran los paquetes de actualización de Microsoft.

Para que el sistema esté cubierto por el soporte técnico, Azure Stack debe mantenerse actualizado hasta un nivel de versión específico. Asegúrese de revisar la [directiva de servicio de Azure Stack](azure-stack-servicing-policy.md).

> [!NOTE]
> No es posible aplicar actualizaciones de Azure Stack al Kit de desarrollo de Azure Stack. Las actualizaciones están diseñadas para sistemas integrados. Para obtener información, consulte [Volver a implementar el ASDK](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>El proveedor de recursos de actualización

Azure Stack incluye un proveedor de recursos de actualización que organiza la aplicación de las actualizaciones de software de Microsoft. Este proveedor garantiza que las actualizaciones se aplican en todos los hosts físicos, entornos en tiempo de ejecución y aplicaciones de Service Fabric, y en todas las máquinas virtuales de la infraestructura y sus servicios asociados.

Cuando se instalan las actualizaciones, puede ver el estado general a medida que el proceso de actualización alcanza los diversos subsistemas de Azure Stack (por ejemplo, hosts físicos y máquinas virtuales de la infraestructura).

## <a name="plan-for-updates"></a>Planeamiento de las actualizaciones

Se recomienda firmemente notificar a los usuarios cualquier operación de mantenimiento, así como programar ventanas de mantenimiento normal durante el horario no laborable si es posible. Las operaciones de mantenimiento pueden afectar tanto a las cargas de trabajo de inquilino como a las operaciones del portal.


- Antes de iniciar la instalación de esta actualización, ejecute [Test-AzureStack](azure-stack-diagnostic-test.md) con los parámetros siguientes para validar el estado de Azure Stack y resolver todos los problemas operativos detectados, incluidas todas las advertencias y errores. Repase también las alertas activas y resuelva las que requieran alguna acción.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

## <a name="using-the-update-tile-to-manage-updates"></a>Uso del icono Actualización para administrar las actualizaciones
Puede administrar las actualizaciones desde el portal de administrador. Como operador de Azure Stack puede usar el icono Actualización del panel para:

- ver información importante, como la versión actual.
- instalar actualizaciones y supervisar el progreso.
- revisar el historial de actualización de las actualizaciones instaladas anteriormente.
 
## <a name="determine-the-current-version"></a>Determinar la versión actual

El icono Actualizar muestra la versión actual de Azure Stack. Para acceder a él, puede usar cualquiera de los siguientes métodos del portal de administrador:

- En el panel, vea la versión actual en el icono **Actualización**.
 
   ![Icono de las actualizaciones en el panel predeterminado](./media/azure-stack-updates/image1.png)
 
- En el icono **Administración de regiones**, haga clic en el nombre de la región. Vea la versión actual en el icono **Actualización**.

## <a name="next-steps"></a>Pasos siguientes

- [Directiva de servicio de Azure Stack](azure-stack-servicing-policy.md) 
- [Administración de regiones en Azure Stack](azure-stack-region-management.md)     


