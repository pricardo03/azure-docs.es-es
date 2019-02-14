---
title: Microsoft Graph para Azure Active Directory Identity Protection | Microsoft Docs
description: Obtenga información sobre cómo realizar una consulta a Microsoft Graph para obtener una lista de eventos de riesgo e información asociada desde Azure Active Directory.
services: active-directory
keywords: azure active directory identity protection, evento de riesgo, vulnerabilidad, directiva de seguridad, Microsoft Graph
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: markvi
ms.reviewer: nigu
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36a2fc9d1ab364d44bd5dd6e7930e5d8ae3fb78d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56204753"
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Introducción a Azure Active Directory Identity Protection y Microsoft Graph

Microsoft Graph es el punto de conexión de API unificada de Microsoft y donde se encuentran las API de [Azure Active Directory Identity Protection](../active-directory-identityprotection.md). Hay tres API que exponen información acerca de los usuarios e inicios de sesión con riesgo. La primera API, **identityRiskEvents**, le permite consultar una lista de Microsoft Graph de [eventos de riesgo](../reports-monitoring/concept-risk-events.md) así como información asociada. La segunda API, **riskyUsers**, permite consultar Microsoft Graph, para obtener información acerca de qué usuarios ha detectado Identity Protection como un riesgo. La tercera API, **signIn**, permite consultar Microsoft Graph para obtener información acerca de los inicios de sesión de Azure AD con propiedades concretas relacionadas con el estado, detalle y nivel del riesgo. En este artículo le introduce a la [conexión a Microsoft Graph](#Connect-to-Microsoft-Graph) y a la [consulta de estas API](#Query-the-APIs). Para obtener una introducción más detallada, ver toda la documentación y acceder al Probador de Graph, diríjase al [sitio web de Microsoft Graph](https://graph.microsoft.io/) o consulte la documentación de referencia específica de estas API:

* [identityRiskEvents API](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta)
* [riskyUsers API](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)
* [signIn API](https://docs.microsoft.com/graph/api/resources/signin?view=graph-rest-beta)


## <a name="connect-to-microsoft-graph"></a>Conexión con Microsoft Graph

Hay cuatro pasos para acceder a los datos de Identity Protection a través de Microsoft Graph:

1. Recupere el nombre de dominio.
2. Cree un nuevo registro de aplicaciones. 
3. Use este secreto y otro tipo de información para autenticarse en Microsoft Graph, donde recibirá un token de autenticación. 
4. Utilice este token para realizar solicitudes en el punto de conexión de API y obtener datos de Identity Protection.

Antes de comenzar, necesitará lo siguiente:

* Privilegios de administrador para crear la aplicación en Azure AD
* El nombre de dominio del inquilino (por ejemplo, contoso.onmicrosoft.com)


## <a name="retrieve-your-domain-name"></a>Recuperación del nombre de dominio 

1. [Inicie sesión](https://portal.azure.com) en Azure Portal como administrador. 

2. En el panel de navegación izquierdo,haga clic en **Active Directory**. 
   
    ![Creación de una aplicación](./media/graph-get-started/41.png)


3. En la sección **Administrar**, haga clic en **Propiedades**.

    ![Creación de una aplicación](./media/graph-get-started/42.png)

4. Copie su nombre de dominio.


## <a name="create-a-new-app-registration"></a>Creación de un registro de aplicaciones

1. En la página **Active Directory**, en la sección **Administrar**, haga clic en **Registros de aplicaciones**.

    ![Creación de una aplicación](./media/graph-get-started/42.png)


2. En el menú de la parte superior, haga clic en **Nuevo registro de aplicaciones**.
   
    ![Creación de una aplicación](./media/graph-get-started/43.png)

3. En la página **Crear**, realice los pasos siguientes:
   
    ![Creación de una aplicación](./media/graph-get-started/44.png)

     a. En el cuadro de texto **Nombre**, escriba el nombre de la aplicación (por ejemplo: AADIP Risk Event API Application).
   
    b. Como **Tipo**, seleccione **Aplicación web y/o API web**.
   
    c. En el cuadro de texto **URL de inicio de sesión**, escriba `http://localhost`.

    d. Haga clic en **Create**(Crear).

4. Para abrir la página **Configuración**, en la lista de aplicaciones, haga clic en el registro de aplicación recién creado. 

5. Copie el **id. de la aplicación**.


## <a name="grant-your-application-permission-to-use-the-api"></a>Conceder a la aplicación permiso para usar la API

1. En la página **Configuración**, haga clic en **Permisos necesarios**.
   
    ![Creación de una aplicación](./media/graph-get-started/15.png)

2. En la página **Permisos necesarios**, en la barra de herramientas de la parte superior, haga clic en **Agregar**.
   
    ![Creación de una aplicación](./media/graph-get-started/16.png)
   
3. En la página **Agregar acceso de API**, haga clic en **Seleccionar una API**.
   
    ![Creación de una aplicación](./media/graph-get-started/17.png)

4. En la página **Seleccionar una API**, seleccione **Microsoft Graph** y, a continuación, haga clic en **Seleccionar**.
   
    ![Creación de una aplicación](./media/graph-get-started/18.png)

5. En la página **Agregar acceso de API**, haga clic en **Seleccionar permisos**.
   
    ![Creación de una aplicación](./media/graph-get-started/19.png)

6. En la página **Habilitar acceso**, haga clic en **Read all identity risk information** (Leer toda la información de riesgo de identidad) y, a continuación, haga clic en **Seleccionar**.
   
    ![Creación de una aplicación](./media/graph-get-started/20.png)

7. En la página **Agregar acceso de API**, haga clic en **Listo**.
   
    ![Creación de una aplicación](./media/graph-get-started/21.png)

8. En la página **Permisos necesarios**, haga clic en **Conceder permisos** y en **Sí**.
   
    ![Creación de una aplicación](./media/graph-get-started/22.png)



## <a name="get-an-access-key"></a>Obtención de una clave de acceso

1. En la página **Configuración**, haga clic en **Claves**.
   
    ![Creación de una aplicación](./media/graph-get-started/23.png)

2. En la página **Claves**, siga estos pasos:
   
    ![Creación de una aplicación](./media/graph-get-started/24.png)

     a. En el cuadro de texto **Descripción de la clave**, escriba una descripción (por ejemplo, *Evento de riesgo de AADIP*).
    
    b. Como **Duración**, seleccione **1 año**.

    c. Haga clic en **Save**(Guardar).
   
    d. Copie el valor de clave y, a continuación, péguelo en una ubicación segura.   
   
   > [!NOTE]
   > Si pierde esta clave, tendrá que volver a esta sección y crear una nueva. Guarde esta clave como un secreto: cualquier persona que la tenga accederá a sus datos.
   > 
   > 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Autenticación en Microsoft Graph y consulta a Identity Risk Events API
En este momento, debe tener:

- El nombre de dominio del inquilino

- El id. de cliente 

- La clave 


Para autenticarse, envíe una solicitud post a `https://login.microsoft.com` con los siguientes parámetros en el cuerpo:

- grant_type: “**client_credentials**”

-  recurso: "**https://graph.microsoft.com**"

- client_id: \<id. de cliente\>

- client_secret: \<su clave\>


Si se realiza correctamente, devuelve un token de autenticación.  
Para llamar a la API, cree un encabezado con el parámetro siguiente:

    `Authorization`=”<token_type> <access_token>"


Al autenticar, puede encontrar el tipo de token y el token de acceso en el token devuelto.

Envíe este encabezado como una solicitud a la siguiente dirección URL de la API: `https://graph.microsoft.com/beta/identityRiskEvents`

La respuesta, si se ha realizado correctamente, es una colección de eventos de riesgo de identidad y datos asociados en formato JSON de OData, que se pueden analizar y tratar como convenga.

Este es el código de ejemplo para autenticar y llamar a la API mediante PowerShell.  
Solo tiene que agregar el identificador de cliente, la clave secreta y el dominio del inquilino.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 

## <a name="query-the-apis"></a>Consulta de las API

Estas tres API ofrecen una gran cantidad de oportunidades para recuperar información acerca de los usuarios e inicios de sesión de riesgo en su organización. A continuación se muestran algunos casos de uso comunes de estas API y las solicitudes de ejemplo asociadas. Para ejecutar puede usar el código de ejemplo anterior, o el [Probador de Graph](https://developer.microsoft.com/graph/graph-explorer).

### <a name="get-the-high-risk-and-medium-risk-events-identityriskevents-api"></a>Obtención de eventos de riesgo medio y riesgo elevado (identityRiskEvents API)

Los eventos de riesgo medio y alto son los que pueden tener la funcionalidad de desencadenar el inicio de sesión de Identity Protection o las directivas de riesgo del usuario. Puesto que tienen una probabilidad media o alta de que el usuario que intenta iniciar sesión no sea el propietario legítimo de la identidad, la solución de estos eventos debería ser una prioridad. 

```
GET https://graph.microsoft.com/beta/identityRiskEvents?`$filter=riskLevel eq 'high' or riskLevel eq 'medium'" 
```

### <a name="get-all-of-the-users-who-successfully-passed-an-mfa-challenge-triggered-by-risky-sign-ins-policy-riskyusers-api"></a>Obtención de todos los usuarios que han superado satisfactoriamente un desafío de MFA desencadenado por la directiva de inicios de sesión de riesgo (riskyUsers API)

Para conocer el impacto que las directivas basadas en riesgo de Identity Protection tienen en su organización puede consultar todos los usuarios que han superado satisfactoriamente un desafío de MFA desencadenado por una directiva de inicios de sesión de riesgo. Esta información puede ayudarle a saber qué usuarios puede haber detectado Identity Protection falsamente como un riesgo y cuáles de los usuarios legítimos pueden estar realizando acciones que la IA considera de riesgo.

```
GET https://graph.microsoft.com/beta/riskyUsers?$filter=riskDetail eq 'userPassedMFADrivenByRiskBasedPolicy'
```

### <a name="get-all-the-risky-sign-ins-for-a-specific-user-signin-api"></a>Obtención de todos los inicios de sesión de riesgo de un usuario concreto (signIn API)

Si cree que un usuario puede haber estado en peligro, puede conocer mejor el estado de su riesgo mediante la recuperación de todos sus inicios de sesión de riesgo. 
```
https://graph.microsoft.com/beta/identityRiskEvents?`$filter=userID eq '<userID>' and riskState eq 'atRisk'
```




# <a name="next-steps"></a>Pasos siguientes

Enhorabuena, acaba de hacer la primera llamada a Microsoft Graph.  
Ahora puede consultar los eventos de riesgo de identidad y utilizar los datos cuando lo estime necesario.


Para obtener más información sobre Microsoft Graph y cómo crear aplicaciones con Graph API, consulte la [documentación](https://docs.microsoft.com/graph/overview) y muchos más detalles en el [sitio web de Microsoft Graph](https://developer.microsoft.com/graph). 


Para obtener información relacionada, consulte:

-  [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)

-  [Types of risk events detected by Azure Active Directory Identity Protection (Tipos de eventos de riesgo que detecta Azure Active Directory Identity Protection)](../reports-monitoring/concept-risk-events.md)

- [Microsoft Graph](https://developer.microsoft.com/graph/)

- [Overview of Microsoft Graph (Información general de Microsoft Graph)](https://developer.microsoft.com/graph/docs)

- [Azure AD Identity Protection Service Root (Raíz del servicio de Azure AD Identity Protection)](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityprotection_root)
