---
title: Descripción de la autenticación en las API desde Azure Digital Twins | Microsoft Docs
description: Uso de Azure Digital Twins para conectarse y autenticarse en las API
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: lyrana
ms.openlocfilehash: f85ab05e785ea559962490b43e75b196d1602159
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51016223"
---
# <a name="connect-and-authenticate-to-apis"></a>Conexión y autenticación en las API

Azure Digital Twins utiliza Azure Active Directory (Azure AD) para autenticar a los usuarios y proteger a las aplicaciones. Azure AD admite la autenticación de diversas arquitecturas modernas. Todas ellas se basan en los protocolos estándar del sector OAuth 2.0 u OpenID Connect. Además, los desarrolladores pueden usar Azure AD para compilar las aplicaciones de inquilino único y línea de negocio (LOB). Los desarrolladores pueden usar Azure AD para desarrollar aplicaciones para varios inquilinos.

Para obtener información general de Azure AD, visite la [página de aspectos básicos](https://docs.microsoft.com/azure/active-directory/fundamentals/index) para acceder a guías paso a paso, conceptos y guías de inicio rápido.

Para integrar una aplicación o un servicio con Azure AD, el desarrollador debe registrar primero la aplicación en Azure AD. Para obtener instrucciones detalladas y capturas de pantalla, consulte [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Azure AD admite [cinco escenarios principales de aplicación](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types):

* Aplicación de una sola página (SPA): un usuario tiene que iniciar sesión en una aplicación de una sola página protegida por Azure AD.
* Explorador web para aplicación web: un usuario tiene que iniciar sesión en una aplicación web protegida por Azure AD.
* Aplicación nativa para API web: una aplicación nativa que se ejecuta en teléfonos, tabletas o equipos tiene que autenticar a un usuario para obtener recursos de una API web protegida por Azure AD.
* Aplicación web para API web: una aplicación web tiene que obtener recursos de una API web protegida por Azure AD.
* Aplicación de servidor o demonio para API web: una aplicación de demonio o de servidor sin interfaz de usuario web tiene que obtener recursos de una API web protegida por Azure AD.

La biblioteca de autenticación de Azure para Windows ofrece muchas alternativas para adquirir tokens de Active Directory. Para obtener más información en la biblioteca y ejemplos de código, vea [este artículo](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Llamada a Digital Twins desde una API web de nivel intermedio

Cuando los desarrolladores diseñan soluciones de Digital Twins, normalmente crean una aplicación o API web de nivel intermedio. La aplicación o la API, a continuación, llama a la API de Digital Twins de un nivel inferior. Los usuarios se autentican primero en la aplicación de nivel intermedio y, a continuación, se usa un flujo de token en el nombre de otra persona durante la llamada al nivel inferior. Para obtener instrucciones sobre cómo coordinar el flujo en el nombre de otra persona, visite [esta página](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). También puede ver ejemplos de código en [esta página](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).


## <a name="test-with-the-postman-client"></a>Pruebas con el cliente de Postman

Para empezar a trabajar con las API de Digital Twins, puede usar un cliente como Postman como entorno de API. Postman le ayuda a crear rápidamente solicitudes HTTP complejas. Los pasos siguientes muestran cómo obtener un token de Azure AD que se necesita para llamar a Digital Twins dentro de la interfaz de usuario de Postman.


1. Vaya a https://www.getpostman.com/ para descargar la aplicación
1. Siga los pasos de [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para crear una aplicación de Azure AD. O bien, puede volver a usar un registro existente. 
1. En **Permisos necesarios**, agregue "Azure Digital Twins" y seleccione **Permisos delegados**. A continuación, seleccione **Conceder permisos**.
1. Abra el manifiesto de aplicación y establezca **oauth2AllowImplicitFlow** en true.
1. Configure una dirección URL de respuesta a [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback).
1. Seleccione la **pestaña Autorización**, seleccione **OAuth 2.0** y luego **Get New Access Token** (Obtener nuevo token de acceso).

    |**Campo**  |**Valor** |
    |---------|---------|
    | Tipo de concesión | Implícita |
    | Dirección URL de devolución de llamadas | [https://www.getpostman.com/oauth2/callback](https://www.getpostman.com/oauth2/callback) |
    | Dirección URL de autenticación | https://login.microsoftonline.com/<Your Azure AD Tenant e.g. Contoso>.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0 |
    | Id. de cliente | Use el identificador de aplicación para la aplicación de Azure AD que se creó o se reutilizó del paso 2. |
    | Ámbito | déjelo en blanco. |
    | Estado | déjelo en blanco. |
    | Autenticación de clientes | Se envía como encabezado de autenticación básico. |

1. Seleccione **Request Token** (Solicitar token).

    >[!NOTE]
    >Si recibe el mensaje de error "No se pudo completar OAuth 2", pruebe lo siguiente:
    > * Cierre Postman, vuelva a abrirlo e inténtelo de nuevo.
   
1. Desplácese hacia abajo y seleccione **Use Token** (Usar token).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la seguridad en Azure Digital Twins, consulte [Create and manage role assignments](./security-create-manage-role-assignments.md) (Creación y administración de las asignaciones de roles).
