---
title: Configuración de Postman para Azure Digital Twins | Microsoft Docs
description: Configuración de Postman para Azure Digital Twins
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 08/09/2019
ms.author: v-adgera
ms.openlocfilehash: 7ceb36d818c84642461372f0df70c8088908550c
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965808"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Configuración de Postman para Azure Digital Twins

En este artículo se describe cómo configurar el cliente de REST de Postman para interactuar con las API de administración de Azure Digital Twins y probarlas. En concreto, trata:

* Cómo configurar una aplicación de Azure Active Directory para usar el flujo de concesión implícita de OAuth 2.0.
* Cómo usar el cliente REST de Postman para realizar solicitudes HTTP que admiten token a las API de administración.
* Cómo usar Postman para realizar solicitudes POST de varias partes en las API de administración.

## <a name="postman-summary"></a>Resumen de Postman

Empiece a trabajar en Azure Digital Twins mediante una herramienta de cliente REST, como [Postman](https://www.getpostman.com/), para preparar el entorno de pruebas local. El cliente de Postman le ayuda a crear rápidamente las solicitudes HTTP complejas. Descargue la versión de escritorio del cliente Postman visitando [www.getpostman.com/apps](https://www.getpostman.com/apps).

[Postman](https://www.getpostman.com/) es herramienta de pruebas REST que localiza funcionalidades clave de solicitudes HTTP en un escritorio útil y la interfaz gráfica de usuario basada en complementos.

A través del cliente Postman, los programadores de soluciones pueden especificar el tipo de solicitud HTTP (*POST*, *GET*, *UPDATE*, *PATCH* y *DELETE*), el punto de conexión API para llamar y el uso de SSL. Postman también admite la incorporación de encabezados de solicitud HTTP, parámetros, datos de formulario y cuerpos.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Configuración de Azure Active Directory para usar el flujo de concesión implícita de OAuth 2.0

Configure la aplicación Azure Active Directory para usar el flujo de concesión implícita de OAuth 2.0.

1. Siga los pasos de [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) para crear una aplicación de Azure AD de tipo nativo. O bien, puede volver a usar un registro de aplicación nativa existente.

1. En **Permisos necesarios**, seleccione **Agregar** y escriba **Azure Digital Twins** en **Agregar acceso de API**. Si la búsqueda no encuentra la API, busque en su lugar **Azure Smart Spaces**. A continuación, seleccione **Conceder permisos > Permisos delegados** y **Listo**.

    [![Registros de aplicaciones de Azure Active Directory: agregar API](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Seleccione **Manifiesto** para abrir el manifiesto de aplicación para la aplicación. Establezca *oauth2AllowImplicitFlow* en `true`.

    [![Flujo implícito de Azure Active Directory](media/how-to-configure-postman/implicit-flow.png)](media/how-to-configure-postman/implicit-flow.png#lightbox)

1. Configure una **URL de respuesta** en `https://www.getpostman.com/oauth2/callback`.

    [![Dirección URL de respuesta de Azure Active Directory](media/how-to-configure-postman/reply-url.png)](media/how-to-configure-postman/reply-url.png#lightbox)

1. Copie y mantenga el valor de **Id. de aplicación** de la aplicación de Azure Active Directory. Se usa en los pasos siguientes.

## <a name="obtain-an-oauth-20-token"></a>Obtención de un token de OAuth 2.0

A continuación, instale y configure Postman para obtener un token de Azure Active Directory. Después, realice una solicitud HTTP autenticada a Azure Digital Twins mediante el token adquirido:

1. Vaya a [www.getpostman.com](https://www.getpostman.com/) para descargar la aplicación.
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
    | Dirección URL de autenticación | Use la **Dirección URL de autorización** del paso 2 |
    | Id. de cliente | Use el valor de **Id. de aplicación** para la aplicación de Azure Active Directory que se creó o se reutilizó en la sección anterior |
    | Ámbito | Déjelo en blanco |
    | Estado | Déjelo en blanco |
    | Autenticación de clientes | `Send as Basic Auth header` |

1. Ahora, el cliente debe aparecer como:

    [![Ejemplo de cliente de Postman](media/how-to-configure-postman/postman-oauth-token.png)](media/how-to-configure-postman/postman-oauth-token.png#lightbox)

1. Seleccione **Request Token** (Solicitar token).

    >[!TIP]
    >Si recibe el mensaje de error "No se pudo completar OAuth 2", pruebe lo siguiente:
    > * Cierre Postman, vuelva a abrirlo e inténtelo de nuevo.
  
1. Desplácese hacia abajo y seleccione **Use Token** (Usar token).

<div id="multi"></div>

## <a name="make-a-multipart-post-request"></a>Realizar una solicitud POST con varias partes

Después de completar los pasos anteriores, configure Postman para realizar una solicitud POST de varias partes HTTP autenticada:

1. En la pestaña **Encabezado**, agregue una clave de encabezado de solicitud HTTP **Content-Type** con el valor `multipart/mixed`.

   [![Tipo de contenido de varias partes o mixto](media/how-to-configure-postman/content-type.png)](media/how-to-configure-postman/content-type.png#lightbox)

1. Serialice los datos no textuales en archivos. Los datos JSON se guardarían como archivo JSON.
1. En la pestaña **Cuerpo**, agregue cada archivo asignando un nombre **clave** seleccionando `file` o `text`.
1. A continuación, seleccione cada archivo con el botón **Elegir archivo**.

   [![Ejemplo de cliente de Postman](media/how-to-configure-postman/form-body.png)](media/how-to-configure-postman/form-body.png#lightbox)

   >[!NOTE]
   > * El cliente de Postman no requiere que los fragmentos de varias partes tengan asignado manualmente **Content-Type** o **Content-Disposition**.
   > * No es necesario especificar esos encabezados para cada parte.
   > * Debe seleccionar `multipart/mixed` u otro **Content-Type** adecuado para toda la solicitud.

1. Por último, seleccione **Enviar** para enviar la solicitud HTTP POST con varias partes.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información acerca de las API de administración de Digital Twins y cómo usarlas, lea [How to use Azure Digital Twins management APIs](how-to-navigate-apis.md) (Uso de las API de administración de Azure Digital Twins).

- Use las solicitudes de varias partes para [agregar blobs a las entidades de Azure Digital Twins](./how-to-add-blobs.md).

- Para aprender sobre la autenticación con la API de administración, lea [Conexión y autenticación en las API](./security-authenticating-apis.md).