---
title: 'Volver a entrenar un nuevo servicio web de Machine Learning Studio con PowerShell: Azure | Microsoft Docs'
description: Obtenga información sobre cómo reciclar un modelo y actualizar el servicio web para utilizar el modelo recién entrenado en Azure Machine Learning con los cmdlets de PowerShell para administración de aprendizaje automático.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: seodec18
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: 0dc41b001ecca26239c0a9e8f69e2709d6927fcd
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53250587"
---
# <a name="retrain-a-new-resource-manager-based-studio-web-service-using-powershell"></a>Repetición del entrenamiento de un nuevo servicio web de Studio basado en Resource Manager mediante Powershell
Al volver a entrenar un servicio web nuevo, actualice la definición del servicio web predictiva para hacer referencia al nuevo modelo entrenado.

## <a name="prerequisites"></a>Requisitos previos
Debe configurar un experimento de entrenamiento y predictivo tal como se muestra en los [modelos de reciclaje de Machine Learning mediante programación](retrain-models-programmatically.md).

> [!IMPORTANT]
> El experimento predictivo debe implementarse como un servicio web Machine Learning basado en Azure Resource Manager (nuevo).
> Para implementar un nuevo servicio web, debe tener permisos suficientes en la suscripción en la que lo implementa. Para obtener más información, consulte [Administración de un servicio web mediante el portal Servicios web Azure Machine Learning](manage-new-webservice.md).

Para obtener más información sobre la implementación de servicios web, vea el artículo sobre [implementación de un servicio web Azure Machine Learning](publish-a-machine-learning-web-service.md).

Este proceso requiere haber instalado los cmdlets de Azure Machine Learning. Para obtener información de instalación de los cmdlets de Machine Learning, vea la referencia sobre los [cmdlets de Azure Machine Learning](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/) en MSDN.

La siguiente información se copia desde la salida reentrenamiento:

* BaseLocation
* RelativeLocation

Los pasos son:

1. Inicie sesión en la cuenta de Azure Resource Manager
2. Obtención de la definición de servicio web
3. Exporte la definición de servicio web como JSON
4. Actualice la referencia al blob ilearner en JSON
5. Importe JSON en una definición de servicio web
6. Actualice el servicio web con la nueva definición de servicio web

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Inicie sesión en la cuenta de Azure Resource Manager
Primero debe iniciar sesión en la cuenta de Azure en el entorno de PowerShell mediante el cmdlet [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount).

## <a name="get-the-web-service-definition"></a>Obtenga la definición de servicio web
A continuación, obtenga el servicio web, llamando al cmdlet [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) . La definición de servicio web es una representación interna del modelo entrenado del servicio web y no es modificable directamente. Asegúrese de que va a recuperar la definición de servicio web para el experimento predictivo y no para el experimento de entrenamiento.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Para determinar el nombre del grupo de recursos de un servicio web existente, ejecute el cmdlet Get-AzureRmMlWebService sin ningún parámetro para mostrar los servicios web en la suscripción. Busque el servicio web y luego examine su identificador de servicio web. El nombre del grupo de recursos es el cuarto elemento del identificador, justo después del elemento *resourceGroups* . En el ejemplo siguiente, el nombre del grupo de recursos es Default-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Como alternativa, para determinar el nombre del grupo de recursos de un servicio web existente, inicie sesión en el portal de servicios web de Microsoft Azure Machine Learning. Seleccione el servicio web. El nombre del grupo de recursos es el quinto elemento de la dirección URL del servicio web, justo después del elemento *resourceGroups* . En el ejemplo siguiente, el nombre del grupo de recursos es Default-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Exporte la definición de servicio web como JSON
Para modificar la definición para el modelo entrenado para usar el modelo recién entrenado, primero debe usar el cmdlet [Export-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) para exportar a un archivo con formato JSON.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>Actualice la referencia al blob ilearner en JSON
En los recursos, busque [modelo entrenado], actualice el valor de *uri* en el nodo *locationInfo* con el identificador URI del blob ilearner. El identificador URI se genera mediante la combinación de *BaseLocation* y *RelativeLocation* a partir del resultado de la llamada de reentrenamiento de BES. Esto actualiza la ruta de acceso para hacer referencia al nuevo modelo entrenado.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Importe JSON en una definición de servicio web
Debe utilizar el cmdlet [Import-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) para convertir el archivo JSON modificado en una definición de servicio web que puede usar para actualizar la definición del servicio web.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Actualice el servicio web con la nueva definición de servicio web
Por último, utilice el cmdlet [Update-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) para actualizar la definición del servicio web.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Resumen
Con el uso de los cmdlets de administración de PowerShell para Machine Learning, puede actualizar el modelo entrenado de un servicio web predictivo habilitando escenarios como:

* Reentrenamiento de modelos periódicos con nuevos datos.
* Distribución de un modelo entre los clientes con el fin de permitirles reentrenar el modelo mediante sus propios datos.

