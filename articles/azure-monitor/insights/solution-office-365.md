---
title: Solución de administración de Office 365 en Azure | Microsoft Docs
description: Este artículo proporciona detalles sobre la configuración y el uso de la solución Office 365 en Azure.  Incluye una descripción detallada de los registros de Office 365 creados en Azure Monitor.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/24/2019
ms.author: bwren
ms.openlocfilehash: da9e322f74433df7066ec574db7a49123f96d76b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "66130800"
---
# <a name="office-365-management-solution-in-azure-preview"></a>Solución de administración de Office 365 en Azure (versión preliminar)

![Logotipo de Office 365](media/solution-office-365/icon.png)

La solución de administración de Office 365 permite supervisar el entorno de Office 365 en Azure Monitor.

- Supervise las actividades de usuario en las cuentas de Office 365 para analizar patrones de uso e identificación de tendencias de comportamiento. Por ejemplo, puede extraer escenarios de uso específicos, como los archivos que se comparten fuera de la organización o los sitios de SharePoint más populares.
- Supervise las actividades del administrador para realizar el seguimiento de cambios de configuración u operaciones de privilegios elevados.
- Detecte e investigue comportamientos de usuario no deseados, que puede personalizar para las necesidades de la organización.
- Demuestre el cumplimiento de las normas y las auditorías. Por ejemplo, puede supervisar las operaciones de acceso a archivos en los archivos confidenciales, lo que pueden ayudarle con el proceso de cumplimiento y auditoría.
- Solucione problemas operativos mediante [consultas de registros](../log-query/log-query-overview.md) en los datos de actividad de Office 365 de su organización.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Se requiere lo siguiente antes de la instalación y configuración de esta solución.

