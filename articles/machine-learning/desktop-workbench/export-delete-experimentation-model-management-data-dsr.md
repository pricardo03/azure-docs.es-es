---
title: Exportación o eliminación de datos de experimentación o de administración de modelos en Azure Machine Learning | Microsoft Docs
description: En Azure Machine Learning, puede exportar o eliminar los datos de su cuenta relacionados con la experimentación o la administración de modelos con Azure Portal, la CLI, el SDK y las API REST autenticadas. Este artículo le muestra cómo.
services: machine-learning
author: cjgronlund
ms.author: cgronlun
manager: haining
ms.reviewer: jmartens, mldocs
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 05/22/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 7daa3bdf9fb51fee6b0e190625f07e0d14c3d1f4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995149"
---
# <a name="export-or-delete-your-experimentation-or-model-management-data-in-machine-learning"></a>Exportar o eliminar los datos de experimentación o de administración de modelos en Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

En Azure Machine Learning, puede exportar o eliminar los datos de su cuenta relacionados con la experimentación o la administración de modelos con la API REST autenticada. En este artículo se indica cómo hacerlo.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-account-data"></a>Control de los datos de la cuenta
Los datos en productos almacenados en la experimentación y la administración de modelos de Azure Machine Learning se encuentran disponibles para exportación y eliminación mediante Azure Portal, la CLI, el SDK y las API RESR autenticadas. Se puede acceder a los datos de telemetría mediante el portal de privacidad de Azure. 

En Azure Machine Learning, los datos personales constan de información del usuario de documentos de historial de ejecución y registros de telemetría de algunas interacciones del usuario con el servicio.

## <a name="delete-account-data-with-the-rest-api"></a>Eliminación de los datos de la cuenta con la API REST 

Para eliminar datos, se pueden realizar las siguientes llamadas API con el verbo HTTP DELETE. Estas llamadas están autorizadas al tener un encabezado `Authorization: Bearer <arm-token>` en la solicitud, donde `<arm-token>` es el token de acceso de AAD del punto de conexión `https://management.core.windows.net/`.  

Para saber cómo obtener este token y llamar a los puntos de conexión de Azure, consulte la [documentación de la API REST de Azure](https://docs.microsoft.com/rest/api/azure/).  

En los ejemplos siguientes, reemplace el texto en {} por los nombres de instancia que determinan el recurso asociado.

## <a name="delete-from-a-hosting-account"></a>Eliminación de una cuenta de hospedaje

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningModelManagement/accounts/{account-name}?api-version=2017-09-01-preview      

## <a name="delete-from-the-model-management-service"></a>Eliminación desde el servicio de administración de modelos

### <a name="model-document"></a>Documento de modelo
Use esta llamada para obtener una lista de modelos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Los modelos individuales se pueden eliminar con:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview

### <a name="manifest-document"></a>Documento de manifiesto
Use esta llamada para obtener una lista de todos los manifiestos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Los manifiestos individuales se pueden eliminar con:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="service-documents"></a>Documentos de servicio
Use esta llamada para obtener una lista de todos los servicios y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Los servicios individuales se pueden eliminar con:    

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="image-document"></a>Documento de imagen
Use esta llamada para obtener una lista de todas las imágenes y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Las imágenes individuales se pueden eliminar con:  

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview

## <a name="delete-run-history-artifact-and-notification-data"></a>Eliminación de datos de historial de ejecución, artefacto y notificación
Los almacenes de historial de ejecución, artefactos y notificaciones de un proyecto se eliminan todos después de eliminar el documento de proyecto correspondiente:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview
    
## <a name="delete-from-experimentation-account-resource-provider"></a>Eliminación desde el proveedor de recursos de la cuenta experimentación
Los documentos de proyecto se eliminan con:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}/projects/{project-name}?api-version=2017-05-01-preview

Los documentos de área de trabajo se eliminan con:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}/workspaces/{workspace-name}?api-version=2017-05-01-preview

La cuenta de experimentación entera se elimina con:
    
    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.MachineLearningExperimentation/accounts/{account-name}?api-version=2017-05-01-preview


## <a name="export-service-data-with-the-rest-api"></a>Exportación de datos de servicio con la API REST
Para exportar datos, se pueden realizar las siguientes llamadas API con el verbo HTTP GET. Estas llamadas están autorizadas al tener un encabezado `Authorization: Bearer <arm-token>` en la solicitud, donde `<arm-token>` es el token de acceso de AAD del punto de conexión `https://management.core.windows.net/`.  

