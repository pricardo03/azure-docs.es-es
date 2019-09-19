---
title: Integración y entrega continuas en Azure Data Factory | Microsoft Docs
description: Aprenda a usar la integración y la entrega continuas para mover canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: jroth
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: e522cba88eaf9cb63ef7ef2f20e3b72691261073
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002401"
---
# <a name="continuous-integration-and-delivery-cicd-in-azure-data-factory"></a>Integración y entrega continuas (CI/CD) en Azure Data Factory

## <a name="overview"></a>Información general

La integración continua es la práctica de probar cada cambio realizado en el código base automáticamente y tan pronto como sea posible. La entrega continua sigue a las pruebas realizadas durante la integración continua y envía los cambios a un sistema de ensayo o producción.

En Azure Data Factory, la integración y la entrega continuas implican el traslado de canalizaciones de Data Factory de un entorno (desarrollo, prueba o producción) a otro. Para realizar la integración y la entrega continuas, puede usar la integración de la experiencia de la interfaz de usuario de Data Factory con plantillas de Azure Resource Manager. La experiencia de la interfaz de usuario de Data Factory puede generar una plantilla de Resource Manager en la lista desplegable **Plantilla de ARM**. Al seleccionar **Export ARM template** (Exportar plantilla de ARM), el portal genera la plantilla de Resource Manager de la factoría de datos y un archivo de configuración que incluye todas las cadenas de conexión y otros parámetros. A continuación, debe crear un archivo de configuración para cada entorno (desarrollo, prueba o producción). El archivo de plantilla de Resource Manager principal sigue siendo el mismo para todos los entornos.

Si desea ver una introducción y una demostración de esta característica, vea el siguiente vídeo de nueve minutos de duración:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-integration-and-deployment-using-Azure-Data-Factory/player]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="continuous-integration-lifecycle"></a>Ciclo de vida de la integración continua

A continuación se muestra una descripción general de ejemplo del ciclo de vida de la integración y entrega continuas en una factoría de datos de Azure configurada con Git de Azure Repos. Para más información sobre cómo configurar un repositorio Git, vea [Control de código fuente en Azure Data Factory](source-control.md).

1.  Se crea una factoría de datos de desarrollo y se configura con Azure Repos Git, donde todos los desarrolladores tienen permiso para crear recursos de Data Factory, como canalizaciones y conjuntos de datos.

1.  A medida que los desarrolladores realizan cambios en su rama de características, depuran las ejecuciones de la canalización con sus cambios más recientes. Para más información sobre cómo depurar una ejecución de canalización, consulte [Desarrollo y depuración iterativos con Azure Data Factory](iterative-development-debugging.md).

1.  Una vez que los desarrolladores hayan realizado todos los cambios necesarios, crean una solicitud de incorporación de cambios de su rama de características a la rama maestra o rama de colaboración para que otros equipos del mismo nivel revisen sus cambios.

1.  Una vez que la solicitud de incorporación de cambios se ha aprobado y los cambios se han combinado en la rama maestra, pueden publicar en la factoría de desarrollo.

1.  Cuando el equipo está listo para implementar los cambios en la factoría de pruebas y después en la factoría de producción, exportan la plantilla de Resource Manager desde la rama maestra.

1.  La plantilla de Resource Manager exportada se implementa con archivos de parámetros distintos en la factoría de pruebas y la de producción.

## <a name="create-a-resource-manager-template-for-each-environment"></a>Creación de una plantilla de Resource Manager para cada entorno

En la lista desplegable **Plantilla de ARM**, seleccione **Export ARM template** (Exportar plantilla de ARM) para exportar la plantilla de Resource Manager de la factoría de datos en el entorno de desarrollo.

![](media/continuous-integration-deployment/continuous-integration-image1.png)

En las factorías de datos de prueba y de producción, seleccione **Import ARM template** (Importar plantilla de ARM). Esta acción abrirá Azure Portal, donde puede importar la plantilla exportada. Seleccione **Cree su propia plantilla en el editor** para abrir el editor de plantillas de Resource Manager.

![](media/continuous-integration-deployment/continuous-integration-image3.png) 

Haga clic en **Cargar archivo** y seleccione la plantilla de Resource Manager generada.

![](media/continuous-integration-deployment/continuous-integration-image4.png)

En el panel de configuración, escriba los valores de configuración, como las credenciales del servicio vinculado. Cuando haya terminado, haga clic en **Comprar** para implementar la plantilla de Resource Manager.