- Suscripción organizativa de Office 365.
- Credenciales de una cuenta de usuario que sea un administrador global.
- Para recibir datos de auditoría, primero debe [configurar la auditoría](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=en-US&rs=en-US&ad=US#PickTab=Before_you_begin) en su suscripción de Office 365.  Tenga en cuenta que la [auditoría de los buzones](https://technet.microsoft.com/library/dn879651.aspx) se configura por separado.  Puede instalar la solución y recopilar otros datos aunque no se configure la auditoría.
 

## <a name="management-packs"></a>Módulos de administración

Esta solución no instala ningún módulo de administración en [grupos de administración conectados](../platform/om-agents.md).
  

## <a name="install-and-configure"></a>Instalar y configurar

Empiece por agregar la [solución Office 365 a su suscripción](solutions.md#install-a-monitoring-solution). Una vez agregada, debe realizar los pasos de configuración de esta sección para proporcionar acceso a la suscripción a Office 365.

### <a name="required-information"></a>Información necesaria

Antes de iniciar este procedimiento, recopile la siguiente información.

Desde el área de trabajo de Log Analytics:

- Nombre del área de trabajo: área de trabajo donde se recopilarán los datos de Office 365.
- Nombre del grupo de recursos: el grupo de recursos que contiene el área de trabajo.
- Identificador de suscripción de Azure: la suscripción que contiene el área de trabajo.

Desde la suscripción a Office 365:

- Nombre de usuario: dirección de correo electrónico de una cuenta administrativa.
- Identificador de inquilino: identificador único de la suscripción a Office 365.
- Identificador de cliente: cadena de 16 caracteres que representa el cliente de Office 365.
- Secreto de cliente: cadena cifrada necesaria para la autenticación.

### <a name="create-an-office-365-application-in-azure-active-directory"></a>Creación de una aplicación de Office 365 en Azure Active Directory

El primer paso es crear una aplicación en Azure Active Directory que la solución de administración usará para acceder a su solución de Office 365.

1. Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com/).
1. Seleccione **Azure Active Directory** y **Registros de aplicaciones**.
1. Haga clic en **Nuevo registro de aplicaciones**.

    ![Adición de un registro de aplicaciones](media/solution-office-365/add-app-registration.png)
1. Escriba valores de **Nombre** y **Dirección URL de inicio de sesión** de la aplicación.  El nombre debe ser descriptivo.  Use `http://localhost` para la dirección URL y mantener _aplicación Web / API_ para el **tipo de aplicación**
    
    ![Crear aplicación](media/solution-office-365/create-application.png)
1. Haga clic en **Crear** y valide la información de la aplicación.

    ![Aplicación registrada](media/solution-office-365/registered-app.png)

### <a name="configure-application-for-office-365"></a>Configuración de la aplicación para Office 365

1. Haga clic en **Configuración** para abrir el menú **Configuración**.
1. Seleccione **Propiedades**. Cambie el valor de **Multiinquilino** a _Sí_.

    ![Configuración de varios inquilinos](media/solution-office-365/settings-multitenant.png)

1. En el menú **Configuración**, seleccione **Permisos necesarios** y haga clic en **Agregar**.
1. Haga clic en **Seleccionar una API** y, a continuación, en **Office 365 Management APIs** (API de administración de Office 365). Haga clic en **Office 365 Management APIs** (API de administración de Office 365). Haga clic en **Seleccionar**.

    ![Seleccionar API](media/solution-office-365/select-api.png)

1. En **Seleccionar permisos**, seleccione las opciones siguientes de **Permisos de la aplicación** y **Permisos delegados**:
   - Leer la información sobre el estado del servicio de su organización
   - Leer datos de actividad de la organización
   - Leer informes de actividad de su organización

     ![Seleccionar API](media/solution-office-365/select-permissions.png)

1. Haga clic en **Seleccionar** y en **Listo**.
1. Haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí** cuando se le solicite la comprobación.

    ![Conceder permisos](media/solution-office-365/grant-permissions.png)

### <a name="add-a-key-for-the-application"></a>Adición de una clave para la aplicación

1. En el menú **Configuración**, seleccione **Claves**.
1. Especifique los valores de **Descripción** y **Duración** de la nueva clave.
1. Haga clic en **Guardar** y, a continuación, copie el **valor** se generó.

    ![Claves](media/solution-office-365/keys.png)

### <a name="add-admin-consent"></a>Adición del consentimiento de administrador

Para habilitar la cuenta administrativa por primera vez, debe proporcionar el consentimiento de administrador para la aplicación. Puede hacerlo con un script de PowerShell. 

1. Guarde el script siguiente como *office365_consent.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,     
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId
    )
    
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $WorkspaceLocation= $Workspace.Location
    $WorkspaceLocation
    
    Function AdminConsent{
    
    $domain='login.microsoftonline.com'
    switch ($WorkspaceLocation.Replace(" ","").ToLower()) {
           "eastus"   {$OfficeAppClientId="d7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westeurope"   {$OfficeAppClientId="c9005da2-023d-40f1-a17a-2b7d91af4ede"; break}
           "southeastasia"   {$OfficeAppClientId="09c5b521-648d-4e29-81ff-7f3a71b27270"; break}
           "australiasoutheast"  {$OfficeAppClientId="f553e464-612b-480f-adb9-14fd8b6cbff8"; break}   
           "westcentralus"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break}
           "japaneast"   {$OfficeAppClientId="b07d97d3-731b-4247-93d1-755b5dae91cb"; break}
           "uksouth"   {$OfficeAppClientId="f232cf9b-e7a9-4ebb-a143-be00850cd22a"; break}
           "centralindia"   {$OfficeAppClientId="ffbd6cf4-cba8-4bea-8b08-4fb5ee2a60bd"; break}
           "canadacentral"  {$OfficeAppClientId="c2d686db-f759-43c9-ade5-9d7aeec19455"; break}
           "eastus2"  {$OfficeAppClientId="7eb65b0-8167-4b5d-b371-719a2e5e30cc"; break}
           "westus2"  {$OfficeAppClientId="98a2a546-84b4-49c0-88b8-11b011dc8c4e"; break} #Need to check
           "usgovvirginia" {$OfficeAppClientId="c8b41a87-f8c5-4d10-98a4-f8c11c3933fe"; 
                             $domain='login.microsoftonline.us'; break}
           default {$OfficeAppClientId="55b65fb5-b825-43b5-8972-c8b6875867c1";
                    $domain='login.windows-ppe.net'; break} #Int
        }
    
        $domain
        Start-Process -FilePath  "https://$($domain)/common/adminconsent?client_id=$($OfficeAppClientId)&state=12345"
    }
    
    AdminConsent -ErrorAction Stop
    ```

2. Ejecute el siguiente comando para ejecutar el script. Se le pedirán las credenciales dos veces. Primero, proporcione las credenciales del área de trabajo de Log Analytics y, luego, las credenciales de administrador global de su inquilino de Office 365.

    ```
    .\office365_consent.ps1 -WorkspaceName <Workspace name> -ResourceGroupName <Resource group name> -SubscriptionId <Subscription ID>
    ```

    Ejemplo:

    ```
    .\office365_consent.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631- yyyyyyyyyyyy'
    ```

1. Aparecerá una ventana similar a la siguiente. Haga clic en **Aceptar**.
    
    ![Consentimiento del administrador](media/solution-office-365/admin-consent.png)

### <a name="subscribe-to-log-analytics-workspace"></a>Suscripción al área de trabajo de Log Analytics

El último paso es suscribirse la aplicación al área de trabajo de Log Analytics. También puede hacerlo con un script de PowerShell.

1. Guarde el script siguiente como *office365_subscription.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeUsername,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId,
        [Parameter(Mandatory=$True)][string]$OfficeClientId,
        [Parameter(Mandatory=$True)][string]$OfficeClientSecret
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    $OfficeClientSecret =[uri]::EscapeDataString($OfficeClientSecret)
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant = $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Failure {
    $line
    $formatstring = "{0} : {1}`n{2}`n" +
                    "    + CategoryInfo          : {3}`n" +
                    "    + FullyQualifiedErrorId : {4}`n"
    $fields = $_.InvocationInfo.MyCommand.Name,
              $_.ErrorDetails.Message,
              $_.InvocationInfo.PositionMessage,
              $_.CategoryInfo.ToString(),
              $_.FullyQualifiedErrorId
    
    $formatstring -f $fields
    $_.Exception.Response
    
    $line
    break
    }
    
    Function Connection-API
    {
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    
    $line
    $connectionAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/connections/office365connection_' + $SubscriptionId + $OfficeTennantId + '?api-version=2017-04-26-preview'
    $connectionAPIUrl
    $line
    
    $xms_client_tenant_Id ='1da8f770-27f4-4351-8cb3-43ee54f14759'
    
    $BodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'clientId': '" + $OfficeClientId + "',
                                    'clientSecret': '" + $OfficeClientSecret + "',
                                    'Username': '" + $OfficeUsername   + "',
                                    'Url': 'https://$($domain)/" + $OfficeTennantId + "/oauth2/token',
                                  },
                    'etag': '*',
                    'kind': 'Connection',
                    'solution': 'Connection',
                   }"
    
    $params = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id #Prod-'1da8f770-27f4-4351-8cb3-43ee54f14759'
        'Content-Type' = 'application/json'
        }
        Body = $BodyString
        Method = 'Put'
        URI = $connectionAPIUrl
    }
    $response = Invoke-WebRequest @params 
    $response
    $line
    
    }
    
    Function Office-Subscribe-Call{
    try{
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $SubscriptionId   =  $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_' + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $OfficeBodyString = "{
                    'properties': {
                                    'AuthProvider':'Office365',
                                    'office365TenantID': '" + $OfficeTennantId + "',
                                    'connectionID': 'office365connection_" + $SubscriptionId + $OfficeTennantId + "',
                                    'office365AdminUsername': '" + $OfficeUsername + "',
                                    'contentTypes':'Audit.Exchange,Audit.AzureActiveDirectory,Audit.SharePoint'
                                  },
                    'etag': '*',
                    'kind': 'Office365',
                    'solution': 'Office365',
                   }"
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Body = $OfficeBodyString
        Method = 'Put'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    }
    catch{ Failure }
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Connection-API -ErrorAction Stop
    Office-Subscribe-Call -ErrorAction Stop
    ```

2. Ejecute el siguiente comando para ejecutar el script:

    ```
    .\office365_subscription.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeUsername <OfficeUsername> -OfficeTennantID <Tenant ID> -OfficeClientId <Client ID> -OfficeClientSecret <Client secret>
    ```

    Ejemplo:

    ```powershell
    .\office365_subscription.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeUsername 'admin@contoso.com' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx' -OfficeClientId 'f8f14c50-5438-4c51-8956-zzzzzzzzzzzz' -OfficeClientSecret 'y5Lrwthu6n5QgLOWlqhvKqtVUZXX0exrA2KRHmtHgQb='
    ```

### <a name="troubleshooting"></a>solución de problemas

Puede que vea el siguiente error si la aplicación ya está suscrita a esta área de trabajo o si este inquilino está suscrito a otra área de trabajo.

```Output
Invoke-WebRequest : {"Message":"An error has occurred."}
At C:\Users\v-tanmah\Desktop\ps scripts\office365_subscription.ps1:161 char:19
+ $officeresponse = Invoke-WebRequest @Officeparams
+                   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (System.Net.HttpWebRequest:HttpWebRequest) [Invoke-WebRequest], WebException
    + FullyQualifiedErrorId : WebCmdletWebResponseException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand 
