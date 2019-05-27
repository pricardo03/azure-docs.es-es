---
title: Integración y entrega continuas en Azure Data Factory | Microsoft Docs
description: Aprenda a usar la integración y la entrega continuas para mover canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2019
author: gauravmalhot
ms.author: gamal
ms.reviewer: maghan
manager: craigg
ms.openlocfilehash: 76962975705ff53a292f41a0a54e42c5f2991a2c
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002722"
---
# <a name="continuous-integration-and-delivery-cicd-in-azure-data-factory"></a>Integración y entrega continuas (CI/CD) en Azure Data Factory

La integración continua es la práctica de probar cada cambio realizado en el código base automáticamente y tan pronto como sea posible. La entrega continua sigue a las pruebas realizadas durante la integración continua y envía los cambios a un sistema de ensayo o producción.

En Azure Data Factory, la integración y la entrega continuas implican el traslado de canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro. Para realizar la integración y la entrega continuas, puede usar la integración de la interfaz de usuario de Data Factory con plantillas de Azure Resource Manager. La interfaz de usuario de Data Factory puede generar una plantilla de Resource Manager al seleccionar las opciones de la **plantilla de ARM**. Al seleccionar **Export ARM template** (Exportar plantilla de ARM), el portal genera la plantilla de Resource Manager de la factoría de datos y un archivo de configuración que incluye todas las cadenas de conexión y otros parámetros. A continuación, debe crear un archivo de configuración para cada entorno (desarrollo, prueba o producción). El archivo de plantilla de Resource Manager principal sigue siendo el mismo para todos los entornos.

Si desea ver una introducción y una demostración de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-resource-manager-template-for-each-environment"></a>Creación de una plantilla de Resource Manager para cada entorno
Seleccione **Export ARM template** (Exportar plantilla de ARM) para exportar la plantilla de Resource Manager de la factoría de datos en el entorno de desarrollo.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

A continuación, vaya a la factoría de datos de prueba y la factoría de datos de producción y seleccione **Import ARM template** (Importar plantilla de ARM).

![](media/continuous-integration-deployment/continuous-integration-image2.png)

Esta acción abrirá Azure Portal, donde puede importar la plantilla exportada. Seleccione **Cree su propia plantilla en el editor** y, a continuación, **cargue el archivo** y seleccione la plantilla de Resource Manager. Defina la configuración, tras lo que se importará la factoría de datos y la canalización completa en el entorno de producción.

![](media/continuous-integration-deployment/continuous-integration-image3.png)

![](media/continuous-integration-deployment/continuous-integration-image4.png)

Seleccione **Cargar archivo** para seleccionar la plantilla de Resource Manager exportada y proporcione todos los valores de configuración (por ejemplo, los servicios vinculados).

![](media/continuous-integration-deployment/continuous-integration-image5.png)

**Cadenas de conexión**. Puede encontrar la información necesaria para crear cadenas de conexión en los artículos sobre los conectores individuales. Por ejemplo, para Azure SQL Database, consulte [Copia de datos con una base de datos de Azure SQL como origen o destino mediante Azure Data Factory](connector-azure-sql-database.md). Para comprobar la cadena de conexión correcta, por ejemplo, para un servicio vinculado, también puede abrir la vista de código para el recurso en la interfaz de usuario de Data Factory. En la vista de código, sin embargo, la parte de contraseña o clave de cuenta de la cadena de conexión se ha quitado. Para abrir la vista de código, seleccione el icono resaltado en la captura de pantalla siguiente.