![](media/continuous-integration-deployment/continuous-integration-image5.png)

### <a name="connection-strings"></a>Cadenas de conexión

Puede encontrar información sobre cómo configurar las cadenas de conexión en cada artículo del conector. Por ejemplo, para Azure SQL Database, consulte [Copia de datos con una base de datos de Azure SQL como origen o destino mediante Azure Data Factory](connector-azure-sql-database.md). Para comprobar una cadena de conexión, puede abrir la vista de código para el recurso en la experiencia de la interfaz de usuario de Data Factory. En la vista de código, la parte de contraseña o clave de cuenta de la cadena de conexión se ha quitado. Para abrir la vista de código, seleccione el icono resaltado en la captura de pantalla siguiente.

![Apertura de la vista de código para ver la cadena de conexión](media/continuous-integration-deployment/continuous-integration-codeview.png)

## <a name="automate-continuous-integration-with-azure-pipelines-releases"></a>Automatización de la integración continua con versiones de Azure Pipelines

A continuación se presenta una guía para configurar una versión de Azure Pipelines, que automatiza la implementación de una factoría de datos en varios entornos.

![Diagrama de integración continua con Azure Pipelines](media/continuous-integration-deployment/continuous-integration-image12.png)

### <a name="requirements"></a>Requisitos

