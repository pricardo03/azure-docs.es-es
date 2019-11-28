---
title: Actualización del script de implementación de plantillas de Visual Studio para usar Az PowerShell
description: Actualización del script de implementación de plantillas de Visual Studio de AzureRM a Az PowerShell
author: cweining
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: cweining
ms.openlocfilehash: c34cde5593b48c301826be3dd2641dc2490ee88d
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2019
ms.locfileid: "74149068"
---
# <a name="updating-the-visual-studio-template-deployment-script-to-use-az-powershell"></a>Actualización del script de implementación de plantillas de Visual Studio para usar Az PowerShell

Visual Studio 16.4 admite el uso de Az PowerShell en el script de implementación de plantillas. Visual Studio no instala Az PowerShell ni actualiza automáticamente el script de implementación en el proyecto del grupo de recursos. Para usar la versión más reciente de Az PowerShell, debe realizar estas cuatro acciones:
1. Desinstalación de AzureRM PowerShell
1. Instalación de Az PowerShell
1. Actualización a Visual Studio 16.4
1. Actualice el script de implementación en el proyecto.

## <a name="uninstall-azurerm-powershell"></a>Desinstalación de AzureRM PowerShell
Para desinstalar AzureRM PowerShell, siga estas [instrucciones](https://docs.microsoft.com/powershell/azure/uninstall-az-ps?view=azps-2.7.0#uninstall-the-azurerm-module).

## <a name="install-az-powershell"></a>Instalación de Az PowerShell
Para instalar Az PowerShell, siga estas [instrucciones](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-2.7.0).

## <a name="update-visual-studio-to-164"></a>Actualización de Visual Studio a la versión 16.4
Realice la actualización a Visual Studio 16.4 cuando esté disponible. Durante la actualización, asegúrese de que el componente Azure PowerShell no esté activado. Como ha instalado Az PowerShell mediante la galería, no quiere la versión AzureRM de PowerShell que se instala con Visual Studio.

Si ya ha realizado la actualización a la versión 16.4 y se activó el componente de Azure PowerShell, puede desinstalarlo mediante la ejecución del Instalador de Visual Studio y la desactivación del componente Azure PowerShell en la carga de trabajo de Azure o en la página de componentes individuales.

## <a name="update-the-deployment-script-in-your-project"></a>Actualización del script de implementación en el proyecto
Reemplace todas las apariciones de la cadena "AzureRm" por "Az" en el script de implementación. Puede hacer referencia a las revisiones de este [gist](https://gist.github.com/cweining/d2da2479418ea403499c4306dcf4f619) para ver los cambios. Para más información sobre la actualización de scripts a Az PowerShell, consulte esta [documentación](https://docs.microsoft.com/powershell/azure/migrate-from-azurerm-to-az?view=azps-2.5.0).