Para saber cómo obtener este token y llamar a los puntos de conexión de Azure, consulte la [documentación de la API REST de Azure](https://docs.microsoft.com/rest/api/azure/).   

En los ejemplos siguientes, reemplace el texto en {} por los nombres de instancia que determinan el recurso asociado.

## <a name="export-hosting-account-data"></a>Exportación de datos de la cuenta de hospedaje

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningModelManagement/accounts/{accountName}?api-version=2017-09-01-preview     

## <a name="export-model-management-service-data"></a>Exportación de datos del servicio de administración de modelos
### <a name="model-document"></a>Documento de modelo

Use esta llamada para obtener una lista de modelos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models?api-version=2017-09-01-preview"

Los modelos individuales pueden obtenerse mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/models/{modelId}?api-version=2017-09-01-preview 

### <a name="manifests"></a>Manifiestos
Use esta llamada para obtener una lista de todos los manifiestos y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests?api-version=2017-09-01-preview

Los manifiestos individuales pueden obtenerse mediante:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/manifests/{manifestId}?api-version=2017-09-01-preview

### <a name="services"></a>Services
Use esta llamada para obtener una lista de todos los servicios y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services?api-version=2017-09-01-preview

Los servicios individuales se pueden obtener mediante: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/services/{serviceName}?api-version=2017-09-01-preview

### <a name="images"></a>Imágenes
Use esta llamada para obtener una lista de todas las imágenes y sus identificadores:

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images?api-version=2017-09-01-preview

Los servicios individuales se pueden obtener mediante: 

    https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/accounts/{accountName}/images/{imageId}?api-version=2017-09-01-preview     

## <a name="export-compute-data"></a>Exportación de datos de proceso
### <a name="compute-clusters"></a>Clústeres de proceso
Use esta llamada para obtener una lista de todos los clústeres de proceso y sus nombres:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2018-03-01-preview

Los clústeres individuales se pueden obtener mediante:

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{compute-name}?api-version=2018-03-01-preview

### <a name="operationalization-clusters"></a>Clústeres de operacionalización
Use esta llamada para obtener una lista de todos los clústeres y sus nombres:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters?api-version=2017-06-01-preview

Los clústeres individuales se pueden obtener mediante:

    https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.MachineLearningCompute/operationalizationClusters/{clusterName}?api-version=2017-06-01-preview

## <a name="export-run-history-data"></a>Exportación de datos del historial de ejecución
Use esta llamada para obtener una lista de todas las ejecuciones y sus identificadores:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs

Use esta llamada para obtener una lista de todos los experimentos y sus identificadores:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments

Los elementos de historial de ejecución se pueden obtener mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}

Los elementos de métrica de ejecución se pueden obtener mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runmetrics

Los experimentos de ejecución se pueden obtener mediante:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/experiments/{experimentId}    

Artefactos de historial de ejecución:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts

URI de artefactos de historial de ejecución:

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacturi?name={artifactName}

## <a name="export-artifacts"></a>Exportación de artefactos
Use esta llamada para obtener una lista de recursos y sus nombres:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets

Use esta llamada para obtener una lista de artefactos y sus rutas de acceso:

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/origins/{origin}/containers/{runId}
        
        
### <a name="artifact-contents"></a>Contenido de artefacto

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/artifacts/contentinfo/ExperimentRun/{runId}/{artifactPath}

### <a name="artifact-documents"></a>Documentos de artefacto

    https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/runs/{runId}/artifacts
        
### <a name="assets"></a>Recursos

    https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/assets/name/{name}

## <a name="export-notifications"></a>Exportación de notificaciones

1. Vaya a la [sección Usuarios de Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/) y, a continuación, seleccione un usuario en la columna **Nombre**. 
2. Anote el **identificador de objeto** y úselo en la siguiente llamada:     

        https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}/users/{objectId}/jobs

## <a name="export-experimentation-account-information"></a>Exportación de información de la cuenta de experimentación
### <a name="experimentation-account-information"></a>Información de la cuenta de experimentación

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}?api-version=2017-05-01-preview
        
### <a name="workspace-information"></a>Información del área de trabajo

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}?api-version=2017-05-01-preview  

### <a name="project-information"></a>Información del proyecto

    https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningExperimentation/accounts/{accountName}/workspaces/{workspaceName}/projects/{projectName}?api-version=2017-05-01-preview
