---
title: Actualizar modelos de Azure Analysis Services con Azure Automation | Microsoft Docs
description: Obtenga información sobre cómo codificar las actualizaciones del modelo mediante el uso de Azure Automation.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 4cae93cff594ad561973f8029ea7335dc4c60263
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66356997"
---
# <a name="refresh-with-azure-automation"></a>Actualización con Azure Automation

Mediante el uso de Azure Automation y Runbooks de PowerShell, puede realizar operaciones de actualización de datos automatizada en los modelos tabulares de análisis de Azure.  

El ejemplo de este artículo usa la [módulos de PowerShell SqlServer](https://docs.microsoft.com/powershell/module/sqlserver/?view=sqlserver-ps).

Un Runbook de PowerShell, que muestra cómo actualizar un modelo de ejemplo se proporciona más adelante en este artículo.  

## <a name="authentication"></a>Authentication

Todas las llamadas deben autenticarse con un token válido de Azure Active Directory (OAuth 2).  El ejemplo de este artículo usará un Principal de servicio (SPN) para autenticarse en Azure Analysis Services.

Para más información acerca de cómo crear una entidad de servicio, consulte [crear una entidad de servicio mediante Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="prerequisites"></a>Requisitos previos

> [!IMPORTANT]
> En el siguiente ejemplo se da por supuesto que el firewall de Azure Analysis Services está deshabilitado. Si el firewall está habilitado, la dirección IP pública del iniciador solicitud deberá estar en la lista blanca del firewall.

### <a name="install-sqlserver-modules-from-powershell-gallery"></a>Instalar módulos de SQL Server desde la Galería de PowerShell.

1. En su cuenta de Azure Automation, haga clic en **módulos**, a continuación, **Examinar Galería**.

2. En la barra de búsqueda, busque **SqlServer**.

    ![Buscar módulos](./media/analysis-services-refresh-azure-automation/1.png)

3. Seleccione SQL Server, a continuación, haga clic en **importación**.
 
    ![Módulo de importación](./media/analysis-services-refresh-azure-automation/2.png)

4. Haga clic en **OK**.
 
### <a name="create-a-service-principal-spn"></a>Crear a una entidad de servicio (SPN)

Para obtener información acerca de cómo crear una entidad de servicio, consulte [crear una entidad de servicio mediante Azure portal](../active-directory/develop/howto-create-service-principal-portal.md).

### <a name="configure-permissions-in-azure-analysis-services"></a>Configurar permisos en Azure Analysis Services
 
La entidad de servicio que cree debe tener permisos de administrador del servidor en el servidor. Para más información, vea [Incorporación de una entidad de servicio al rol de administrador del servidor](analysis-services-addservprinc-admins.md).

## <a name="design-the-azure-automation-runbook"></a>Diseño del Runbook de Azure Automation

1. En la cuenta de Automation, cree un **credenciales** recursos que se usará para almacenar de forma segura la entidad de servicio.

    ![crear credencial](./media/analysis-services-refresh-azure-automation/6.png)

2. Escriba los detalles de la credencial.  Para el **nombre de usuario**, escriba el **SPN ClientId**, para el **contraseña**, escriba el **SPN secreto**.

    ![crear credencial](./media/analysis-services-refresh-azure-automation/7.png)

3. Importar el Runbook de Automation

    ![Importación del runbook](./media/analysis-services-refresh-azure-automation/8.png)

4. Busque el **actualización Model.ps1** de archivos, proporcione un **nombre** y **descripción**y, a continuación, haga clic en **crear**.

    ![Importación del runbook](./media/analysis-services-refresh-azure-automation/9.png)

5. Cuando se ha creado el Runbook, pasará automáticamente al modo de edición.  Seleccione **Publicar**.

    ![Publicar runbook](./media/analysis-services-refresh-azure-automation/10.png)

    > [!NOTE]
    > El recurso de credencial que se creó anteriormente se recupera el runbook mediante el **Get-AutomationPSCredential** comando.  Este comando, a continuación, se pasa a la **Invoke ProcessASADatabase** comando de PowerShell para realizar la autenticación a Azure Analysis Services.

6. Probar el runbook, haga clic en **iniciar**.

    ![Iniciar el Runbook](./media/analysis-services-refresh-azure-automation/11.png)

7. Rellene el **DATABASENAME**, **ANALYSISSERVER**, y **REFRESHTYPE** parámetros y, a continuación, haga clic en **Aceptar**. El **WEBHOOKDATA** parámetro no es necesario cuando se ejecuta manualmente el Runbook.

    ![Iniciar el Runbook](./media/analysis-services-refresh-azure-automation/12.png)

Si el Runbook se ha ejecutado correctamente, recibirá una salida similar al siguiente:

![Ejecución correcta](./media/analysis-services-refresh-azure-automation/13.png)

## <a name="use-a-self-contained-azure-automation-runbook"></a>Usar un Runbook de automatización de Azure independientes

El Runbook puede configurarse para actualizar el modelo de Azure Analysis Services de forma programada del desencadenador.

Esto se puede configurar como sigue:

1. En el Runbook de automatización, haga clic en **programaciones**, a continuación, **agregar una programación**.
 
    ![Crear programación](./media/analysis-services-refresh-azure-automation/14.png)

2. Haga clic en **programación** > **crear una nueva programación**y, a continuación, rellene los detalles.

    ![Configurar programación](./media/analysis-services-refresh-azure-automation/15.png)

3. Haga clic en **Create**(Crear).

4. Rellene los parámetros para la programación. Se utilizarán cada vez que se desencadena el Runbook. El **WEBHOOKDATA** parámetro debe dejarse en blanco cuando se ejecuta a través de una programación.

    ![Configuración de parámetros](./media/analysis-services-refresh-azure-automation/16.png)

5. Haga clic en **OK**.

## <a name="consume-with-data-factory"></a>Consumir con Data Factory

Para consumir el runbook mediante Azure Data Factory, cree primero un **Webhook** para el runbook. El **Webhook** proporcionará una dirección URL que se puede llamar a través de una actividad web de Azure Data Factory.

> [!IMPORTANT]
> Para crear un **Webhook**, el estado del Runbook debe ser **publicada**.

1. En el Runbook de Automation, haga clic en **Webhooks**y, a continuación, haga clic en **agregar Webhook**.

   ![Agregar Webhook](./media/analysis-services-refresh-azure-automation/17.png)

2. Asigne el Webhook un nombre y una fecha de expiración.  El nombre solo identifica el Webhook dentro del Runbook de Automation, no forma parte de la dirección URL.

   >[!CAUTION]
   >Asegúrese de que copiar la dirección URL antes de cerrar al asistente como que no puede recuperarla una vez cerrado.
    
   ![Configuración de Webhook](./media/analysis-services-refresh-azure-automation/18.png)

    Los parámetros del webhook pueden permanecer en blanco.  Al configurar la actividad web de Azure Data Factory, los parámetros se pueden pasar en el cuerpo de la llamada web.

3. En Data Factory, configure un **actividad web**

### <a name="example"></a>Ejemplo

   ![Actividad Web de ejemplo](./media/analysis-services-refresh-azure-automation/19.png)

El **URL** es la dirección URL creada a partir del Webhook.

El **cuerpo** es un documento JSON que debe contener las siguientes propiedades:


|Propiedad  |Valor  |
|---------|---------|
|**AnalysisServicesDatabase**     |El nombre de la base de datos de Azure Analysis Services <br/> Ejemplo: AdventureWorksDB         |
|**AnalysisServicesServer**     |El nombre del servidor de Azure Analysis Services. <br/> Ejemplo: https:\//westus.asazure.windows.net/servers/myserver/models/AdventureWorks/         |
|**DatabaseRefreshType**     |El tipo de actualización para realizar. <br/> Ejemplo: Completo         |

Cuerpo de JSON de ejemplo:

```json
{
    "AnalysisServicesDatabaseName": "AdventureWorksDB",
    "AnalysisServicesServer": "asazure://westeurope.asazure.windows.net/MyAnalysisServer",
    "DatabaseRefreshType": "Full"
}
```

Estos parámetros se definen en el script de PowerShell de runbook.  Cuando se ejecuta la actividad web, la carga de JSON que se pasa es WEBHOOKDATA.

Esto se deserializa y se almacenan como parámetros de PowerShell, que son utilizados por el comando de PowerShell de Invoke-ProcesASDatabase.

![Deserializado Webhook](./media/analysis-services-refresh-azure-automation/20.png)

## <a name="use-a-hybrid-worker-with-azure-analysis-services"></a>Usar Hybrid Worker con Azure Analysis Services

Una máquina Virtual de Azure con una dirección IP pública estática puede usarse como un Hybrid Worker de Azure Automation.  Esta dirección IP pública, a continuación, se puede agregar al firewall de Azure Analysis Services.

> [!IMPORTANT]
> Asegúrese de que la dirección IP pública de máquina Virtual está configurada como estática.
>
>Para más información acerca de cómo configurar Azure Automation Hybrid Worker, consulte [automatización de recursos en su centro de datos o en la nube con Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md#install-a-hybrid-runbook-worker).

Una vez que se configura un Hybrid Worker, crear un Webhook como se describe en la sección [Consume con Data Factory](#consume-with-data-factory).  La única diferencia consiste en Seleccionar el **ejecutar en** > **Hybrid Worker** opción al configurar el Webhook.

Webhook de ejemplo con Hybrid Worker:

![Webhook de ejemplo Hybrid Worker](./media/analysis-services-refresh-azure-automation/21.png)

## <a name="sample-powershell-runbook"></a>Runbook de PowerShell de ejemplo

El fragmento de código siguiente es un ejemplo de cómo realizar la actualización del modelo de Azure Analysis Services mediante un Runbook de PowerShell.

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData,

    [Parameter (Mandatory = $false)]
    [String] $DatabaseName,
    [Parameter (Mandatory = $false)]
    [String] $AnalysisServer,
    [Parameter (Mandatory = $false)]
    [String] $RefreshType
)

$_Credential = Get-AutomationPSCredential -Name "ServicePrincipal"

# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData)
{ 
    # Retrieve AAS details from Webhook request body
    $atmParameters = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)
    Write-Output "CredentialName: $($atmParameters.CredentialName)"
    Write-Output "AnalysisServicesDatabaseName: $($atmParameters.AnalysisServicesDatabaseName)"
    Write-Output "AnalysisServicesServer: $($atmParameters.AnalysisServicesServer)"
    Write-Output "DatabaseRefreshType: $($atmParameters.DatabaseRefreshType)"
    
    $_databaseName = $atmParameters.AnalysisServicesDatabaseName
    $_analysisServer = $atmParameters.AnalysisServicesServer
    $_refreshType = $atmParameters.DatabaseRefreshType
 
    Invoke-ProcessASDatabase -DatabaseName $_databaseName -RefreshType $_refreshType -Server $_analysisServer -ServicePrincipal -Credential $_credential
}
else 
{
    Invoke-ProcessASDatabase -DatabaseName $DatabaseName -RefreshType $RefreshType -Server $AnalysisServer -ServicePrincipal -Credential $_Credential
}
```


## <a name="next-steps"></a>Pasos siguientes

[Muestras](analysis-services-samples.md)  
[API DE REST](https://docs.microsoft.com/rest/api/analysisservices/servers)
