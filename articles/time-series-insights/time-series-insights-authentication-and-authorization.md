---
title: Autenticación y autorización mediante una API en Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo configurar la autenticación y la autorización para una aplicación personalizada que llama a la API de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/29/2019
ms.custom: seodec18
ms.openlocfilehash: bdf0fbfb2b40e932f9e3627c3bc0356eb0afb472
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677925"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Autenticación y autorización para la API de Azure Time Series Insights

En este documento se describe cómo registrar una aplicación en Azure Active Directory mediante la nueva hoja de Azure Active Directory. Las aplicaciones registradas en Azure Active Directory permiten a los usuarios autenticarse y recibir permiso para usar la API de Azure Time Series Insight asociada a un entorno de Time Series Insights.

## <a name="service-principal"></a>Entidad de servicio

En las siguientes secciones se explica cómo configurar una aplicación para acceder a la API de Time Series Insights en nombre de una aplicación. Después, la aplicación puede consultar o publicar datos de referencia en el entorno de Time Series Insights con sus propias credenciales de aplicación mediante Azure Active Directory.

## <a name="summary-and-best-practices"></a>Resumen y procedimientos recomendados

El flujo de registro de una aplicación de Azure Active Directory conlleva tres pasos principales.

1. [Registrar una aplicación](#azure-active-directory-app-registration) en Azure Active Directory.
1. Autorizar a la aplicación para tener [acceso a datos en el entorno de Time Series Insights](#granting-data-access).
1. Usar el **identificador de la aplicación** y el **secreto de cliente** para obtener un token de `https://api.timeseries.azure.com/` en la [aplicación cliente](#client-app-initialization). Luego el token puede usarse para llamar a la API de Time Series Insights.

Según el **paso 3**, distinguir entre las credenciales de la aplicación y las suyas de usuario le permite lo siguiente:

* Asignar permisos a la identidad de la aplicación que sean diferentes a los suyos propios. Normalmente, estos permisos están restringidos a exactamente aquello que la aplicación requiere. Por ejemplo, puede permitir que la aplicación lea datos de únicamente un entorno de Time Series Insights determinado.
* Aislar la seguridad de la aplicación de las credenciales de autenticación del usuario, utilizando para ello un **secreto de cliente** o un certificado de seguridad. Como resultado, las credenciales de la aplicación no son dependientes de las credenciales de un usuario específico. Si el rol del usuario cambia, la aplicación no necesitará nuevas credenciales ni ninguna configuración adicional. Si el usuario cambia de contraseña, todo el acceso a la aplicación no requiere nuevas credenciales o claves.
* Ejecutar un script de instalación desatendida con un **secreto de cliente** o con un certificado de seguridad, en lugar de con las credenciales de un usuario específico (lo que requeriría estar presente).
* Usar un certificado de seguridad en lugar de una contraseña para proteger el acceso a la API de Azure Time Series Insights.

> [!IMPORTANT]
> Siga el principio de **Separación de intereses** (descrito anteriormente para este escenario) al configurar la directiva de seguridad de Azure Time Series Insights.

> [!NOTE]
> * Este artículo se centra en una aplicación de un único inquilino diseñada para ejecutarse en una sola organización.
> * Las aplicaciones de un único inquilino suelen usarse en aplicaciones de línea de negocio que se ejecutan en la organización.

## <a name="detailed-setup"></a>Configuración detallada

### <a name="azure-active-directory-app-registration"></a>Registro de la aplicación de Azure Active Directory

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Conceder acceso a datos

1. En el entorno de Time Series Insights, seleccione **Directivas de acceso a datos** y, después, **Agregar**.

   [![Adición de nueva directiva de acceso a datos al entorno de Time Series Insights](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. En el cuadro de diálogo **Seleccionar usuario**, pegue el **nombre de la aplicación** o el **identificador de la aplicación** desde la sección de registro de la aplicación de Azure Active Directory.

   [![Búsqueda de una aplicación en el cuadro de diálogo Seleccionar usuario](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Seleccione el rol. Seleccione **Lector** para consultar datos o **Colaborador** para consultar datos y cambiar los datos de referencia. Seleccione **Aceptar**.

   [![Selección de Lector o Colaborador en el cuadro de diálogo Seleccionar rol de usuario](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Seleccione **Aceptar** para guardar la directiva.

   > [!TIP]
   > Lea sobre cómo [conceder acceso a datos](./time-series-insights-data-access.md) en el entorno de Time Series Insights en Azure Active Directory.

### <a name="client-app-initialization"></a>Inicialización de la aplicación cliente

1. Use el **identificador de la aplicación** y el **secreto de cliente** (clave de aplicación) de la sección de registro de la aplicación de Azure Active Directory para obtener el token en nombre de la aplicación.

    En C#, puede emplear el siguiente código para conseguir el token en nombre de la aplicación. Para obtener un ejemplo completo, vea [Consulta de datos mediante C#](time-series-insights-query-data-csharp.md).

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

1. Luego el token se puede pasar en el encabezado `Authorization` cuando la aplicación llame a la API de Time Series Insights.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener código de ejemplo que llama a la API de Time Series Insights (disponibilidad general), vea [Consulta de datos mediante C#](./time-series-insights-query-data-csharp.md).

- Para obtener ejemplos de código de la API de Time Series Insights (versión preliminar), vea [Consulta de datos de versión preliminar mediante C#](./time-series-insights-update-query-data-csharp.md).

- Para obtener información de referencia de la API, vea la [referencia sobre la API de consulta](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api).

- Sepa cómo [crear una entidad de servicio](../active-directory/develop/howto-create-service-principal-portal.md).