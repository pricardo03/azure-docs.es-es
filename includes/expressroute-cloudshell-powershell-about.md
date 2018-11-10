---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/25/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 874643c5149cf6a222ab6d244dd561e5c4da5a84
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251403"
---
En este artículo se usan cmdlets de PowerShell. Para ejecutar los cmdlets, puede usar Azure Cloud Shell, un shell interactivo gratuito. Tiene las herramientas comunes de Azure preinstaladas y configuradas para usarlas en la cuenta. Simplemente haga clic en **Copiar** para copiar el código, péguelo en la instancia de Cloud Shell y, a continuación, presione Entrar para ejecutarlo. Hay unas cuantas maneras de iniciar Cloud Shell:

|  |   |
|-----------------------------------------------|---|
| Haga clic en **Probarlo** en la esquina superior derecha de un bloque de código. | ![Cloud Shell en este artículo](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-powershell-try-it.png) |
| Abra Cloud Shell en el explorador. | [![https://shell.azure.com/powershell](./media/vpn-gateway-cloud-shell-powershell/launchcloudshell.png)](https://shell.azure.com/powershell) |
| Haga clic en el botón **Cloud Shell** en el menú de la parte superior derecha de Azure Portal. | [![Cloud Shell en el portal](./media/vpn-gateway-cloud-shell-powershell/cloud-shell-menu.png)](https://portal.azure.com) |
|  |  |

Si no desea usar Azure Cloud Shell, puede instalar PowerShell localmente en su lugar. Si elige instalar y usar PowerShell de forma local, asegúrese de instalar la versión más reciente de los cmdlets de PowerShell de Azure Resource Manager. Los cmdlets de PowerShell se actualizan con frecuencia y, por lo general, deberá actualizarlos para obtener la funcionalidad más reciente de la característica. Si no actualiza los cmdlets de PowerShell, se pueden producir errores en los valores especificados. 

Para buscar la versión de PowerShell que se ejecuta localmente, use el cmdlet "Get-Module -ListAvailable AzureRM". Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). Para obtener más información, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview).