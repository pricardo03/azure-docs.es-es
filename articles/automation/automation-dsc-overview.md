---
title: Introducción a Azure Automation State Configuration
description: Una introducción a Azure Automation State Configuration (DSC), sus condiciones y problemas conocidos
keywords: powershell dsc, configuración de estado deseado, powershell dsc azure
services: automation
ms.service: automation
ms.component: dsc
author: DCtheGeek
ms.author: dacoulte
ms.date: 08/08/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: d2d46803f81b369f8f24a6f0e3c7f32955503e4a
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006068"
---
# <a name="azure-automation-state-configuration-overview"></a>Introducción a Azure Automation State Configuration

Azure Automation State Configuration es un servicio de Azure que le permite escribir, administrar y compilar [configuraciones](/powershell/dsc/configurations) de PowerShell Desired State Configuration (DSC), importar [recursos de DSC](/powershell/dsc/resources) y asignar configuraciones a nodos de destino, todo en la nube.

## <a name="why-use-azure-automation-state-configuration"></a>Razones para usar Azure Automation State Configuration

Azure Automation State Configuration proporciona varias ventajas con respecto a DSC fuera de Azure.

### <a name="built-in-pull-server"></a>Servidor de extracción integrado

Azure Automation State Configuration proporciona un servidor de extracción de DSC similar a la [característica de Windows DSC-Service](/powershell/dsc/pullserver) para que los nodos de destino reciban automáticamente las configuraciones, se ajusten al estado deseado y devuelvan la información de cumplimiento. El servidor de extracción integrado en Azure Automation elimina la necesidad de configurar y mantener su propio servidor de extracción. Azure Automation puede tener como destino máquinas físicas o virtuales Windows o Linux, en la nube o en un entorno local.

### <a name="management-of-all-your-dsc-artifacts"></a>Administración de todos los artefactos de DSC

Azure Automation State Configuration proporciona la misma capa de administración a [PowerShell Desired State Configuration](/powershell/dsc/overview) que ofrece en la actualidad Azure Automation para el scripting de PowerShell.

Desde Azure Portal o desde PowerShell, puede administrar todas las configuraciones, recursos y nodos de destino de DSC.

![Captura de pantalla de la hoja de Azure Automation](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Importación de datos de informes a Log Analytics

Los nodos que se administran con Azure Automation State Configuration envían datos de informe de estado detallados al servidor de extracción integrado. Puede configurar Azure Automation State Configuration para que envíe estos datos a su área de trabajo de Log Analytics. Para obtener información sobre cómo enviar datos de estado de State Configuration al área de trabajo de Log Analytics, consulte [Reenvío de datos de informes de Azure Automation State Configuration a Log Analytics](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Vídeo de presentación

¿Prefiere ver a leer? Eche un vistazo al vídeo siguiente, de mayo de 2015, cuando se anunció Azure Automation State Configuration por primera vez.

> [!NOTE]
> Aunque los conceptos y el ciclo de vida que se tratan en este vídeo son correctos, Azure Automation State Configuration ha progresado mucho desde que se grabó este vídeo. Ahora está disponible con carácter general, tiene una interfaz de usuario mucho más amplia en el Portal de Azure y admite muchas funciones adicionales.

[!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Pasos siguientes

- Para empezar a usar Azure Automation State Configuration, consulte [Introducción a Azure Automation State Configuration](automation-dsc-getting-started.md).
- Para aprender a incorporar nodos, consulte [Incorporación de máquinas para administrarlas con Azure Automation State Configuration](automation-dsc-onboarding.md)
- Para obtener información acerca de la compilación de configuraciones de DSC para que poder asignarlas a los nodos de destino, consulte [Compilación de configuraciones en Azure Automation State Configuration](automation-dsc-compile.md).
- Para la referencia de cmdlets de PowerShell, consulte [cmdlets de Azure Automation State Configuration](/powershell/module/azurerm.automation/#automation)
- Para obtener información de precios, consulte [Precios de Azure Automation State Configuration](https://azure.microsoft.com/pricing/details/automation/).
- Para ver un ejemplo del uso de Azure Automation State Configuration en una canalización de implementación continua, consulte [Implementación continua mediante Azure Automation State Configuration y Chocolatey](automation-dsc-cd-chocolatey.md)