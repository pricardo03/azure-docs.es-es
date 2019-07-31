---
title: Ejemplos y definiciones de los registros de auditoría en Azure Active Directory B2C | Microsoft Docs
description: Orientación y ejemplos para acceder a los registros de auditoría de Azure AD B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 216f5413ce3dae1f2d040643a30a4d7db4a879b8
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835410"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Acceso a los registros de auditoría de Azure AD B2C

Azure Active Directory B2C (Azure AD B2C) emite registros de auditoría que contienen información sobre los recursos de B2C, los tokens emitidos y el acceso de administrador. Este artículo proporciona un breve resumen de la información disponible a través de los registros de auditoría e instrucciones sobre cómo acceder a estos datos del inquilino de Azure AD B2C.

> [!IMPORTANT]
> Los registros de auditoría solo se conservan durante siete días. Si necesita un periodo de retención mayor, planee la descarga y el almacenamiento de los registros mediante alguno de los métodos que se muestran a continuación.

> [!NOTE]
> No puede ver inicios de sesión de usuario para aplicaciones individuales de Azure AD B2C en la sección **Usuarios** de la hoja **Azure Active Directory** o **Azure AD B2C**. Los inicios de sesión mostrarán la actividad del usuario, pero no pueden correlacionarse de nuevo con la aplicación B2C en que el usuario inició sesión. Debe usar los registros de auditoría para ello, como se explica en este artículo.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Información general sobre las actividades disponibles en la categoría B2C de los registros de auditoría
La categoría **B2C** de los registros de auditoría contiene los siguientes tipos de actividades:

|Tipo de actividad |DESCRIPCIÓN  |
|---------|---------|
|Authorization |Actividades relacionadas con la autorización de un usuario para acceder a los recursos de B2C (por ejemplo, un administrador que accede a una lista de directivas de B2C)         |
|Directorio |Actividades relacionadas con los atributos de directorio que se recuperan cuando un administrador inicia sesión con Azure Portal |
|Application | Operaciones CRUD en aplicaciones B2C |
|Clave |Operaciones CRUD en claves almacenadas en el contenedor de claves B2C |
|Recurso |Operaciones CRUD en recursos de B2C (por ejemplo, directivas y proveedores de identidades)
|Authentication |Validación de credenciales de usuario y emisión de tokens|

> [!NOTE]
> Para las actividades CRUD de objeto de usuario, vea la categoría **Core Directory**.

## <a name="example-activity"></a>Actividad de ejemplo
En el ejemplo siguiente se muestran los datos capturados cuando un usuario inicia sesión con un proveedor de identidades externo: ![Ejemplo de la página Detalles de la actividad: Registro de auditoría de Azure Portal](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

El panel de detalles de actividad contiene la siguiente información pertinente:

|Sección|Campo|DESCRIPCIÓN|
|-------|-----|-----------|
| Actividad | NOMBRE | Qué actividad tuvo lugar. Por ejemplo, "Emitir un valor de id_token a la aplicación" (que finaliza el inicio de sesión efectivo del usuario). |
| Iniciado por (actor) | ObjectId | El **identificador de objeto** de la aplicación B2C en que el usuario está iniciando sesión (este identificador no es visible en Azure Portal, pero es accesible a través de Graph API, por ejemplo). |
| Iniciado por (actor) | Spn | El **identificador de aplicación** de la aplicación B2C en que el usuario está iniciando sesión. |
| Destinos | ObjectId | El **identificador de objeto** del usuario que está iniciando sesión. |
| Detalles adicionales | TenantId | El **identificador de inquilino** del inquilino de Azure AD B2C. |
| Detalles adicionales | PolicyId | El **identificador de directiva** del flujo de usuario (directiva) que se usa para iniciar la sesión del usuario. |
| Detalles adicionales | ApplicationId | El **identificador de aplicación** de la aplicación B2C en que el usuario está iniciando sesión. |

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Acceso a los registros de auditoría a través de Azure Portal
1. Vaya a [Azure Portal](https://portal.azure.com). Asegúrese de que se encuentra en el directorio B2C.
2. Abra **Azure Active Directory** en la barra de favoritos del lado izquierdo

    ![Botón Azure Active Directory resaltado en el menú del portal izquierdo](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. En **Actividad**, haga clic en **Registros de auditoría**

    ![Botón Registros de auditoría resaltados en la sección Actividad del menú](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. En el cuadro desplegable **Categoría**, seleccione **B2C**
3. Haga clic en **Aplicar**

    ![Categoría y botón Aplicar resaltados en el filtro de registros de auditoría](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Se ve una lista de actividades registradas durante los últimos siete días.
- Use la lista desplegable **Tipo de recurso de actividad** para filtrar por los tipos de actividad descritos arriba
- Use la lista desplegable **Intervalo de fechas** para filtrar el intervalo de fechas de las actividades mostradas
- Si hace clic en una fila determinada de la lista, un cuadro contextual a la derecha muestra los atributos adicionales asociados a la actividad
- Haga clic en **Descargar** para descargar las actividades como un archivo csv

> [!NOTE]
> También puede ver los registros de auditoría; para ello, vaya a **Azure AD B2C** en lugar de a **Azure Active Directory** en la barra de favoritos a la izquierda. En **Actividades**, haga clic en **Registros de auditoría**, donde encontrará los mismos registros con capacidades de filtrado similares.

## <a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Acceso a los registros de auditoría a través de la API de informes de Azure AD
Los registros de auditoría se publican en la misma canalización que otras actividades de Azure Active Directory, por lo que son accesibles a través de la [API de informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference).

### <a name="prerequisites"></a>Requisitos previos
Para autenticarse en la API de informes de Azure AD, debe registrar una aplicación. Asegúrese de seguir los pasos de [Requisitos previos para acceder a la API de informes de Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

### <a name="accessing-the-api"></a>Acceso a la API
Para descargar los registros de auditoría de Azure AD B2C a través de la API, se recomienda filtrar los registros en la categoría **B2C**. Para filtrar por categoría, use el parámetro de cadena de consulta al llamar al punto de conexión de la API de informes de Azure AD, como se muestra a continuación:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

### <a name="powershell-script"></a>Script de PowerShell
El script siguiente proporciona un ejemplo de uso de PowerShell para consultar la API de informes de Azure AD y almacenar los resultados como un archivo JSON:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago

    # loop through each query page (1 through n)
    Do {
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```
