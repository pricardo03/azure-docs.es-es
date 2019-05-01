---
title: Registros de aplicaciones en la Guía de formación en Azure portal - Azure
description: Cree flujos de autenticación sin explorador e insertados mediante el uso de la concesión de código de dispositivo.
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 118c6ecb16d325a384246a0b3d9e685f6f6f04ee
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870123"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>Guía de aprendizaje: Registros de aplicaciones en Azure portal  

Puede encontrar numerosas mejoras en el nuevo [registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) experiencia en el portal de Azure. Si está más familiarizado con la experiencia heredada, use esta guía de aprendizaje para ayudarle a comenzar a usar la nueva experiencia.

## <a name="key-changes"></a>Cambios de clave

- Registros de aplicaciones no se limitan a ser cualquiera un **aplicación web o API** o un **nativo** app. Puede usar el mismo registro de aplicación para todos ellos mediante el registro de los URI de redireccionamiento respectivo.
- Las aplicaciones de la experiencia heredada compatible que inicie sesión en solo cuentas de organización (Azure AD). Las aplicaciones que se registraron como único inquilino (compatibles con cuentas de organización solo desde el directorio que se registró la aplicación) y podría modificarse para ser apto para multiempresa (compatibles con todas las cuentas de organización). La nueva experiencia le permite registrar las aplicaciones que pueden admitir ambos esas opciones, así como una tercera opción: todas las cuentas de la organización así como cuentas personales de Microsoft.
- La experiencia heredada solo estaba disponible cuando se ha iniciado sesión en el portal de Azure con una cuenta de organización. Con la nueva experiencia, puede usar las cuentas personales de Microsoft que no están asociadas con un directorio.

## <a name="list-of-applications"></a>Lista de aplicaciones

