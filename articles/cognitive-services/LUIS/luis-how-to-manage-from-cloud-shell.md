---
title: 'Datos de uso: Cloud Shell'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Descubra cómo obtener información de uso acerca del número de llamadas del punto de conexión en Azure Cloud Shell para LUIS.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 703332ece0208856bfbedb852b4b1e985d157dc9
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53605965"
---
# <a name="usage-data-for-luis-service-from-azure-cloud-shell"></a>Datos de uso del servicio LUIS desde Azure Cloud Shell

Descubra cómo obtener información de uso acerca del número de llamadas del punto de conexión en Azure Cloud Shell para LUIS.

Azure Portal permite usar cmdlets de PowerShell para trabajar con recursos de LUIS. 

Estos cmdlets permiten [crear](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/new-azurermcognitiveservicesaccount?view=azurermps-6.0.0) una suscripción de LUIS, obtener información acerca de la suscripción, incluidos los datos de [uso](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0), y [quitar](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/remove-azurermcognitiveservicesaccount?view=azurermps-6.0.0) la suscripción. 

## <a name="cloud-shell-storage-account-and-authentication"></a>Autenticación y cuenta de almacenamiento de Cloud Shell

Para poder usar PowerShell en [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell) de Azure Portal, debe tener una cuenta de Azure Storage. Si no tiene una [cuenta de almacenamiento](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage), se le pedirá que cree una. La cuenta de almacenamiento permite guardar scripts de PowerShell en Cloud Shell.  

También debe autenticarse en Azure en Cloud Shell para acceder a los recursos. 

Cuando tenga una cuenta de almacenamiento y se haya autenticado, puede ejecutar cmdlets de PowerShell.

## <a name="open-cloud-shell"></a>Apertura de Cloud Shell

Si usa Cloud Shell de Azure Portal, siempre tendrá la versión más reciente de PowerShell. 

Use el botón **Iniciar Cloud Shell** para abrir Cloud Shell o abra un explorador con [https://shell.azure.com](https://shell.azure.com). Seleccione PowerShell como entorno. Si no tiene ninguna cuenta de almacenamiento de Azure, deberá crear una. 

<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><image src="https://shell.azure.com/images/launchcloudshell.png" alt="Start powershell" /></a>

## <a name="luis-endpoint-usage-information"></a>Información de uso del punto de conexión de LUIS

El cmdlet de PowerShell 6.x, `Get-AzureRmCognitiveServicesAccountUsage`, proporciona información de uso de Microsoft Cognitive Services, incluido LUIS. [Get-AzureRmCognitiveServicesAccountUsage](https://docs.microsoft.com/powershell/module/azurerm.cognitiveservices/get-azurermcognitiveservicesaccountusage?view=azurermps-6.0.0) requiere el nombre del recurso y el grupo de recursos del servicio. 

La sintaxis del comando es la siguiente:

```powershell
Get-AzureRmCognitiveServicesAccountUsage -ResourceGroupName my-resource-group -Name my-luis-service-name
```

En el ejemplo siguiente, el nombre del grupo de recursos es `luis-westus-rg` y el nombre de suscripción del servicio LUIS es `luis-westus-1`. Ambos nombres se eligen cuando se crea el servicio LUIS. 

El cmdlet devuelve información de uso de 16 de 10 000 coincidencias de punto de conexión utilizadas en un período de 30 días con el período que finaliza el 7 de junio:

```powershell
CurrentValue  : 16
Name          : LUIS.Calls
Limit         : 10000
Status        : Included
Unit          : Count
QuotaPeriod   : 30.00:00:00
NextResetTime : 2018-06-07T18:28:52Z
```

Guarde el comando como un archivo de PowerShell, *.ps1, en la cuenta de Azure Storage asociada a Cloud Shell y ejecútelo en cualquier momento. 

![Ejecutar el script desde el almacenamiento](./media/luis-how-to-manage-from-powershell/run-script-from-storage.png)

Una vez que se guarda el script en la unidad de la nube, puede ejecutar el script de PowerShell desde Cloud Shell de la aplicación móvil de Azure. 

![Ejecutar el script desde el almacenamiento de la aplicación móvil](./media/luis-how-to-manage-from-powershell/phone-app.png)