![Apertura de la vista de código para ver la cadena de conexión](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="continuous-integration-lifecycle"></a>Ciclo de vida de la integración continua
Este es el ciclo de vida completo de la integración y la entrega continuas que puede usar después de habilitar la integración de GIT de Azure Repos en la interfaz de usuario de Data Factory:

1.  Configure una factoría de datos de desarrollo con Azure Repos en la que todos los desarrolladores puedan crear recursos de Data Factory, como canalizaciones, conjuntos de datos, etc.

1.  A continuación, los desarrolladores podrán modificar recursos, como las canalizaciones. A medida que realicen modificaciones, podrán seleccionar **Depurar** para ver cómo se ejecuta la canalización con los cambios más recientes.

1.  Cuando los desarrolladores hayan realizado todos los cambios necesarios, pueden crear una solicitud de incorporación de cambios de su rama a la rama master (o la rama de colaboración) para que otros equipos del mismo nivel revisen sus cambios.

1.  Una vez que los cambios se encuentren en la rama master, puede publicar en la factoría de desarrollo seleccionando **Publicar**.

1.  Cuando el equipo esté listo para promocionar los cambios a la factoría de pruebas y la de producción, podrán exportar la plantilla de Resource Manager desde la rama master o desde cualquier otra rama en caso de que la rama master respalde la instancia de desarrollo de Data Factory activa.

1.  La plantilla de Resource Manager exportada se puede implementar con archivos de parámetros distintos en la factoría de pruebas y la de producción.

## <a name="automate-continuous-integration-with-azure-pipelines-releases"></a>Automatización de la integración continua con versiones de Azure Pipelines

Estos son los pasos para configurar una versión de Azure Pipelines para que pueda automatizar la implementación de una factoría de datos en varios entornos.

![Diagrama de integración continua con Azure Pipelines](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Requisitos

-   Una suscripción a Azure vinculada a Team Foundation Server o Azure Repos que use el  [*punto de conexión de servicio de Azure Resource Manager*](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

-   Una factoría de datos que tenga configurada la integración de GIT de Azure Repos.

-   Una instancia de  [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)  que contenga los secretos.

### <a name="set-up-an-azure-pipelines-release"></a>Configuración de una versión de Azure Pipelines

1.  Vaya a la página de Azure Repos en el mismo proyecto que el configurado con Data Factory.

1.  Haga clic en el menú superior **Azure Pipelines** &gt; **Releases** (Versiones) &gt; **Create release definition** (Crear definición de versión).

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Seleccione la plantilla **Proceso vacío**.

1.  Escriba el nombre del entorno.

1.  Añada un artefacto Git y seleccione el mismo repositorio configurado con la instancia de Data Factory. Seleccione `adf_publish` como rama predeterminada con la versión predeterminada más reciente.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Añada una tarea de implementación de Azure Resource Manager:

     a.  Cree una nueva tarea, busque **Implementación de un grupo de recursos de Azure** y añádalo.

    b.  En la tarea de implementación, elija la suscripción, el grupo de recursos y la ubicación de la instancia de Data Factory de destino y proporcione las credenciales si es necesario.

    c.  Seleccione la acción **Creación o actualización del grupo de recursos**.

    d.  Seleccione **…** en el campo **Plantilla**. Busque la plantilla de Resource Manager (*ARMTemplateForFactory.json*) creada por la acción de publicación en el portal. Busque este archivo en la carpeta `<FactoryName>` de la rama `adf_publish`.

    e.  Haga lo mismo para el archivo de parámetros. Seleccione el archivo correcto en función de si ha creado una copia o si está usando el archivo predeterminado *ARMTemplateParametersForFactory.json*.

    f.  Seleccione **…** junto al campo **Reemplazar parámetros de plantilla** y rellene la información de la instancia de Data Factory de destino. Para las credenciales que proceden del almacén de claves, use el mismo nombre para el secreto con el siguiente formato: suponiendo que el nombre del secreto sea `cred1`, escriba `"$(cred1)"` (entre comillas).

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

    g. Seleccione el modo de implementación **Incremental**.

    > [!WARNING]
    > Si selecciona el modo de implementación **Complete** (Completo), se pueden eliminar los recursos existentes, lo que incluye todos los recursos del grupo de recursos de destino que no están definidos en la plantilla de Resource Manager.

1.  Guarde la canalización de versión.

1.  Cree una versión a partir de esta canalización de versión.

    ![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="optional---get-the-secrets-from-azure-key-vault"></a>Opcional: Obtenga los secretos de Azure Key Vault

Si tiene secretos para pasar en una plantilla de Azure Resource Manager, se recomienda usar Azure Key Vault con la versión de Azure Pipelines.

Hay dos formas de administrar los secretos:

1.  Agregue los secretos al archivo de parámetros. Para más información, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](../azure-resource-manager/resource-manager-keyvault-parameter.md).

    -   Cree una copia del archivo de parámetros cargado en la rama de publicación y establezca los valores de los parámetros que desea obtener del almacén de claves con el formato siguiente:

    ```json
    {
        "parameters": {
            "azureSqlReportingDbPassword": {
                "reference": {
                    "keyVault": {
                        "id": "/subscriptions/<subId>/resourceGroups/<resourcegroupId> /providers/Microsoft.KeyVault/vaults/<vault-name> "
                    },
                    "secretName": " < secret - name > "
                }
            }
        }
    }
    ```

    -   Al utilizar este método, el secreto se extrae automáticamente del almacén de claves.

    -   El archivo de parámetros también debe estar en la rama de publicación.

1.  Agregue una [tarea de Azure Key Vault](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-key-vault) antes de la implementación de Azure Resource Manager que se describió en la sección anterior:

    -   Seleccione la pestaña **Tareas**, cree una nueva tarea, busque **Azure Key Vault** y añádalo.

    -   En la tarea de Key Vault, elija la suscripción en la que se creó el almacén de claves, proporcione las credenciales si es necesario y, a continuación, seleccione el almacén de claves.

    ![](media/continuous-integration-deployment/continuous-integration-image8.png)

### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Concesión de permisos al agente de Azure Pipelines
La tarea de Azure Key Vault puede producir un error de tiempo de ejecución de fIntegration con un error acceso denegado. Descargue los registros de la versión y busque el archivo `.ps1` con el comando para conceder permisos al agente de Azure Pipelines. Puede ejecutar el comando directamente o bien copiar el identificador de entidad de seguridad del archivo y añadir la directiva de acceso manualmente en Azure Portal (*Get* y *List* son los permisos mínimos necesarios).

### <a name="update-active-triggers"></a>Actualización de desencadenadores activos
Puede producirse un error en la implementación si intenta actualizar desencadenadores activos. Para actualizar desencadenadores activos, debe detenerlos manualmente e iniciarlos después de la implementación. Puede añadir una tarea de Azure Powershell para este propósito, como se muestra en el ejemplo siguiente:

1.  En la pestaña de tareas de la versión, busque **Azure PowerShell** y agréguelo.

1.  Elija **Azure Resource Manager** como tipo de conexión y seleccione su suscripción.

1.  Elija **Script alineado** como tipo de script y, a continuación, especifique el código. En el ejemplo siguiente se detienen los desencadenadores:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

Puede seguir los mismos pasos y utilizar un código similar (con la función `Start-AzDataFactoryV2Trigger`) para reiniciar los desencadenadores después de la implementación.

> [!IMPORTANT]
> En escenarios de integración e implementación continuas, el tipo de entorno de ejecución de integración entre distintos entornos debe ser el mismo. Por ejemplo, si tiene un entorno de ejecución de integración (IR) *autohospedado* en el entorno de desarrollo, el mismo IR debe ser también de tipo *autohospedado* en otros entornos, como prueba y producción. De igual forma, si va a compartir los entornos de ejecución de integración entre varias fases, tiene que configurar las instancias de Integration Runtime como *autohospedadas vinculadas* en todos los entornos: desarrollo, prueba y producción.

## <a name="sample-deployment-template"></a>Plantilla de implementación de ejemplo

Esta es una plantilla de implementación de ejemplo que se puede importar en Azure Pipelines.

```json
{
    "source": 2,
    "id": 1,
    "revision": 51,
    "name": "Data Factory Prod Deployment",
    "description": null,
    "createdBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "createdOn": "2018-03-01T22:57:25.660Z",
    "modifiedBy": {
        "displayName": "Sample User",
        "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "uniqueName": "sampleuser@microsoft.com",
        "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
        "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
    },
    "modifiedOn": "2018-03-14T17:58:11.643Z",
    "isDeleted": false,
    "path": "\\",
    "variables": {},
    "variableGroups": [],
    "environments": [{
        "id": 1,
        "name": "Prod",
        "rank": 1,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserprod"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 1
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 2
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 3
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param\n(\n    [parameter(Mandatory = $false)] [String] $rootFolder=\"C:\\Users\\sampleuser\\Downloads\\arm_template\",\n    [parameter(Mandatory = $false)] [String] $armTemplate=\"$rootFolder\\arm_template.json\",\n    [parameter(Mandatory = $false)] [String] $armTemplateParameters=\"$rootFolder\\arm_template_parameters.json\",\n    [parameter(Mandatory = $false)] [String] $domain=\"microsoft.onmicrosoft.com\",\n    [parameter(Mandatory = $false)] [String] $TenantId=\"72f988bf-86f1-41af-91ab-2d7cd011db47\",\n    [parame",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": "5.*"
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "secret1",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/1"
    }, {
        "id": 2,
        "name": "Staging",
        "rank": 2,
        "owner": {
            "displayName": "Sample User",
            "url": "https://pde14b1dc-d2c9-49e5-88cb-45ccd58d0335.codex.ms/vssps/_apis/Identities/c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "id": "c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "uniqueName": "sampleuser@microsoft.com",
            "imageUrl": "https://sampleuser.visualstudio.com/_api/_common/identityImage?id=c9f828d1-2dbb-4e39-b096-f1c53d82bc2c",
            "descriptor": "aad.M2Y2N2JlZGUtMDViZC03ZWI3LTgxYWMtMDcwM2UyODMxNTBk"
        },
        "variables": {
            "factoryName": {
                "value": "sampleuserstaging"
            }
        },
        "variableGroups": [],
        "preDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 4
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 1
            }
        },
        "deployStep": {
            "id": 5
        },
        "postDeployApprovals": {
            "approvals": [{
                "rank": 1,
                "isAutomated": true,
                "isNotificationOn": false,
                "id": 6
            }],
            "approvalOptions": {
                "requiredApproverCount": null,
                "releaseCreatorCanBeApprover": false,
                "autoTriggeredAndPreviousEnvironmentApprovedCanBeSkipped": false,
                "enforceIdentityRevalidation": false,
                "timeoutInMinutes": 0,
                "executionOrder": 2
            }
        },
        "deployPhases": [{
            "deploymentInput": {
                "parallelExecution": {
                    "parallelExecutionType": "none"
                },
                "skipArtifactsDownload": false,
                "artifactsDownloadInput": {
                    "downloadInputs": []
                },
                "queueId": 19,
                "demands": [],
                "enableAccessToken": false,
                "timeoutInMinutes": 0,
                "jobCancelTimeoutInMinutes": 1,
                "condition": "succeeded()",
                "overrideInputs": {}
            },
            "rank": 1,
            "phaseType": 1,
            "name": "Run on agent",
            "workflowTasks": [{
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "# You can write your azure powershell scripts inline here. \n# You can also pass predefined and custom variables to this script using arguments",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $true",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }, {
                "taskId": "1e244d32-2dd4-4165-96fb-b7441ca9331e",
                "version": "1.*",
                "name": "Azure Key Vault: sampleuservault",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "KeyVaultName": "sampleuservault",
                    "SecretsFilter": "*"
                }
            }, {
                "taskId": "94a74903-f93f-4075-884f-dc11f34058b4",
                "version": "2.*",
                "name": "Azure Deployment:Create Or Update Resource Group action on sampleuser-datafactory",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceName": "e4e2ef4b-8289-41a6-ba7c-92ca469700aa",
                    "action": "Create Or Update Resource Group",
                    "resourceGroupName": "sampleuser-datafactory",
                    "location": "East US",
                    "templateLocation": "Linked artifact",
                    "csmFileLink": "",
                    "csmParametersFileLink": "",
                    "csmFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateForFactory.json",
                    "csmParametersFile": "$(System.DefaultWorkingDirectory)/Dev/ARMTemplateParametersForFactory.json",
                    "overrideParameters": "-factoryName \"$(factoryName)\" -linkedService1_connectionString \"$(linkedService1-connectionString)\" -linkedService2_connectionString \"$(linkedService2-connectionString)\"",
                    "deploymentMode": "Incremental",
                    "enableDeploymentPrerequisites": "None",
                    "deploymentGroupEndpoint": "",
                    "project": "",
                    "deploymentGroupName": "",
                    "copyAzureVMTags": "true",
                    "outputVariable": "",
                    "deploymentOutputs": ""
                }
            }, {
                "taskId": "72a1931b-effb-4d2e-8fd8-f8472a07cb62",
                "version": "2.*",
                "name": "Azure PowerShell script: FilePath",
                "refName": "",
                "enabled": true,
                "alwaysRun": false,
                "continueOnError": false,
                "timeoutInMinutes": 0,
                "definitionType": "task",
                "overrideInputs": {},
                "condition": "succeeded()",
                "inputs": {
                    "ConnectedServiceNameSelector": "ConnectedServiceNameARM",
                    "ConnectedServiceName": "",
                    "ConnectedServiceNameARM": "16a37943-8b58-4c2f-a3d6-052d6f032a07",
                    "ScriptType": "FilePath",
                    "ScriptPath": "$(System.DefaultWorkingDirectory)/Dev/deployment.ps1",
                    "Inline": "param(\n$x,\n$y,\n$z)\nwrite-host \"----------\"\nwrite-host $x\nwrite-host $y\nwrite-host $z | ConvertTo-SecureString\nwrite-host \"----------\"",
                    "ScriptArguments": "-rootFolder \"$(System.DefaultWorkingDirectory)/Dev/\" -DataFactoryName $(factoryname) -predeployment $false",
                    "TargetAzurePs": "LatestVersion",
                    "CustomTargetAzurePs": ""
                }
            }]
        }],
        "environmentOptions": {
            "emailNotificationType": "OnlyOnFailure",
            "emailRecipients": "release.environment.owner;release.creator",
            "skipArtifactsDownload": false,
            "timeoutInMinutes": 0,
            "enableAccessToken": false,
            "publishDeploymentStatus": true,
            "badgeEnabled": false,
            "autoLinkWorkItems": false
        },
        "demands": [],
        "conditions": [{
            "name": "ReleaseStarted",
            "conditionType": 1,
            "value": ""
        }],
        "executionPolicy": {
            "concurrencyCount": 1,
            "queueDepthCount": 0
        },
        "schedules": [],
        "retentionPolicy": {
            "daysToKeep": 30,
            "releasesToKeep": 3,
            "retainBuild": true
        },
        "processParameters": {
            "dataSourceBindings": [{
                "dataSourceName": "AzureRMWebAppNamesByType",
                "parameters": {
                    "WebAppKind": "$(WebAppKind)"
                },
                "endpointId": "$(ConnectedServiceName)",
                "target": "WebAppName"
            }]
        },
        "properties": {},
        "preDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "postDeploymentGates": {
            "id": 0,
            "gatesOptions": null,
            "gates": []
        },
        "badgeUrl": "https://sampleuser.vsrm.visualstudio.com/_apis/public/Release/badge/19749ef3-2f42-49b5-9696-f28b49faebcb/1/2"
    }],
    "artifacts": [{
        "sourceId": "19749ef3-2f42-49b5-9696-f28b49faebcb:a6c88f30-5e1f-4de8-b24d-279bb209d85f",
        "type": "Git",
        "alias": "Dev",
        "definitionReference": {
            "branches": {
                "id": "adf_publish",
                "name": "adf_publish"
            },
            "checkoutSubmodules": {
                "id": "",
                "name": ""
            },
            "defaultVersionSpecific": {
                "id": "",
                "name": ""
            },
            "defaultVersionType": {
                "id": "latestFromBranchType",
                "name": "Latest from default branch"
            },
            "definition": {
                "id": "a6c88f30-5e1f-4de8-b24d-279bb209d85f",
                "name": "Dev"
            },
            "fetchDepth": {
                "id": "",
                "name": ""
            },
            "gitLfsSupport": {
                "id": "",
                "name": ""
            },
            "project": {
                "id": "19749ef3-2f42-49b5-9696-f28b49faebcb",
                "name": "Prod"
            }
        },
        "isPrimary": true
    }],
    "triggers": [{
        "schedule": {
            "jobId": "b5ef09b6-8dfd-4b91-8b48-0709e3e67b2d",
            "timeZoneId": "UTC",
            "startHours": 3,
            "startMinutes": 0,
            "daysToRelease": 31
        },
        "triggerType": 2
    }],
    "releaseNameFormat": "Release-$(rev:r)",
    "url": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1",
    "_links": {
        "self": {
            "href": "https://sampleuser.vsrm.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_apis/Release/definitions/1"
        },
        "web": {
            "href": "https://sampleuser.visualstudio.com/19749ef3-2f42-49b5-9696-f28b49faebcb/_release?definitionId=1"
        }
    },
    "tags": [],
    "properties": {
        "DefinitionCreationSource": {
            "$type": "System.String",
            "$value": "ReleaseNew"
        }
    }
}
```

## <a name="sample-script-to-stop-and-restart-triggers-and-clean-up"></a>Script de ejemplo para detener y reiniciar los desencadenadores y limpiar

Este es un script de ejemplo para detener los desencadenadores antes de la implementación y reiniciarlos después. El script también incluye código para eliminar recursos que se han quitado. Para instalar la versión más reciente de Azure PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-az-ps).

```powershell
param
(
    [parameter(Mandatory = $false)] [String] $rootFolder,
    [parameter(Mandatory = $false)] [String] $armTemplate,
    [parameter(Mandatory = $false)] [String] $ResourceGroupName,
    [parameter(Mandatory = $false)] [String] $DataFactoryName,
    [parameter(Mandatory = $false)] [Bool] $predeployment=$true,
    [parameter(Mandatory = $false)] [Bool] $deleteDeployment=$false
)

$templateJson = Get-Content $armTemplate | ConvertFrom-Json
$resources = $templateJson.resources

#Triggers 
Write-Host "Getting triggers"
$triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
$triggersTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/triggers" }
$triggerNames = $triggersTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$activeTriggerNames = $triggersTemplate | Where-Object { $_.properties.runtimeState -eq "Started" -and ($_.properties.pipelines.Count -gt 0 -or $_.properties.pipeline.pipelineReference -ne $null)} | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
$deletedtriggers = $triggersADF | Where-Object { $triggerNames -notcontains $_.Name }
$triggerstostop = $triggerNames | where { ($triggersADF | Select-Object name).name -contains $_ }

if ($predeployment -eq $true) {
    #Stop all triggers
    Write-Host "Stopping deployed triggers"
    $triggerstostop | ForEach-Object { 
        Write-host "Disabling trigger " $_
        Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
else {
    #Deleted resources
    #pipelines
    Write-Host "Getting pipelines"
    $pipelinesADF = Get-AzDataFactoryV2Pipeline -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $pipelinesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/pipelines" }
    $pipelinesNames = $pipelinesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedpipelines = $pipelinesADF | Where-Object { $pipelinesNames -notcontains $_.Name }
    #datasets
    Write-Host "Getting datasets"
    $datasetsADF = Get-AzDataFactoryV2Dataset -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $datasetsTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/datasets" }
    $datasetsNames = $datasetsTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40) }
    $deleteddataset = $datasetsADF | Where-Object { $datasetsNames -notcontains $_.Name }
    #linkedservices
    Write-Host "Getting linked services"
    $linkedservicesADF = Get-AzDataFactoryV2LinkedService -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $linkedservicesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/linkedservices" }
    $linkedservicesNames = $linkedservicesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedlinkedservices = $linkedservicesADF | Where-Object { $linkedservicesNames -notcontains $_.Name }
    #Integrationruntimes
    Write-Host "Getting integration runtimes"
    $integrationruntimesADF = Get-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName
    $integrationruntimesTemplate = $resources | Where-Object { $_.type -eq "Microsoft.DataFactory/factories/integrationruntimes" }
    $integrationruntimesNames = $integrationruntimesTemplate | ForEach-Object {$_.name.Substring(37, $_.name.Length-40)}
    $deletedintegrationruntimes = $integrationruntimesADF | Where-Object { $integrationruntimesNames -notcontains $_.Name }

    #Delete resources
    Write-Host "Deleting triggers"
    $deletedtriggers | ForEach-Object { 
        Write-Host "Deleting trigger "  $_.Name
        $trig = Get-AzDataFactoryV2Trigger -name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName
        if ($trig.RuntimeState -eq "Started") {
            Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.Name -Force 
        }
        Remove-AzDataFactoryV2Trigger -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting pipelines"
    $deletedpipelines | ForEach-Object { 
        Write-Host "Deleting pipeline " $_.Name
        Remove-AzDataFactoryV2Pipeline -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting datasets"
    $deleteddataset | ForEach-Object { 
        Write-Host "Deleting dataset " $_.Name
        Remove-AzDataFactoryV2Dataset -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting linked services"
    $deletedlinkedservices | ForEach-Object { 
        Write-Host "Deleting Linked Service " $_.Name
        Remove-AzDataFactoryV2LinkedService -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }
    Write-Host "Deleting integration runtimes"
    $deletedintegrationruntimes | ForEach-Object { 
        Write-Host "Deleting integration runtime " $_.Name
        Remove-AzDataFactoryV2IntegrationRuntime -Name $_.Name -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Force 
    }

    if ($deleteDeployment -eq $true) {
        Write-Host "Deleting ARM deployment ... under resource group: " $ResourceGroupName
        $deployments = Get-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName
        $deploymentsToConsider = $deployments | Where { $_.DeploymentName -like "ArmTemplate_master*" -or $_.DeploymentName -like "ArmTemplateForFactory*" } | Sort-Object -Property Timestamp -Descending
        $deploymentName = $deploymentsToConsider[0].DeploymentName

       Write-Host "Deployment to be deleted: " $deploymentName
        $deploymentOperations = Get-AzResourceGroupDeploymentOperation -DeploymentName $deploymentName -ResourceGroupName $ResourceGroupName
        $deploymentsToDelete = $deploymentOperations | Where { $_.properties.targetResource.id -like "*Microsoft.Resources/deployments*" }

        $deploymentsToDelete | ForEach-Object { 
            Write-host "Deleting inner deployment: " $_.properties.targetResource.id
            Remove-AzResourceGroupDeployment -Id $_.properties.targetResource.id
        }
        Write-Host "Deleting deployment: " $deploymentName
        Remove-AzResourceGroupDeployment -ResourceGroupName $ResourceGroupName -Name $deploymentName
    }

    #Start Active triggers - After cleanup efforts
    Write-Host "Starting active triggers"
    $activeTriggerNames | ForEach-Object { 
        Write-host "Enabling trigger " $_
        Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_ -Force 
    }
}
```

## <a name="use-custom-parameters-with-the-resource-manager-template"></a>Usar parámetros personalizados con la plantilla de Resource Manager

Si está en modo GIT, puede invalidar las propiedades predeterminadas en la plantilla de Resource Manager para establecer las propiedades que incluyen parámetros en la plantilla y las propiedades que están codificados. Es posible que desee reemplazar la plantilla de parametrización predeterminada en estos escenarios:

* Usar CI/CD automatizada y desea cambiar algunas propiedades durante la implementación de Resource Manager, pero las propiedades no son parámetros de forma predeterminada.
* La fábrica es tan grande que la plantilla de Resource Manager de forma predeterminada no es válida porque contiene más que el máximo permitido de parámetros (256).

En estas condiciones, para reemplazar la plantilla predeterminada de la parametrización, cree un archivo denominado *arm-plantillas-parámetros-definition.json* en la carpeta raíz del repositorio. Debe coincidir exactamente con el nombre de archivo. Factoría de datos intenta leer este archivo desde cualquier rama está actualmente en el portal de Azure Data Factory, no solo desde la rama de colaboración. Puede crear o editar el archivo desde una bifurcación privada, donde puede probar los cambios mediante el uso de la **plantilla ARM exportar** en la interfaz de usuario. A continuación, puede combinar el archivo en la rama de colaboración. Si se encuentra ningún archivo, se usa la plantilla predeterminada.


### <a name="syntax-of-a-custom-parameters-file"></a>Sintaxis de un archivo de parámetros personalizados

Estas son algunas directrices que se usará al crear el archivo de parámetros personalizados. El archivo consta de una sección para cada tipo de entidad: desencadenador, canalización, linkedservice, dataset, integrationruntime y así sucesivamente.
* Escriba la ruta de acceso de propiedad en el tipo de entidad correspondiente.
* Cuando se establece un nombre de propiedad en '\*'', indica que desea parametrizar todas las propiedades en él (sólo en el primer nivel, no de forma recursiva). También puede proporcionar las excepciones a esto.
* Al establecer el valor de una propiedad como una cadena, indica que desea parametrizar la propiedad. Use el formato `<action>:<name>:<stype>`.
   *  `<action>` puede ser uno de los siguientes caracteres:
      * `=` significa mantener el valor actual como el valor predeterminado para el parámetro.
      * `-` significa que no tenga el valor predeterminado para el parámetro.
      * `|` es un caso especial para los secretos de Azure Key Vault para las cadenas de conexión o las claves.
   * `<name>` Es el nombre del parámetro. Si está en blanco, toma el nombre de la propiedad. Si el valor empieza por un `-` carácter, el nombre se ha reducido. Por ejemplo, `AzureStorage1_properties_typeProperties_connectionString` podría reducirse a `AzureStorage1_connectionString`.
   * `<stype>` es el tipo de parámetro. Si `<stype>` está en blanco, el tipo predeterminado es `string`. Valores admitidos: `string`, `bool`, `number`, `object`, y `securestring`.
* Cuando se especifica una matriz en el archivo de definición, se indica que la propiedad coincidente en la plantilla es una matriz. Factoría de datos recorre en iteración todos los objetos de la matriz mediante el uso de la definición que se especifica en el objeto en tiempo de ejecución de integración de la matriz. El segundo objeto, una cadena, se convierte en el nombre de la propiedad, que se utiliza como el nombre del parámetro para cada iteración.
* No es posible tener una definición que es específica para una instancia de recursos. Cualquier definición se aplica a todos los recursos de ese tipo.
* De forma predeterminada, todas las cadenas seguras, como secretos de Key Vault y cadenas seguras, como las cadenas de conexión, las claves y tokens, incluyen parámetros.
 
## <a name="sample-parameterization-template"></a>Plantilla de ejemplo de parametrización

```json
{
    "Microsoft.DataFactory/factories/pipelines": {
        "properties": {
            "activities": [{
                "typeProperties": {
                    "waitTimeInSeconds": "-::number",
                    "headers": "=::object"
                }
            }]
        }
    },
    "Microsoft.DataFactory/factories/integrationRuntimes": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "typeProperties": {
                "recurrence": {
                    "*": "=",
                    "interval": "=:triggerSuffix:number",
                    "frequency": "=:-freq"
                },
                "maxConcurrency": "="
            }
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "secretAccessKey": "|"
                }
            }
        },
        "AzureDataLakeStore": {
            "properties": {
                "typeProperties": {
                    "dataLakeStoreUri": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "properties": {
            "typeProperties": {
                "*": "="
            }
        }
    }
}
```

### <a name="explanation"></a>Explicación:

#### <a name="pipelines"></a>Procesos
    
* Cualquier propiedad de la ruta de acceso actividades/typeProperties/waitTimeInSeconds tiene parámetros. Esto significa que cualquier actividad en una canalización que tiene una propiedad de nivel de código denominada `waitTimeInSeconds` (por ejemplo, el `Wait` actividad) tiene parámetros como un número con un nombre predeterminado. Sin embargo, no tendrá un valor predeterminado en la plantilla de Resource Manager. Durante la implementación de Resource Manager será una entrada obligatoria.
* De forma similar, una propiedad denominada `headers` (por ejemplo, en un `Web` actividad) tiene parámetros con tipo `object` (JObject). Tiene un valor predeterminado, que es el mismo valor que en el generador del origen.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Solo las propiedades y todas las propiedades, en la ruta de acceso `typeProperties` parametrizada, con sus valores predeterminados correspondientes. Por ejemplo, a partir del esquema de hoy en día, hay dos propiedades en **IntegrationRuntimes** propiedades de tipo: `computeProperties` y `ssisProperties`. Ambos tipos de propiedad se crean con sus valores respectivos predeterminados y tipos (objeto).

#### <a name="triggers"></a>Desencadenadores

* En `typeProperties`, dos propiedades parametrizadas. La primera de ellas es `maxConcurrency`, que se especifica para tener un valor predeterminado y el tipo sería `string`. Tiene el nombre de parámetro predeterminado de `<entityName>_properties_typeProperties_maxConcurrency`.
* El `recurrence` propiedad también tiene parámetros. En ella, todas las propiedades de ese nivel se especifican que se parametrice como cadenas con valores predeterminados y los nombres de parámetro. Una excepción es el `interval` propiedad, que es parametrizar como tipo de número y con el nombre del parámetro con el sufijo `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. De forma similar, la `freq` propiedad es una cadena y tiene parámetros como una cadena. Sin embargo, el `freq` se parametriza la propiedad sin un valor predeterminado. El nombre se ha reducido y el sufijo. Por ejemplo, `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Servicios vinculados es único. Dado que los servicios vinculados y conjuntos de datos pueden ser de varios tipos, puede proporcionar personalización específicos del tipo. Por ejemplo, podría decir que para todos los servicios de tipo vinculados `AzureDataLakeStore`, será una plantilla específica aplicada y para todos los demás (a través de \*) se aplicará una plantilla diferente.
* En el ejemplo anterior, el `connectionString` propiedad será parametrizada como una `securestring` valor, no tendrá un valor predeterminado, y tendrá un nombre de parámetro abreviado que tiene el sufijo `connectionString`.
* La propiedad `secretAccessKey`, sin embargo, resulta ser una `AzureKeyVaultSecret` (por ejemplo, un `AmazonS3` servicio vinculado). Por lo tanto, se parametriza automáticamente como un secreto de Azure Key Vault y se captura desde el almacén de claves que se configura con en el generador del origen. También puede parametrizar el almacén de claves, sí.

#### <a name="datasets"></a>Conjuntos de datos

* Aunque personalización específicos del tipo está disponible para conjuntos de datos, se puede proporcionar la configuración sin necesidad de forma explícita un \*-configuración de nivel. En el ejemplo anterior, todas las propiedades del conjunto de datos de `typeProperties` incluyen parámetros.

Puede cambiar la plantilla predeterminada de la parametrización, pero se trata de la plantilla actual. Esto será útil si desea agregar una propiedad adicional como un parámetro, sino también si no desea perder las parametrizaciones automáticas existentes y deberá volver a crearlos.


```json
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }

        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```

**Ejemplo**: Agregar un identificador de clúster Databricks interactiva (desde un servicio vinculado de Databricks) al archivo de parámetros:

```
{
    "Microsoft.DataFactory/factories/pipelines": {
    },
    "Microsoft.DataFactory/factories/integrationRuntimes":{
        "properties": {
            "typeProperties": {
                "ssisProperties": {
                    "catalogInfo": {
                        "catalogServerEndpoint": "=",
                        "catalogAdminUserName": "=",
                        "catalogAdminPassword": {
                            "value": "-::secureString"
                        }
                    },
                    "customSetupScriptProperties": {
                        "sasToken": {
                            "value": "-::secureString"
                        }
                    }
                },
                "linkedInfo": {
                    "key": {
                        "value": "-::secureString"
                    },
                    "resourceId": "="
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/triggers": {
        "properties": {
            "pipelines": [{
                    "parameters": {
                        "*": "="
                    }
                },  
                "pipelineReference.referenceName"
            ],
            "pipeline": {
                "parameters": {
                    "*": "="
                }
            },
            "typeProperties": {
                "scope": "="
            }
 
        }
    },
    "Microsoft.DataFactory/factories/linkedServices": {
        "*": {
            "properties": {
                "typeProperties": {
                    "accountName": "=",
                    "username": "=",
                    "userName": "=",
                    "accessKeyId": "=",
                    "servicePrincipalId": "=",
                    "userId": "=",
                    "clientId": "=",
                    "clusterUserName": "=",
                    "clusterSshUserName": "=",
                    "hostSubscriptionId": "=",
                    "clusterResourceGroup": "=",
                    "subscriptionId": "=",
                    "resourceGroupName": "=",
                    "tenant": "=",
                    "dataLakeStoreUri": "=",
                    "baseUrl": "=",
                    "database": "=",
                    "serviceEndpoint": "=",
                    "batchUri": "=",
                    "databaseName": "=",
                    "systemNumber": "=",
                    "server": "=",
                    "url":"=",
                    "aadResourceId": "=",
                    "connectionString": "|:-connectionString:secureString",
                    "existingClusterId": "-"
                }
            }
        },
        "Odbc": {
            "properties": {
                "typeProperties": {
                    "userName": "=",
                    "connectionString": {
                        "secretName": "="
                    }
                }
            }
        }
    },
    "Microsoft.DataFactory/factories/datasets": {
        "*": {
            "properties": {
                "typeProperties": {
                    "folderPath": "=",
                    "fileName": "="
                }
            }
        }}
}
```


## <a name="linked-resource-manager-templates"></a>Plantillas vinculadas de Resource Manager

Si ha configurado la implementación e integración continua (CI/CD) para las factorías de datos, puede que observe que, a medida que la factoría crece, incurre en los límites de las plantillas de Resource Manager, como el número máximo de recursos o la carga máxima de una plantilla de Resource Manager. Para escenarios como estos, junto con la generación de la plantilla completa de Resource Manager para una factoría, ahora Data Factory también genera plantillas vinculadas de Resource Manager. Como resultado, la carga de la factoría completa se divide en varios archivos, por lo que no se incurre en los límites mencionados.

Si tiene GIT configurado, se generan las plantillas vinculadas y se guardan junto con las plantillas completas de Resource Manager en una carpeta nueva denominada `linkedTemplates` de la rama `adf_publish`.

![Carpeta de plantillas vinculadas de Resource Manager](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Normalmente las plantillas vinculadas de Resource Manager tienen una plantilla principal y un conjunto de plantillas secundarias vinculadas a la principal. La plantilla principal se denomina `ArmTemplate_master.json` y las plantillas secundarias se denominan con el patrón `ArmTemplate_0.json`, `ArmTemplate_1.json`, y así sucesivamente. Para pasar de usar la plantilla completa de Resource Manager a usar las plantillas vinculadas, actualice la tarea de CI/CD para que apunte a `ArmTemplate_master.json` en lugar de apuntar a `ArmTemplateForFactory.json` (es decir, la plantilla completa de Resource Manager). Resource Manager también requiere la carga de las plantillas vinculadas en una cuenta de almacenamiento para que Azure pueda obtener acceso a ellas durante la implementación. Para obtener más información, consulte [Deploying Linked ARM Templates with VSTS](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/) (Implementación de plantillas vinculadas de ARM con VSTS).

No se olvide de agregar los scripts de Data Factory en la canalización de CI/CD antes y después de la tarea de implementación.

Si no tiene GIT configurado, las plantillas vinculadas son accesibles a través del gesto **Exportar plantilla de ARM**.

## <a name="best-practices-for-cicd"></a>Procedimientos recomendados para CI/CD

Si usa la integración de Git con la factoría de datos y tiene una canalización de CI/CD que mueve los cambios desde el entorno de desarrollo al entorno de prueba y, luego, a producción, los procedimientos recomendados son los siguientes:

-   **Integración de Git**. Solo se necesita configurar la factoría de datos de desarrollo con la integración de Git. Los cambios en los entorno de prueba y producción se implementan a través de CI/CD y no es necesario que tengan la integración de Git.

-   **Script de CI/CD de Data Factory**. Antes del paso de implementación de Resource Manager en CI/CD, debe preocuparse de aspectos como detener los desencadenadores y de un tipo distinto de limpieza de factoría. Se recomienda usar [este script](#sample-script-to-stop-and-restart-triggers-and-clean-up) que se preocupa de todos estos aspectos. Ejecute el script una vez antes de la implementación y una vez después, mediante el uso de las marcas adecuadas.

-   **Entornos de ejecución de integración y uso compartido**. Los entornos de ejecución de integración son uno de los componentes de la infraestructura de la factoría de datos, que experimentan cambios frecuentes y son similares en todas las etapas de CI/CD. Como resultado, Data Factory espera que el usuario tenga el mismo nombre y tipo que los entornos de ejecución de integración en todas las etapas de CI/CD. Si quiere compartir los entornos de ejecución de integración en todas las etapas, por ejemplo, los entornos de ejecución de integración autohospedados, una manera de compartir es hospedando el IR autohospedado en una factoría ternaria, solo para incluir los entornos de ejecución de integración compartidos. Luego, puede usarlos en desarrollo/pruebas/producción como tipo de IR vinculado.

-   **Key Vault**. Cuando usa los servicios vinculados basados en Azure Key Vault recomendados, puede aprovecharlos todavía más si mantiene almacenes de claves independientes para desarrollo/pruebas/producción. También puede configurar niveles de permisos independientes para cada uno de ellos. Es posible que no quiera que los miembros del equipo tengan permisos para ver los secretos del entorno de producción. También se recomienda mantener los mismos nombres de los secretos en todas las etapas. Si conserva los mismos nombres, no es necesario cambiar las plantillas de Resource Manager a través de CI/CD, porque lo único que sí se debe cambiar es el nombre del almacén de claves, que es uno de los parámetros de la plantilla de Resource Manager.

## <a name="unsupported-features"></a>Características no admitidas

-   No puede publicar recursos individuales, porque las entidades de factoría de datos dependen unas de otras. Por ejemplo, los desencadenadores dependen de las canalizaciones, las canalizaciones dependen de los conjuntos de datos y otras canalizaciones, etc. Es difícil hacer un seguimiento de los cambios de las dependencias. Si era posible seleccionar los recursos para publicarlos manualmente, sería posible elegir solo un subconjunto de todo el conjunto de cambios, lo que daría lugar a un comportamiento inesperado después de la publicación.

-   No es posible publicar desde ramas privadas.

-   No es posible hospedar proyectos en Bitbucket.