- Las aplicaciones que se registran a través de la aplicación heredada muestra la lista de aplicaciones nueva experiencia de los registros en el portal de Azure (aplicaciones que inician sesión en cuentas de Azure AD), así como las aplicaciones aunque registrado el [Portal de registro de aplicación](https://apps.dev.microsoft.com/) (aplicaciones que inician sesión en Azure AD y las cuentas personales de Microsoft).
- La nueva lista de aplicaciones no tiene un **tipo de aplicación** columna (dado que un solo registro de aplicación puede ser varios tipos) y tiene dos columnas adicionales: un **creado en** columna y un **certificados & secretos** columna que muestra el estado (actual, con expiración en breve o expirada) de las credenciales que se han registrado en la aplicación.

## <a name="new-app-registration"></a>Nuevo registro de aplicación

En la experiencia heredada, para registrar una aplicación era necesario para proporcionar: **Nombre**, **tipo de aplicación**, y **inicio de sesión en el URI de redirección deURL/**. Las aplicaciones que se crearon eran aplicaciones solo un único inquilino de Azure AD lo que significa que solo admite cuentas de organización desde el directorio que se registró la aplicación.

En la nueva experiencia, debe proporcionar un **nombre** para la aplicación y elija el **admite tipos de cuenta**. También puede proporcionar un **URI de redirección**. Si proporciona un URI de redirección, deberá especificar si se trata de web/público (escritorio y móvil). Para obtener más información sobre cómo registrar una aplicación mediante los registros de aplicaciones nueva experiencia, consulte [este inicio rápido](quickstart-register-app.md).

## <a name="the-legacy-properties-page"></a>La página de propiedades heredada

La experiencia heredada tenía un **propiedades** página que no tiene la nueva experiencia. El **propiedades** hoja tenía los siguientes campos: **Nombre**, **Id. de objeto**, **Id. de aplicación**, **App ID URI**, **logotipo**, **URL de página principal** , **Dirección URL de cierre de sesión**, **términos de la dirección URL del servicio**, **URL de la declaración de privacidad**, **tipo de aplicación**, y  **Varios inquilinos.**

Aquí es donde puede encontrar la funcionalidad equivalente en la nueva experiencia:

- **Nombre**, **logotipo**, **URL de página principal**, **términos de la dirección URL del servicio**, y **URL de la declaración de privacidad** ahora está en la aplicación  **Personalización de marca** página.
- **Id. de objeto** y **Id. de aplicación (cliente)** está activado el **Introducción** página.
- La funcionalidad controlada por el **multiinquilino** alternar en la experiencia heredada se ha reemplazado por **admite tipos de cuenta** en el **autenticación** página. Para obtener más información acerca de cómo se asigna varios inquilinos a las opciones de tipo de cuenta compatibles, consulte [este inicio rápido](quickstart-modify-supported-accounts.md).
- **Dirección URL de cierre de sesión** ahora está en el **autenticación** página.
- **Tipo de aplicación** ya no es un campo válido. En su lugar, el URI de redireccionamiento (que encontrará en el **autenticación** página) determinar qué tipos de aplicaciones son compatibles.
- **URI de Id. de aplicación** ahora se denomina **URI de Id. de aplicación** y puede encontrarlo en el **exponer una API** hoja. En la experiencia heredada, esta propiedad se registrarse automáticamente con el formato siguiente: `https://{tenantdomain}/{appID}` (por ejemplo, `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). En el nuevo formato, es generado automáticamente como `api://{appID}`, pero tienen que guardarse explícitamente.

## <a name="reply-urlsredirect-urls"></a>Direcciones URL/redirect URL de respuesta

En la experiencia heredada, una aplicación tenía un **direcciones URL de respuesta** página. En la nueva experiencia, las direcciones URL de respuesta puede encontrarse en una aplicación **autenticación** sección. Además, se conocen como **URI de redirección**. Además, ha cambiado el formato para redirigir el URI. Son necesarios para asociarse con un tipo de aplicación (web o público). Además, por motivos de seguridad, los caracteres comodín y http:// no se admiten esquemas (con la excepción de http://localhost).

## <a name="keyscertificates--secrets"></a>Los certificados o claves y secretos

En la experiencia heredada, una aplicación debía **claves** página. En la nueva experiencia, se ha cambiado a **certificados y secretos**. Además, **claves públicas** se conocen como **certificados** y **contraseñas** se conocen como **los secretos de cliente**.

## <a name="required-permissionsapi-permissions"></a>Permisos de los permisos necesarios y de API

- En la experiencia heredada, una aplicación tenía un **permisos necesarios** página. En la nueva experiencia, se ha cambiado a **permisos de API**.
- Al seleccionar una API en la experiencia heredada, puede elegir entre una lista reducida de Microsoft APIs o realizar búsquedas en las entidades de servicio en el inquilino. En la nueva experiencia, puede elegir entre varias pestañas: **Microsoft APIs**, **API que usa mi organización**, o **Mis API**. En la barra de búsqueda **las API de mi organización** usa búsquedas de pestaña a través de las entidades de servicio en el inquilino. 

   > [!NOTE]
   > Si la aplicación no está asociada a un inquilino no podrá ver esta pestaña. Para obtener más información sobre cómo solicitar permisos mediante la nueva experiencia, consulte [este inicio rápido](quickstart-configure-app-access-web-apis.md).

- La experiencia heredada tenía un **conceder permisos** situado en la parte superior de la **permisos solicitados** página. En la nueva experiencia, hay un **conceder consentimiento** sección con un **conceder consentimiento del administrador** botón en una aplicación **permisos de API** sección. Además, hay algunas diferencias en las formas de la función de los botones:
   - En la experiencia heredada, la lógica varía según el usuario con sesión iniciada y los permisos que se solicita. La lógica era:
      - Si solo se solicitan permisos capaz de consentimiento de usuario y el usuario con sesión iniciada no era administrador, el usuario pudo conceder consentimiento del usuario para los permisos solicitados.
      - Si se solicitaba al menos un permiso que requiere el consentimiento del administrador y el usuario con sesión iniciada no era administrador, el usuario recibe un error al intentar conceder consentimiento.
      - Si el usuario ha iniciado sesión era administrador, se concedió el consentimiento de administrador para todos los permisos solicitados.
   - En la nueva experiencia, solo un administrador puede conceder consentimiento. Cuando se selecciona un administrador la **conceder consentimiento del administrador** botón, el consentimiento del administrador se concede a todos los permisos solicitados.

## <a name="deleting-an-app-registration"></a>Eliminar un registro de aplicación

En la experiencia heredada, una aplicación tenía que ser un único inquilino va a eliminar. Se deshabilitó el botón Eliminar para aplicaciones de varios inquilinos. En la nueva experiencia, las aplicaciones pueden eliminarse en cualquier estado, pero debe confirmar la acción. Para obtener más información acerca de cómo eliminar los registros de aplicaciones, consulte [este inicio rápido](quickstart-remove-app.md).

## <a name="application-manifest"></a>Manifiesto de aplicación

Las experiencias anteriores y nuevas utilizan versiones diferentes para el formato de JSON en el editor de manifiestos. Para más información, consulte [Manifiesto de aplicación](reference-app-manifest.md).

## <a name="new-ui"></a>Nueva interfaz de usuario

Hay una nueva interfaz de usuario para las propiedades que anteriormente sólo se podía establecer mediante el editor de manifiestos o la API o no existían.

- **Flujo de concesión implícita** (oauth2AllowImplicitFlow) puede encontrarse en el **autenticación** página. A diferencia de la experiencia heredada, puede habilitar **tokens de acceso** o **los tokens de identificador**, o ambos.
- **Ámbitos definen con esta API** (oauth2Permissions) y **las aplicaciones cliente autorizadas** (preAuthorizedApplications) pueden configurarse a través de la **exponer una API** página. Para obtener más información sobre cómo configurar una aplicación para que una API web y exponer o ámbitos de permisos, consulte [este inicio rápido](quickstart-configure-app-expose-web-apis.md).
- **Dominio del publicador** (que se muestra a los usuarios en el [de consentimiento de la aplicación](application-consent-experience.md)) puede encontrarse en el **hoja de personalización de marca** página. Para obtener más información sobre cómo configurar un dominio del publicador, vea [este tema de procedimientos](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Limitaciones

La nueva experiencia tiene las siguientes limitaciones:

- Actualmente, la nueva experiencia no está disponible en los inquilinos de Azure AD B2C.
- El formato de los secretos de cliente (las contraseñas de aplicación) es diferente de la experiencia heredada y saltos de CLI.
- No se admite cambiar el valor para las cuentas admitidas en la interfaz de usuario. Deberá usar el manifiesto de aplicación a menos que se va a cambiar entre Azure AD del inquilino único y varios inquilinos.
