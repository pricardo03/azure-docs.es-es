---
title: Autenticación y autorización mediante API en Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo configurar la autenticación y la autorización para una aplicación personalizada que llama a la API de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 9b6cd993e9f6c6dbf173c161de638c6c4a8b18d3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237054"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Autenticación y autorización para la API de Azure Time Series Insights

En este artículo se explica cómo configurar la autenticación y la autorización utilizadas en una aplicación personalizada que llama a la API de Azure Time Series Insights.

> [!TIP]
> Obtenga información sobre [conceder acceso a datos](./time-series-insights-data-access.md) a su entorno de Time Series Insights en Azure Active Directory.

## <a name="service-principal"></a>Entidad de servicio

Esto y las siguientes secciones describen cómo configurar una aplicación para acceder a la API de Time Series Insights en nombre de la aplicación. La aplicación puede consultar o publicar datos de referencia en el entorno de Time Series Insights con credenciales de la aplicación en lugar de las credenciales de usuario.

## <a name="best-practices"></a>Procedimientos recomendados

Cuando haya una aplicación que necesita acceder a Time Series Insights:

1. Configurar una aplicación de Azure Active Directory.
1. [Asignación de directivas de acceso a datos](./time-series-insights-data-access.md) en el entorno de Time Series Insights.

Uso de la aplicación en lugar de sus credenciales de usuario es deseable desde:

* Puede asignar permisos a la identidad de aplicación que son distintos de sus propios permisos. Normalmente, estos permisos están restringidos a exactamente aquello que la aplicación requiere. Por ejemplo, puede permitir que la aplicación solo lea datos en un entorno de Time Series Insights determinado.
* No debe cambiar las credenciales de la aplicación, si las responsabilidades cambian.
* Puede usar un certificado o una clave de aplicación para automatizar la autenticación cuando vaya a ejecutar un script desatendido.

Las secciones siguientes muestran cómo realizar esos pasos a través del portal de Azure. El artículo se centra en una aplicación de inquilino único donde la aplicación está diseñada para ejecutarse en sólo una organización. Normalmente, usará las aplicaciones de inquilino único para aplicaciones de línea de negocio que se ejecutan en su organización.

## <a name="set-up-summary"></a>Configuración de resumen

El flujo del programa de instalación consta de tres pasos:

1. Creación de una aplicación en Azure Active Directory.
1. Autorización a esta aplicación a acceder al entorno de Time Series Insights.
1. Use el identificador de la aplicación y la clave para adquirir un token de `https://api.timeseries.azure.com/`. Luego el token puede usarse para llamar a la API de Time Series Insights.

## <a name="detailed-setup"></a>Programa de instalación detallada

1. En Azure Portal, seleccione **Azure Active Directory** > **Registros de aplicaciones** > **Nuevo registro de aplicaciones**.

   [![Nuevo registro de aplicaciones en Azure Active Directory](media/authentication-and-authorization/active-directory-new-application-registration.png)](media/authentication-and-authorization/active-directory-new-application-registration.png#lightbox)

1. Asigne un nombre a la aplicación, seleccione **Aplicación web o API** como tipo, seleccione cualquier URI válido para **URL de inicio de sesión** y haga clic en **Crear**.

   [![Crear la aplicación en Azure Active Directory](media/authentication-and-authorization/active-directory-create-web-api-application.png)](media/authentication-and-authorization/active-directory-create-web-api-application.png#lightbox)

1. Seleccione la aplicación recién creada y copie su identificador en el editor de texto que prefiera.

   [![Copie el identificador de aplicación](media/authentication-and-authorization/active-directory-copy-application-id.png)](media/authentication-and-authorization/active-directory-copy-application-id.png#lightbox)

1. Seleccione **Claves**, escriba el nombre de la clave, seleccione la expiración y haga clic en **Guardar**.

   [![Seleccione las claves de aplicación](media/authentication-and-authorization/active-directory-application-keys.png)](media/authentication-and-authorization/active-directory-application-keys.png#lightbox)

   [![Escriba el nombre de clave y la expiración y haga clic en Guardar](media/authentication-and-authorization/active-directory-application-keys-save.png)](media/authentication-and-authorization/active-directory-application-keys-save.png#lightbox)

1. Copie la clave en el editor de texto que prefiera.

   [![Copie la clave de aplicación](media/authentication-and-authorization/active-directory-copy-application-key.png)](media/authentication-and-authorization/active-directory-copy-application-key.png#lightbox)

1. Para el entorno de Time Series Insights, seleccione **Directivas de acceso a datos** y haga clic en **Agregar**.

   [![Agregar nueva directiva de acceso de datos en el entorno de Time Series Insights](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. En el **Seleccionar usuario** diálogo cuadro, pegue el nombre de la aplicación (desde **paso 2**) o el identificador de la aplicación (desde **paso 3**).

   [![Buscar una aplicación en el cuadro de diálogo Seleccionar usuario](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Seleccione el rol (**lector** para consultar datos, **colaborador** para consultar los datos y cambiar los datos de referencia) y haga clic en **Aceptar**.

   [![Selección de lector o colaborador en el cuadro de diálogo Seleccionar rol](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Guarde la directiva haciendo clic en **Aceptar**.

1. Use el identificador de aplicación (desde **paso 3**) y la clave de aplicación (desde **paso 5**) para adquirir el token en nombre de la aplicación. Luego el token se puede pasar en el encabezado `Authorization` cuando la aplicación llame a la API de Time Series Insights.

    Si está usando C#, puede emplear el siguiente código para conseguir el token en nombre de la aplicación. Para obtener un ejemplo completo, vea [Consulta de datos mediante C#](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/",
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

Use la **Id. de aplicación** y **clave** en la aplicación para autenticarse con Azure Time Series Insights.

## <a name="next-steps"></a>Pasos siguientes

- Para consultar código de ejemplo que llama a la API de Time Series Insights, vea [Consulta de datos mediante C#](time-series-insights-query-data-csharp.md).

- Para obtener información de referencia de la API, vea la [referencia sobre la API de consulta](/rest/api/time-series-insights/ga-query-api).

- Obtenga información sobre cómo [crear una entidad de servicio](../active-directory/develop/howto-create-service-principal-portal.md).
