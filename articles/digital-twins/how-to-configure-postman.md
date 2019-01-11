---
title: Configuración de Postman para Azure Digital Twins | Microsoft Docs
description: Configuración de Postman para Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: adgera
ms.openlocfilehash: 92ff8cb732c7c10c525d8a8ec76180cb435bd466
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53975022"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Configuración de Postman para Azure Digital Twins

En este artículo se describe cómo configurar una aplicación de Azure Active Directory (Azure AD) para usar el flujo de concesión implícita de OAuth 2.0. A continuación, describe cómo configurar el cliente REST de Postman para realizar solicitudes HTTP que admiten token a las API de administración.

## <a name="postman-summary"></a>Resumen de Postman

Empiece a trabajar en Azure Digital Twins mediante una herramienta de cliente REST, como [Postman](https://www.getpostman.com/), para preparar el entorno de pruebas local. El cliente de Postman le ayuda a crear rápidamente las solicitudes HTTP complejas. Descargue la versión de escritorio del cliente Postman visitando [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) es herramienta de pruebas REST que localiza funcionalidades clave de solicitudes HTTP en un escritorio útil y la interfaz gráfica de usuario basada en complementos. A través del cliente Postman, los programadores de soluciones pueden especificar el tipo de solicitud HTTP (POST, GET, UPDATE, PATCH y DELETE), el punto de conexión API para llamar y el uso de SSL. Postman también admite la incorporación de encabezados de solicitud HTTP, parámetros, datos de formulario y cuerpos.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Configuración de Azure Active Directory para usar el flujo de concesión implícita de OAuth 2.0

Configure la aplicación de Azure AD para usar el flujo de concesión implícita de OAuth 2.0.

1. Siga los pasos de [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para crear una aplicación de Azure AD de tipo nativo. O bien, puede volver a usar un registro de aplicación nativa existente.

1. En **Permisos necesarios**, seleccione **Agregar** y escriba **Azure Digital Twins** en **Agregar acceso de API**. Si la búsqueda no encuentra la API, busque en su lugar **Azure Smart Spaces**. A continuación, seleccione **Conceder permisos > Permisos delegados** y **Listo**.

    ![api de incorporación de registros de aplicación de Azure AD](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)

1. Haga clic en **Manifiesto** para abrir el manifiesto de aplicación para la aplicación. Establezca *oauth2AllowImplicitFlow* en `true`.

      ![Flujo implícito de Azure AD][1]

1. Configure **URL de respuesta** en [`https://www.getpostman.com/oauth2/callback`](https://www.getpostman.com/oauth2/callback).

      ![Dirección URL de respuesta de Azure AD][2]

1. Copie y mantenga el valor de **Id. de aplicación** de la aplicación de Azure AD. Se usa a continuación.

## <a name="configure-the-postman-client"></a>Configuración del cliente Postman

A continuación, instale y configure Postman para obtener un token de Azure AD. Después, realice una solicitud HTTP autenticada a Azure Digital Twins mediante el token adquirido:

1. Vaya a [www.getpostman.com]([https://www.getpostman.com/) para descargar la aplicación.
1. Asegúrese de que el valor de **Dirección URL de autorización** es correcto. Debe tener el formato siguiente:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | NOMBRE  | Reemplazar por | Ejemplo |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | El nombre de su inquilino u organización | `microsoft` |

1. Seleccione la **pestaña Autorización**, seleccione **OAuth 2.0** y luego **Get New Access Token** (Obtener nuevo token de acceso).

    | Campo  | Valor |
    |---------|---------|
    | Tipo de concesión | `Implicit` |
    | Dirección URL de devolución de llamadas | `https://www.getpostman.com/oauth2/callback` |
    | Dirección URL de autenticación | Use el valor de **Dirección URL de autorización** del paso 2 anterior |
    | Id. de cliente | Use el valor de **Id. de aplicación** para la aplicación de Azure AD que se creó o se reutilizó en la sección anterior |
    | Ámbito | Déjelo en blanco |
    | Estado | Déjelo en blanco |
    | Autenticación de clientes | `Send as Basic Auth header` |

1. Ahora, el cliente debe tener un aspecto similar a:

   ![Ejemplo de cliente de Postman][3]

1. Seleccione **Request Token** (Solicitar token).

    >[!NOTE]
    >Si recibe el mensaje de error "No se pudo completar OAuth 2", pruebe lo siguiente:
    > * Cierre Postman, vuelva a abrirlo e inténtelo de nuevo.
  
1. Desplácese hacia abajo y seleccione **Use Token** (Usar token).

## <a name="next-steps"></a>Pasos siguientes

Para aprender sobre la autenticación con la API de administración, lea [Conexión y autenticación en las API](./security-authenticating-apis.md).

<!-- Images -->
[1]: media/how-to-configure-postman/implicit-flow.png
[2]: media/how-to-configure-postman/reply-url.png
[3]: media/how-to-configure-postman/postman-oauth-token.png