-   Una suscripción a Azure vinculada a Team Foundation Server o Azure Repos que use el  [punto de conexión de servicio de Azure Resource Manager](https://docs.microsoft.com/azure/devops/pipelines/library/service-endpoints#sep-azure-rm).

-   Una instancia de Data Factory que tenga configurada la integración de Git de Azure Repos.

-   Una instancia de  [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) que contenga los secretos para cada entorno.

### <a name="set-up-an-azure-pipelines-release"></a>Configuración de una versión de Azure Pipelines

1.  En la [experiencia del usuario de Azure DevOps](https://dev.azure.com/), abra el proyecto configurado con Data Factory.

1.  En el lado izquierdo de la página, haga clic en **Canalizaciones** y seleccione **Versiones**.

    ![](media/continuous-integration-deployment/continuous-integration-image6.png)

1.  Seleccione **Nueva canalización** o, si tiene canalizaciones existentes, **Nueva** y, luego **Nueva canalización de versión**.

1.  Seleccione la plantilla **Fase vacía**.

    ![](media/continuous-integration-deployment/continuous-integration-image13.png)

1.  En el campo **Nombre de la fase**, escriba el nombre del entorno.

1.  Seleccione **Agregar un artefacto** y seleccione el mismo repositorio configurado con la instancia de Data Factory. Seleccione `adf_publish` como rama predeterminada con la versión predeterminada más reciente.

    ![](media/continuous-integration-deployment/continuous-integration-image7.png)

1.  Añada una tarea de implementación de Azure Resource Manager:

    a.  En la vista de fase, haga clic en el vínculo **Ver tareas de la fase**.

    ![](media/continuous-integration-deployment/continuous-integration-image14.png)

    b.  Cree una nueva tarea. Busque **Implementación de un grupo de recursos de Azure** y haga clic en **Agregar**.

    c.  En la tarea de implementación, elija la suscripción, el grupo de recursos y la ubicación de la instancia de Data Factory de destino y proporcione las credenciales si es necesario.

    d.  En la lista desplegable de acciones, seleccione **Create or update resource group** (Crear o actualizar grupo de recursos).

    e.  Seleccione **…** en el campo **Plantilla**. Busque la plantilla de Azure Resource Manager creada mediante el paso **Import ARM template** (Importar plantilla de ARM) de la sección [Creación de una plantilla de Resource Manager para cada entorno](continuous-integration-deployment.md#create-a-resource-manager-template-for-each-environment). Busque este archivo en la carpeta `<FactoryName>` de la rama `adf_publish`.

    f.  Seleccione **…** en el campo **Parámetros de la plantilla**. para elegir el archivo de parámetros. Seleccione el archivo correcto en función de si ha creado una copia o si está usando el archivo predeterminado *ARMTemplateParametersForFactory.json*.

    g.  Seleccione **…** junto al campo **Reemplazar parámetros de plantilla** y rellene la información de la instancia de Data Factory de destino. En el caso de las credenciales que proceden del almacén de claves, escriba el nombre del secreto entre comillas dobles. Por ejemplo, si el nombre del secreto es `cred1`, escriba `"$(cred1)"` para su valor.

    ![](media/continuous-integration-deployment/continuous-integration-image9.png)

    h. Seleccione el modo de implementación **Incremental**.

    > [!WARNING]
    > Si selecciona el modo de implementación **Completo**, se pueden eliminar los recursos existentes, lo que incluye todos los recursos del grupo de recursos de destino que no están definidos en la plantilla de Resource Manager.

1.  Guarde la canalización de versión.

1. Para desencadenar una versión, haga clic en **Crear versión**.

![](media/continuous-integration-deployment/continuous-integration-image10.png)

### <a name="get-secrets-from-azure-key-vault"></a>Obtención de secretos de Azure Key Vault

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

#### <a name="grant-permissions-to-the-azure-pipelines-agent"></a>Concesión de permisos al agente de Azure Pipelines

La tarea Azure Key Vault puede producir un error de acceso denegado si no están presentes los permisos adecuados. Descargue los registros de la versión y busque el archivo `.ps1` con el comando para conceder permisos al agente de Azure Pipelines. Puede ejecutar el comando directamente o bien copiar el identificador de entidad de seguridad del archivo y añadir la directiva de acceso manualmente en Azure Portal **Get** y **List** son los permisos mínimos necesarios.

### <a name="update-active-triggers"></a>Actualización de desencadenadores activos

Puede producirse un error en la implementación si intenta actualizar desencadenadores activos. Para actualizar desencadenadores activos, debe detenerlos manualmente e iniciarlos después de la implementación. Puede hacerlo a través de una tarea de Azure PowerShell.

1.  En la pestaña Tareas de la versión, agregue la tarea **Azure PowerShell**.

1.  Elija **Azure Resource Manager** como tipo de conexión y seleccione su suscripción.

1.  Elija **Script alineado** como tipo de script y, a continuación, especifique el código. En el ejemplo siguiente se detienen los desencadenadores:

    ```powershell
    $triggersADF = Get-AzDataFactoryV2Trigger -DataFactoryName $DataFactoryName -ResourceGroupName $ResourceGroupName

    $triggersADF | ForEach-Object { Stop-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $_.name -Force }
    ```

    ![](media/continuous-integration-deployment/continuous-integration-image11.png)

Puede seguir los mismos pasos (con la función `Start-AzDataFactoryV2Trigger`) para reiniciar los desencadenadores después de la implementación.

> [!IMPORTANT]
> En escenarios de integración e implementación continuas, el tipo de entorno de ejecución de integración entre distintos entornos debe ser el mismo. Por ejemplo, si tiene un entorno de ejecución de integración (IR) *autohospedado* en el entorno de desarrollo, el mismo IR debe ser también de tipo *autohospedado* en otros entornos, como prueba y producción. De igual forma, si va a compartir los entornos de ejecución de integración entre varias fases, tiene que configurar las instancias de Integration Runtime como *autohospedadas vinculadas* en todos los entornos: desarrollo, prueba y producción.

#### <a name="sample-prepostdeployment-script"></a>Script de ejemplo anterior y posterior a la implementación

A continuación se muestra un script de ejemplo para detener los desencadenadores antes de la implementación y reiniciarlos después. El script también incluye código para eliminar recursos que se han quitado. Para instalar la versión más reciente de Azure PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-az-ps).

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

Si está en modo GIT, puede reemplazar las propiedades predeterminadas en la plantilla de Resource Manager para establecer propiedades que están parametrizadas en la plantilla y propiedades que están codificadas de forma rígida. Es posible que desee reemplazar la plantilla de parametrización predeterminada en estos escenarios:

* Se usa CI/CD automatizada y se quieren cambiar algunas propiedades durante la implementación de Resource Manager, pero las propiedades no están parametrizadas de forma predeterminada.
* La fábrica es tan grande que la plantilla de Resource Manager predeterminada no es válida porque contiene más parámetros que el número máximo permitido (256).

En estas condiciones, para reemplazar la plantilla predeterminada de parametrización, cree un archivo denominado *arm-template-parameters-definition.json* en la carpeta raíz del repositorio. El nombre de archivo debe coincidir exactamente. Data Factory intenta leer el archivo de la rama en la que está actualmente en el portal de Azure Data Factory, no solo de la rama de colaboración. Puede crear o editar el archivo desde una rama privada, donde pueda probar los cambios usando la **plantilla de exportación de ARM** en la interfaz de usuario. A continuación, puede combinar el archivo en la rama de colaboración. Si no se encuentra ningún archivo, se usa la plantilla predeterminada.


### <a name="syntax-of-a-custom-parameters-file"></a>Sintaxis de un archivo de parámetros personalizados

Estas son algunas directrices para usar durante la creación del archivo de parámetros personalizados. El archivo consta de una sección para cada tipo de entidad: desencadenador, canalización, servicio vinculado, conjunto de datos, entorno de ejecución de integración, etc.
* Escriba la ruta de acceso de la propiedad en el tipo de entidad correspondiente.
* Cuando se establece un nombre de propiedad en '\*'', indica que quiere parametrizar todas las propiedades que incluye (solo en el primer nivel, no de forma recursiva). También puede proporcionar excepciones.
* Al establecer el valor de una propiedad como una cadena, indica que desea parametrizar la propiedad. Use el formato `<action>:<name>:<stype>`.
   *  `<action>` puede ser uno de los siguientes caracteres:
      * `=` significa que el valor actual debe conservarse como el valor predeterminado para el parámetro.
      * `-` significa que no se debe conservar el valor predeterminado para el parámetro.
      * `|` es un caso especial para los secretos de Azure Key Vault para una cadena de conexión o claves.
   * `<name>` es el nombre del parámetro. Si está en blanco, toma el nombre de la propiedad. Si el valor empieza por un carácter `-`, el nombre está abreviado. Por ejemplo, `AzureStorage1_properties_typeProperties_connectionString` se abreviará a `AzureStorage1_connectionString`.
   * `<stype>` es el tipo del parámetro. Si `<stype>` está en blanco, el tipo predeterminado es `string`. Los valores admitidos son: `string`, `bool`, `number`, `object` y `securestring`.
* Cuando especifica una matriz en el archivo de definición, se indica que la propiedad coincidente en la plantilla es una matriz. Data Factory recorre en iteración todos los objetos de la matriz utilizando la definición especificada en el objeto del entorno de ejecución de integración de la matriz. El segundo objeto, una cadena, se convierte en el nombre de la propiedad, que se utiliza como el nombre del parámetro para cada iteración.
* No es posible una definición que sea específica para una instancia de recursos. Cualquier definición se aplica a todos los recursos de ese tipo.
* De forma predeterminada, todas las cadenas seguras, como los secretos de Key Vault, y las cadenas seguras, como las cadenas de conexión, las claves y los tokens, están parametrizadas.
 
### <a name="sample-parameterization-template"></a>Plantilla de parametrización de ejemplo

A continuación se muestra un ejemplo de cómo puede ser una plantilla de parametrización:

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
A continuación se muestra una explicación de cómo se construye la plantilla anterior, desglosada por tipo de recurso.

#### <a name="pipelines"></a>Procesos
    
* Cualquier propiedad de la ruta de acceso activities/typeProperties/waitTimeInSeconds está parametrizada. Cualquier actividad en una canalización que tiene una propiedad de nivel de código denominada `waitTimeInSeconds` (por ejemplo, la actividad `Wait`) está parametrizada como un número, con un nombre predeterminado. Sin embargo, no tendrá un valor predeterminado en la plantilla de Resource Manager. Será una entrada obligatoria durante la implementación de Resource Manager.
* De forma similar, una propiedad denominada `headers` (por ejemplo, en una actividad `Web`) está parametrizada con el tipo `object` (JObject). Tiene un valor predeterminado, que es el mismo valor que en la fábrica de origen.

#### <a name="integrationruntimes"></a>IntegrationRuntimes

* Todas las propiedades bajo la ruta de acceso `typeProperties` están parametrizadas con sus valores predeterminados correspondientes. Por ejemplo, hay dos propiedades en las propiedades de tipo **IntegrationRuntimes**: `computeProperties` y `ssisProperties`. Ambos tipos de propiedades se crean con sus valores predeterminados y tipos (objeto) respectivos.

#### <a name="triggers"></a>Desencadenadores

* En `typeProperties`, hay dos propiedades parametrizadas. La primera de ellas es `maxConcurrency`, que tiene especificado un valor predeterminado y es de tipo `string`. Tiene el nombre de parámetro predeterminado de `<entityName>_properties_typeProperties_maxConcurrency`.
* La propiedad `recurrence` también está parametrizada. En ella, se especifica que todas las propiedades de ese nivel están parametrizadas como cadenas, con valores predeterminados y los nombres de parámetro. Una excepción es la propiedad `interval`, que está parametrizada como tipo de número y con el nombre de parámetro con el sufijo `<entityName>_properties_typeProperties_recurrence_triggerSuffix`. De forma similar, la propiedad `freq` es una cadena y está parametrizada como una cadena. Sin embargo, la propiedad `freq` está parametrizada sin un valor predeterminado. El nombre está abreviado y con un sufijo. Por ejemplo, `<entityName>_freq`.

#### <a name="linkedservices"></a>LinkedServices

* Los servicios vinculados son únicos. Dado que los servicios vinculados y los conjuntos de datos tienen una gran variedad de tipos, puede proporcionar una personalización específica de tipo. En este ejemplo, para todos los servicios vinculados de tipo `AzureDataLakeStore` se aplicará una plantilla específica y para todos los demás (a través de \*) se aplicará otra plantilla.
* La propiedad `connectionString` se parametrizará como un valor `securestring`, no tendrá un valor predeterminado y tendrá un nombre de parámetro abreviado con el sufijo `connectionString`.
* La propiedad `secretAccessKey` resulta ser `AzureKeyVaultSecret` (por ejemplo, en un servicio vinculado `AmazonS3`). Se parametriza automáticamente como un secreto de Azure Key Vault y se captura desde el almacén de claves configurado. También puede parametrizar el propio almacén de claves.

#### <a name="datasets"></a>Conjuntos de datos

* Aunque la personalización específica de tipo está disponible para conjuntos de datos, se puede proporcionar configuración sin necesidad de una configuración de nivel \* explícita. En el ejemplo anterior, todas las propiedades del conjunto de datos de `typeProperties` están parametrizadas.

### <a name="default-parameterization-template"></a>Plantilla de parametrización predeterminada

A continuación se muestra la plantilla de parametrización predeterminada actual. Si solo necesita agregar uno o varios parámetros, puede resultar útil editarla directamente, ya que no perderá la estructura de parametrización existente.

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

A continuación se muestra un ejemplo de cómo agregar un único valor a la plantilla de parametrización predeterminada. Solamente queremos agregar un identificador de clúster Databricks interactivo para un servicio vinculado de Databricks al archivo de parámetros. Tenga en cuenta que el archivo siguiente es el mismo que el archivo anterior, excepto para `existingClusterId` incluido en el campo propiedades de `Microsoft.DataFactory/factories/linkedServices`.

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

Si ha configurado la integración e implementación continuas (CI/CD) para las factorías de datos, puede que se encuentre con los límites de la plantilla de Azure Resource Manager a medida que la fábrica crece más. Un ejemplo de límite es el número máximo de recursos en una plantilla de Resource Manager. Para acomodar factorías más grandes, junto con la generación de la plantilla completa de Resource Manager para una factoría, ahora Data Factory genera plantillas vinculadas de Resource Manager. Con esta característica, la carga de la factoría completa se divide en varios archivos, por lo que no se incurre en los límites.

Si tiene Git configurado, se generan las plantillas vinculadas y se guardan junto con las plantillas completas de Resource Manager en la rama `adf_publish` bajo una nueva carpeta denominada `linkedTemplates`.

![Carpeta de plantillas vinculadas de Resource Manager](media/continuous-integration-deployment/linked-resource-manager-templates.png)

Normalmente las plantillas vinculadas de Resource Manager tienen una plantilla principal y un conjunto de plantillas secundarias vinculadas a la principal. La plantilla principal se denomina `ArmTemplate_master.json` y las plantillas secundarias se denominan con el patrón `ArmTemplate_0.json`, `ArmTemplate_1.json`, y así sucesivamente. Para usar plantillas vinculadas en lugar de la plantilla de Resource Manager completa, actualice la tarea de CI/CD para que apunte a `ArmTemplate_master.json` en lugar de apuntar a `ArmTemplateForFactory.json` (la plantilla de Resource Manager completa). Resource Manager también requiere la carga de las plantillas vinculadas en una cuenta de almacenamiento para que Azure pueda obtener acceso a ellas durante la implementación. Para obtener más información, consulte [Deploying Linked ARM Templates with VSTS](https://blogs.msdn.microsoft.com/najib/2018/04/22/deploying-linked-arm-templates-with-vsts/) (Implementación de plantillas vinculadas de ARM con VSTS).

No se olvide de agregar los scripts de Data Factory en la canalización de CI/CD antes y después de la tarea de implementación.

Si no tiene GIT configurado, las plantillas vinculadas son accesibles a través del gesto **Exportar plantilla de ARM**.

## <a name="hot-fix-production-branch"></a>Rama de producción de revisión

Si implementa una factoría para la producción y se da cuenta de que hay un error que debe corregirse de inmediato, pero no puede implementar la rama de colaboración actual, es posible que deba implementar una revisión.

1.  En Azure DevOps, vaya a la versión que se implementó en producción y busque la última confirmación que se implementó.

2.  En el mensaje de confirmación, obtenga el identificador de confirmación de la rama de colaboración.

3.  Cree una nueva rama de revisión a partir de esa confirmación.

4.  Vaya a experiencia de la interfaz de usuario de Azure Data Factory y cambie a esta rama.

5.  Mediante la experiencia de la interfaz de usuario de Azure Data Factory, corrija el error. Guarde los cambios.

6.  Una vez comprobada la corrección, haga clic en **Export ARM template** (Exportar plantilla de ARM) para obtener la plantilla de Resource Manager de revisión.

7.  Proteja manualmente esta compilación en la rama adf_publish.

8.  Si ha configurado la canalización de versión para que se desencadene automáticamente en función de las protecciones de adf_publish, se iniciará automáticamente una nueva versión. De lo contrario, ponga en cola manualmente una versión.

9.  Implemente la versión de revisión en las factorías de prueba y producción. Esta versión contiene la carga de producción anterior más la corrección realizada en el paso 5.

10. Agregue los cambios de la revisión a la rama de desarrollo para que las versiones posteriores no se encuentren con el mismo error.

## <a name="best-practices-for-cicd"></a>Procedimientos recomendados para CI/CD

Si usa la integración de Git con la factoría de datos y tiene una canalización de CI/CD que mueve los cambios desde el entorno de desarrollo al entorno de prueba y, luego, a producción, los procedimientos recomendados son los siguientes:

-   **Integración de Git**. Solo se necesita configurar la factoría de datos de desarrollo con la integración de Git. Los cambios en los entorno de prueba y producción se implementan a través de CI/CD y no se necesita la integración Git.

-   **Script de CI/CD de Data Factory**. Antes del paso de implementación de Resource Manager en CI/CD, es necesario realizar ciertas tareas, como la detención o el inicio de desencadenadores y la limpieza. Se recomienda usar scripts de PowerShell antes y después de la implementación. Para más información, vea [Actualización de desencadenadores activos](#update-active-triggers). 

-   **Entornos de ejecución de integración y uso compartido**. Las instancias de Integration Runtime no cambian a menudo y son similares en todas las fases de CI/CD. Como resultado, Data Factory espera que el usuario tenga el mismo nombre y tipo que los entornos de ejecución de integración en todas las etapas de CI/CD. Si desea compartir las instancias de Integration Runtime en todas las fases, considere la posibilidad de usar una factoría ternaria solo para contener las instancias de Integration Runtime compartidas. Puede usar esta factoría compartida en todos los entornos como un tipo de entorno de ejecución de integración vinculado.

-   **Key Vault**. Cuando usa los servicios vinculados basados en Azure Key Vault, puede aprovecharlos todavía más si mantiene almacenes de claves independientes para diferentes entornos. También puede configurar niveles de permisos independientes para cada uno de ellos. Por ejemplo, es posible que no quiera que los miembros del equipo tengan permisos para ver los secretos de producción. Si sigue este enfoque, le recomendamos que mantenga los mismos nombres de secreto en todas las fases. Si conserva los mismos nombres, no es necesario cambiar las plantillas de Resource Manager a través de los entornos de CI/CD, porque lo único cambia es el nombre del almacén de claves, que es uno de los parámetros de la plantilla de Resource Manager.

## <a name="unsupported-features"></a>Características no admitidas

-   No se pueden publicar recursos individuales. Las entidades de factoría de datos dependen unas de otras y el seguimiento de las dependencias puede ser difícil y provocar un comportamiento inesperado. Por ejemplo, los desencadenadores dependen de las canalizaciones, las canalizaciones dependen de los conjuntos de datos y otras canalizaciones, y así sucesivamente. Si fuera posible publicar solo un subconjunto del conjunto de cambios completo, podrían producirse determinados errores imprevistos.

-   No es posible publicar desde ramas privadas.

-   No es posible hospedar proyectos en Bitbucket.
