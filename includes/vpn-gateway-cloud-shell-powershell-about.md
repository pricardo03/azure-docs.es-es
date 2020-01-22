---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 684b212ca771af6c336cf6239e18ea367f2da5ce
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76045099"
---
En este artículo se usan cmdlets de PowerShell. Para ejecutar los cmdlets, puede usar Azure Cloud Shell, un entorno de shell interactivo hospedado en Azure y que se utiliza a través del explorador. Azure Cloud Shell viene con los cmdlets de Azure PowerShell preinstalados.

Para ejecutar cualquier código contenido en este artículo en Azure Cloud Shell, abra una sesión de Cloud Shell, utilice el botón **Copiar** en un bloque de código para copiar el código y péguelo en la sesión de Cloud Shell con __Ctrl+Mayús+V__ en Windows y Linux, o __Cmd+Mayús+V__ en macOS. El texto pegado no se ejecuta automáticamente, así que presione **ENTRAR** para ejecutar el código.

Puede iniciar Azure Cloud Shell con:

|  |   |
|-----------------------------------------------|---|
| Seleccione **Probarlo** en la esquina superior derecha de un bloque de código. Esto __no__ copia automáticamente el texto en Cloud Shell. | ![Ejemplo de Probarlo para Azure Cloud Shell](./media/cloud-shell-try-it/hdi-azure-cli-try-it.png) |
| Abra [shell.azure.com](https://shell.azure.com) en el explorador. | [![Botón Iniciar Cloud Shell](./media/cloud-shell-try-it/hdi-launch-cloud-shell.png)](https://shell.azure.com) |
| Seleccione el botón **Cloud Shell** en el menú de la esquina superior derecha de [Azure Portal](https://portal.azure.com). | ![Botón Cloud Shell en Azure Portal](./media/cloud-shell-try-it/hdi-cloud-shell-menu.png) |

**Ejecución de PowerShell localmente**

También puede instalar y ejecutar los cmdlets de Azure PowerShell localmente en el equipo. Los cmdlets de PowerShell se actualizan con frecuencia. Si no está ejecutando la última versión, los valores especificados en las instrucciones pueden dar lugar a errores. Para buscar las versiones de Azure PowerShell instaladas en el equipo, use el cmdlet `Get-Module -ListAvailable Az`. Para instalar la actualización, vea [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps).

Si PowerShell se ejecuta localmente, asegúrese de ejecutar "Connect-AzAccount" para crear una conexión a Azure.