```

Es posible que se muestre el error siguiente si se proporcionan valores de parámetros no válidos.

```Output
Select-AzSubscription : Please provide a valid tenant or a valid subscription.
At line:12 char:18
+ ... cription = (Select-AzSubscription -SubscriptionId $($Subscriptio ...
+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzContext], ArgumentException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.Profile.SetAzContextCommand

```

## <a name="uninstall"></a>Desinstalar

Puede quitar la solución de administración de Office 365 mediante el proceso de [Quitar una solución de administración](solutions.md#remove-a-monitoring-solution). Esta acción no detendrá la recopilación de datos de Office 365 en Azure Monitor. Realice el procedimiento siguiente para cancelar la suscripción a Office 365 y detener la recopilación de datos.

1. Guarde el script siguiente como *office365_unsubscribe.ps1*.

    ```powershell
    param (
        [Parameter(Mandatory=$True)][string]$WorkspaceName,
        [Parameter(Mandatory=$True)][string]$ResourceGroupName,
        [Parameter(Mandatory=$True)][string]$SubscriptionId,
        [Parameter(Mandatory=$True)][string]$OfficeTennantId
    )
    $line='#-------------------------------------------------------------------------------------------------------------------------------------------------------------------------'
    
    $line
    IF ($Subscription -eq $null)
        {Login-AzAccount -ErrorAction Stop}
    $Subscription = (Select-AzSubscription -SubscriptionId $($SubscriptionId) -ErrorAction Stop)
    $Subscription
    $option = [System.StringSplitOptions]::RemoveEmptyEntries 
    $Workspace = (Set-AzOperationalInsightsWorkspace -Name $($WorkspaceName) -ResourceGroupName $($ResourceGroupName) -ErrorAction Stop)
    $Workspace
    $WorkspaceLocation= $Workspace.Location
    
    # Client ID for Azure PowerShell
    $clientId = "1950a258-227b-4e31-a9cf-717495945fc2"
    # Set redirect URI for Azure PowerShell
    $redirectUri = "urn:ietf:wg:oauth:2.0:oob"
    $domain='login.microsoftonline.com'
    $adTenant =  $Subscription[0].Tenant.Id
    $authority = "https://login.windows.net/$adTenant";
    $ARMResource ="https://management.azure.com/";
    $xms_client_tenant_Id ='55b65fb5-b825-43b5-8972-c8b6875867c1'
    
    switch ($WorkspaceLocation) {
           "USGov Virginia" { 
                             $domain='login.microsoftonline.us';
                              $authority = "https://login.microsoftonline.us/$adTenant";
                              $ARMResource ="https://management.usgovcloudapi.net/"; break} # US Gov Virginia
           default {
                    $domain='login.microsoftonline.com'; 
                    $authority = "https://login.windows.net/$adTenant";
                    $ARMResource ="https://management.azure.com/";break} 
                    }
    
    Function RESTAPI-Auth { 
    
    $global:SubscriptionID = $Subscription.SubscriptionId
    # Set Resource URI to Azure Service Management API
    $resourceAppIdURIARM=$ARMResource;
    # Authenticate and Acquire Token 
    # Create Authentication Context tied to Azure AD Tenant
    $authContext = New-Object "Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext" -ArgumentList $authority
    # Acquire token
    $global:authResultARM = $authContext.AcquireToken($resourceAppIdURIARM, $clientId, $redirectUri, "Auto")
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $authHeader
    }
    
    Function Office-UnSubscribe-Call{
    
    #----------------------------------------------------------------------------------------------------------------------------------------------
    $authHeader = $global:authResultARM.CreateAuthorizationHeader()
    $ResourceName = "https://manage.office.com"
    $SubscriptionId   = $Subscription[0].Subscription.Id
    $OfficeAPIUrl = $ARMResource + 'subscriptions/' + $SubscriptionId + '/resourceGroups/' + $ResourceGroupName + '/providers/Microsoft.OperationalInsights/workspaces/' + $WorkspaceName + '/datasources/office365datasources_'  + $SubscriptionId + $OfficeTennantId + '?api-version=2015-11-01-preview'
    
    $Officeparams = @{
        ContentType = 'application/json'
        Headers = @{
        'Authorization'="$($authHeader)"
        'x-ms-client-tenant-id'=$xms_client_tenant_Id
        'Content-Type' = 'application/json'
        }
        Method = 'Delete'
        URI = $OfficeAPIUrl
      }
    
    $officeresponse = Invoke-WebRequest @Officeparams 
    $officeresponse
    
    }
    
    #GetDetails 
    RESTAPI-Auth -ErrorAction Stop
    Office-UnSubscribe-Call -ErrorAction Stop
    ```

2. Ejecute el siguiente comando para ejecutar el script:

    ```
    .\office365_unsubscribe.ps1 -WorkspaceName <Log Analytics workspace name> -ResourceGroupName <Resource Group name> -SubscriptionId <Subscription ID> -OfficeTennantID <Tenant ID> 
    ```

    Ejemplo:

    ```powershell
    .\office365_unsubscribe.ps1 -WorkspaceName MyWorkspace -ResourceGroupName MyResourceGroup -SubscriptionId '60b79d74-f4e4-4867-b631-yyyyyyyyyyyy' -OfficeTennantID 'ce4464f8-a172-4dcf-b675-xxxxxxxxxxxx'
    ```

## <a name="data-collection"></a>Colección de datos

### <a name="supported-agents"></a>Agentes admitidos

La solución de Office 365 no recupera los datos desde ninguno de los [agentes de Log Analytics](../platform/agent-data-sources.md).  Recupera los datos directamente desde Office 365.

### <a name="collection-frequency"></a>Frecuencia de recopilación

Inicialmente, la recopilación de datos puede tardar unas horas. Cuando comienza la recopilación, Office 365 envía una [notificación de webhook](https://msdn.microsoft.com/office-365/office-365-management-activity-api-reference#receiving-notifications) con datos detallados a Azure Monitor cada vez que se crea un registro. Este registro está disponible en Azure Monitor al cabo de unos minutos de su recepción.

## <a name="using-the-solution"></a>Uso de la solución

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

Al agregar la solución Office 365 al área de trabajo de Log Analytics, se agrega el icono **Office 365** al panel. Este icono muestra un recuento y una representación gráfica del número de equipos en el entorno y del cumplimiento de las actualizaciones.<br><br>
![Icono de resumen de Office 365](media/solution-office-365/tile.png)  

Haga clic en el icono de **Office 365** para abrir el panel de **Office 365**.

![Panel de Office 365](media/solution-office-365/dashboard.png)  

El panel incluye las columnas de la tabla siguiente. Cada columna muestra las diez principales alertas por recuento que coinciden con los criterios de esa columna para el ámbito e intervalo de tiempo especificados. Puede ejecutar una búsqueda de registros que proporcione toda la lista haciendo clic en Ver todo en la parte inferior de la columna o haciendo clic en el encabezado de columna.

| columna | DESCRIPCIÓN |
|:--|:--|
| Operaciones | Proporciona información acerca de los usuarios activos de todas las suscripciones de Office 365 supervisadas. También podrá ver el número de actividades que se producen con el tiempo.
| Exchange | Muestra el desglose de las actividades del servidor de Exchange, como Add-Mailbox Permission o Set-Mailbox. |
| SharePoint | Muestra las actividades principales que realizan los usuarios en documentos de SharePoint. Cuando obtiene los detalles de este icono, la página de búsqueda muestra los detalles de estas actividades, como el documento de destino y la ubicación de esta actividad. Por ejemplo, para un evento de acceso a archivos, podrá ver el documento al que se tiene acceso, el nombre de la cuenta asociada y la dirección IP. |
| Azure Active Directory | Incluye las actividades principales de los usuarios, como restablecer la contraseña de usuario y los intentos de inicio de sesión. Cuando se profundiza, podrá ver los detalles de estas actividades, como el estado del resultado. Esto es especialmente útil si desea supervisar las actividades sospechosas en Azure Active Directory. |




## <a name="azure-monitor-log-records"></a>Registros de Azure Monitor

El valor de **Tipo** es **OfficeActivity** para todos los registros creados en el área de trabajo de Log Analytics en Azure Monitor por la solución de Office 365.  La propiedad **OfficeWorkload** determina a qué servicio de Office 365 hace referencia el registro: Exchange, AzureActiveDirectory, SharePoint o OneDrive.  La propiedad **RecordType** especifica el tipo de operación.  Las propiedades varían para cada tipo de operación y se muestran en las tablas siguientes.

### <a name="common-properties"></a>Propiedades comunes

Las siguientes propiedades son comunes a todos los registros de Office 365.

| Propiedad | Descripción |
|:--- |:--- |
| Type | *OfficeActivity* |
| ClientIP | La dirección IP del dispositivo que se usó cuando se registró la actividad. La dirección IP se muestra en formato de dirección IPv4 o IPv6. |
| OfficeWorkload | Servicio de Office 365 al que hace referencia el registro.<br><br>AzureActiveDirectory<br>Exchange<br>SharePoint|
| Operación | El nombre de la actividad de usuario o administrador.  |
| OrganizationId | El identificador único GUID del inquilino de Office 365 de su organización. Este valor siempre será el mismo para su organización, con independencia del servicio de Office 365 en el que se produce. |
| RecordType | Tipo de operación realizada. |
| ResultStatus | Indica si la acción (especificada en la propiedad Operation) se realizó correctamente o no. Los valores posibles son Succeeded (correcta), PartiallySucceeded (parcialmente correcta) o Failed (con errores). Para la actividad de administración de Exchange, el valor es True o False. |
| UserId | El UPN (nombre principal de usuario) del usuario que realizó la acción que generó el registro, por ejemplo, my_name@my_domain_name. Tenga en cuenta que también se incluyen los registros de actividad realizada por cuentas del sistema (como SHAREPOINT\system o NTAUTHORITY\SYSTEM). | 
| UserKey | Un identificador alternativo para el usuario identificado en la propiedad UserId.  Por ejemplo, esta propiedad se rellena con el identificador único de Passport (PUID) para los eventos producidos por los usuarios de SharePoint, OneDrive para la empresa y Exchange. Esta propiedad también puede especificar el mismo valor que la propiedad UserID para los eventos que se producen en otros servicios y los eventos producidos por las cuentas del sistema|
| UserType | El tipo de usuario que realizó la operación.<br><br>Admin.<br>Application<br>DcAdmin<br>Normal<br>Reserved<br>ServicePrincipal<br>Sistema |


### <a name="azure-active-directory-base"></a>Azure Active Directory

Las siguientes propiedades son comunes a todos los registros de Azure Active Directory.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AzureActiveDirectory_EventType | El tipo de evento de Azure AD. |
| ExtendedProperties | Las propiedades extendidas del evento de Azure AD. |


### <a name="azure-active-directory-account-logon"></a>Inicio de sesión de cuenta de Azure Active Directory

Estos registros se crean cuando un usuario de Active Directory intenta iniciar sesión.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectoryAccountLogon |
| Application | La aplicación que desencadena el evento de inicio de sesión en la cuenta, como Office 15. |
| Cliente | Detalles sobre dispositivo, sistema operativo del dispositivo y explorador del dispositivo que usó el cliente para el evento de inicio de sesión en la cuenta. |
| LoginStatus | Esta propiedad viene directamente de OrgIdLogon.LoginStatus. Los algoritmos de alertas podrían realizar la asignación de distintos errores de inicio de sesión interesantes. |
| UserDomain | La información de identidad del inquilino (TII). | 


### <a name="azure-active-directory"></a>Azure Active Directory

Estos registros se crean cuando se realizan cambios o adiciones en objetos de Active Directory de Azure.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | AzureActiveDirectory |
| RecordType     | AzureActiveDirectory |
| AADTarget | El usuario sobre el que se realizó la acción (identificada por la propiedad Operation). |
| Actor | El usuario o la entidad de servicio que realizó la acción. |
| ActorContextId | El identificador GUID de la organización al que pertenece el actor. |
| ActorIpAddress | Dirección IP del actor en formato de dirección IPV4 o IPV6. |
| InterSystemsId | El identificador GUID que realiza el seguimiento de las acciones en los componentes del servicio Office 365. |
| IntraSystemId |   El identificador GUID generado por Azure Active Directory para realizar el seguimiento de la acción. |
| SupportTicketId | El identificador del vale de soporte técnico para la acción en situaciones de "actuar en nombre de". |
| TargetContextId | El identificador GUID de la organización a la que pertenece el usuario de destino. |


### <a name="data-center-security"></a>Data Center Security

Estos registros se crean a partir de los datos de auditoría de Data Center Security.  

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| EffectiveOrganization | El nombre del inquilino al que estaba destinada la elevación o el cmdlet. |
| ElevationApprovedTime | La marca de tiempo en el momento de la aprobación de la elevación. |
| ElevationApprover | El nombre de un administrador de Microsoft. |
| ElevationDuration | La duración en activo de la elevación. |
| ElevationRequestId |  Un identificador único para la solicitud de elevación. |
| ElevationRole | El rol para el que se solicitó la elevación. |
| ElevationTime | La hora de inicio de la elevación. |
| Start_Time | La hora de inicio de la ejecución del cmdlet. |


### <a name="exchange-admin"></a>Administración de Exchange

Estos registros se crean cuando se realizan cambios en la configuración de Exchange.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeAdmin |
| ExternalAccess |  Especifica si el cmdlet se ha ejecutado por un usuario de la organización, por personal del centro de datos de Microsoft o una cuenta de servicio del centro de datos o por un administrador delegado. El valor False indica que el cmdlet se ejecutó por alguien de su organización. El valor True indica que el cmdlet lo ejecutó personal del centro de datos, una cuenta de servicio del centro de datos o un administrador delegado. |
| ModifiedObjectResolvedName |  Este es el nombre descriptivo del objeto que ha sido modificado por el cmdlet. Esto se registra únicamente si el cmdlet modifica el objeto. |
| OrganizationName | El nombre del inquilino. |
| OriginatingServer | El nombre del servidor desde el que se ejecutó el cmdlet. |
| Parámetros | El nombre y valor de todos los parámetros que se utilizaron con el cmdlet que se identifica en la propiedad Operations. |


### <a name="exchange-mailbox"></a>Buzón de Exchange

Estos registros se crean cuando se realizan cambios o agregaciones en los buzones de Exchange.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| ClientInfoString | Información sobre el cliente de correo electrónico que se usó para realizar la operación, como la versión del explorador, la versión de Outlook y la información del dispositivo móvil. |
| Client_IPAddress | La dirección IP del dispositivo que se usó cuando se registró la operación. La dirección IP se muestra en formato de dirección IPv4 o IPv6. |
| ClientMachineName | El nombre del equipo que hospeda al cliente de Outlook. |
| ClientProcessName | El cliente de correo electrónico que se usó para tener acceso al buzón. |
| ClientVersion | La versión del cliente de correo electrónico. |
| InternalLogonType | Reservado para uso interno. |
| Logon_Type | Indica el tipo de usuario que tuvo acceso al buzón de correo y realizó la operación que se ha registrado. |
| LogonUserDisplayName |    El nombre descriptivo del usuario que realizó la operación. |
| LogonUserSid | El identificador SID del usuario que realizó la operación. |
| MailboxGuid | El identificador GUID de Exchange del buzón de correo al que se obtuvo acceso. |
| MailboxOwnerMasterAccountSid | Identificador SID de la cuenta maestra de la cuenta del propietario del buzón de correo. |
| MailboxOwnerSid | El identificador SID del propietario del buzón. |
| MailboxOwnerUPN | La dirección de correo electrónico de la persona que posee el buzón al que se obtuvo acceso. |


### <a name="exchange-mailbox-audit"></a>Auditoría de buzón de Exchange

Estos registros se crean cuando se crea una entrada de auditoría de buzones de correo.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | Exchange |
| RecordType     | ExchangeItem |
| Elemento | Representa el elemento en el que se realizó la operación | 
| SendAsUserMailboxGuid | El identificador GUID de Exchange del buzón de correo al que se obtuvo acceso para enviar correo. |
| SendAsUserSmtp | Dirección SMTP del usuario que se está suplantando. |
| SendonBehalfOfUserMailboxGuid | El identificador GUID de Exchange del buzón de correo al que se obtuvo acceso para enviar correo en su nombre. |
| SendOnBehalfOfUserSmtp | Dirección SMTP del usuario en cuyo nombre se envió el correo electrónico. |


### <a name="exchange-mailbox-audit-group"></a>Auditoría de grupos buzones de Exchange

Estos registros se crean cuando se realizan cambios o agregaciones en los grupos de Exchange.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | Exchange |
| OfficeWorkload | ExchangeItemGroup |
| AffectedItems | Información sobre cada elemento del grupo. |
| CrossMailboxOperations | Indica si estuvo implicado más de un buzón de correo en la operación. |
| DestMailboxId | Se establece únicamente si el parámetro CrossMailboxOperations es True. Especifica el identificador GUID del buzón de destino. |
| DestMailboxOwnerMasterAccountSid | Se establece únicamente si el parámetro CrossMailboxOperations es True. Especifica el identificador SID de la cuenta maestra del propietario del buzón de destino. |
| DestMailboxOwnerSid | Se establece únicamente si el parámetro CrossMailboxOperations es True. Especifica el identificador SID del buzón de correo de destino. |
| DestMailboxOwnerUPN | Se establece únicamente si el parámetro CrossMailboxOperations es True. Especifica el UPN del propietario del buzón de correo de destino. |
| DestFolder | La carpeta de destino, para operaciones como Move (mover). |
| Carpeta | La carpeta donde se encuentra un grupo de elementos. |
| Carpetas |     Información sobre las carpetas de origen implicadas en una operación; por ejemplo, si las carpetas son seleccionadas y, a continuación, se eliminan. |


### <a name="sharepoint-base"></a>SharePoint

Estas propiedades son comunes a todos los registros de SharePoint.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| EventSource | Identifica que se ha producido un evento en SharePoint. Los valores posibles son SharePoint y ObjectModel. |
| ItemType | El tipo de objeto al que se ha accedido o modificado. Consulte la tabla ItemType para obtener detalles sobre los tipos de objetos. |
| MachineDomainInfo | Información sobre las operaciones de sincronización del dispositivo. Esta información se notifica solo si está presente en la solicitud. |
| MachineId |   Información sobre las operaciones de sincronización del dispositivo. Esta información se notifica solo si está presente en la solicitud. |
| Site_ | El identificador GUID del sitio donde se encuentra el archivo o carpeta al que tuvo acceso el usuario. |
| Source_Name | La entidad que desencadenó la operación auditada. Los valores posibles son SharePoint y ObjectModel. |
| UserAgent | Información sobre el cliente o explorador del usuario. Esta información la proporciona el cliente o el explorador. |


### <a name="sharepoint-schema"></a>Esquema de SharePoint

Estos registros se crean cuando se realizan cambios en la configuración de SharePoint.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePoint |
| CustomEvent | Cadena opcional para eventos personalizados. |
| Event_Data |  Carga opcional para eventos personalizados. |
| ModifiedProperties | La propiedad se incluye para los eventos de administración, como agregar a un usuario como miembro de un sitio o un grupo de administración de la colección de sitios. La propiedad incluye el nombre de la propiedad que se ha modificado (por ejemplo, el grupo de administración del sitio), el nuevo valor de la propiedad modificada (como el usuario que se ha agregado como un administrador del sitio) y el valor anterior del objeto modificado. |


### <a name="sharepoint-file-operations"></a>Operaciones de archivos de SharePoint

Estos registros se crean en respuesta a las operaciones de archivos en SharePoint.

| Propiedad | DESCRIPCIÓN |
|:--- |:--- |
| OfficeWorkload | SharePoint |
| OfficeWorkload | SharePointFileOperation |
| DestinationFileExtension | La extensión de archivo de un archivo que se ha copiado o movido. Esta propiedad solo se muestra para los eventos FileCopied y FileMoved. |
| DestinationFileName | El nombre del archivo que se ha copiado o movido. Esta propiedad solo se muestra para los eventos FileCopied y FileMoved. |
| DestinationRelativeUrl | La dirección URL de la carpeta de destino donde se ha copiado o movido un archivo. La combinación de los valores de los parámetros SiteURL, DestinationRelativeURL y DestinationFileName es el valor de la propiedad ObjectID, que es el nombre de la ruta de acceso completa del archivo que se copió. Esta propiedad solo se muestra para los eventos FileCopied y FileMoved. |
| SharingType | El tipo de permisos que se asignaron al usuario con el que se ha compartido el recurso. Este usuario se identifica mediante el parámetro UserSharedWith. |
| Site_Url | La dirección URL del sitio donde se encuentra el archivo o carpeta al que tuvo acceso el usuario. |
| SourceFileExtension | La extensión de archivo del archivo al que tuvo acceso el usuario. Esta propiedad está en blanco si el objeto al que se tuvo acceso es una carpeta. |
| SourceFileName |  El nombre del archivo o carpeta al que tuvo acceso el usuario. |
| SourceRelativeUrl | La dirección URL de la carpeta que contiene el archivo al que tuvo acceso el usuario. La combinación de los valores de los parámetros SiteURL, SourceRelativeURL y SourceFileName es el valor de la propiedad ObjectID, que es el nombre de la ruta de acceso completa del archivo al que tuvo acceso el usuario. |
| UserSharedWith |  El usuario con el que se compartió un recurso. |




## <a name="sample-log-searches"></a>Búsquedas de registros de ejemplo

En la tabla siguiente se proporcionan ejemplos de búsquedas de registros para los registros de actualización recopilados por esta solución.

| Consultar | DESCRIPCIÓN |
| --- | --- |
|Recuento de todas las operaciones de la suscripción a Office 365 |OfficeActivity &#124; summarize count() by Operation |
|Uso de sitios de SharePoint|OfficeActivity &#124; where OfficeWorkload =~ "sharepoint" &#124; summarize count() by SiteUrl \| sort by Count asc|
|Operaciones de acceso a archivos por tipo de usuario|search in (OfficeActivity) OfficeWorkload =~ "azureactivedirectory" and "MyTest"|
|Búsqueda por una palabra clave específica|Type=OfficeActivity OfficeWorkload=azureactivedirectory "MyTest"|
|Supervisión de acciones externas en Exchange|OfficeActivity &#124; where OfficeWorkload =~ "exchange" and ExternalAccess == true|



## <a name="next-steps"></a>Pasos siguientes

* Use las [consultas de registros de Azure Monitor](../log-query/log-query-overview.md) para ver datos detallados sobre la actualización.
* [Cree sus propios paneles](../learn/tutorial-logs-dashboards.md) para mostrar las consultas de búsqueda favoritas de Office 365.
* [Cree alertas](../platform/alerts-overview.md) para recibir notificaciones proactivas de actividades importantes de Office 365.  