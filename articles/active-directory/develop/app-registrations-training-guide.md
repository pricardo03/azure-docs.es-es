---
title: Registros de aplicaciones en la Guía de aprendizaje de Azure Portal - Azure
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870123"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>Guía de aprendizaje: registros de aplicaciones en Azure Portal  

Puede encontrar varias mejoras en la nueva experiencia de [registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) en Azure Portal. Si está más familiarizado con la experiencia heredada, use esta guía de aprendizaje para ayudarle a comenzar a usar la nueva experiencia.

## <a name="key-changes"></a>Cambios clave

- Los registros de aplicaciones no son solo una **aplicación web o API** o una aplicación **nativa**. Puede usar el mismo registro de aplicación para todas ellas al registrar los respectivos URI de redirección.
- La experiencia heredada admitía aplicaciones que inician sesión en cuentas de organización (Azure AD) solamente. Las aplicaciones que registraban como inquilino único (que admitían solo cuentas de organización desde el directorio en el que estaba registrada la aplicación) y se podían modificar para que sean multiempresa (que admiten todas las cuentas de organización). La nueva experiencia le permite registrar aplicaciones que admiten ambas opciones, así como una tercera opción: todas las cuentas de organización así como las cuentas de Microsoft personales.
- La experiencia heredada solo estaba disponible cuando se había iniciado sesión en Azure Portal con una cuenta de organización. Con la nueva experiencia, puede usar las cuentas de Microsoft personales que no están asociadas a un directorio.

## <a name="list-of-applications"></a>Lista de aplicaciones

- En la lista de nuevas aplicaciones se muestran las aplicaciones registradas a través de la experiencia de registro de aplicación heredada en Azure Portal (aplicaciones que inician sesión mediante cuentas de Azure AD), así como las aplicaciones registradas a través del [Portal de registro de aplicaciones](https://apps.dev.microsoft.com/) (aplicaciones que inician sesión mediante Azure AD y cuentas de Microsoft personales).
- La lista de nuevas aplicaciones no tiene una columna **Tipo de aplicación** (dado que un registro único de aplicación puede ser de varios tipos) y tiene dos columnas adicionales: una columna **Fecha de creación** y una columna **Certificados y secretos** en la que se muestra el estado (actual, expiración en breve o expirada) de las credenciales que se han registrado en la aplicación.

## <a name="new-app-registration"></a>Nuevo registro de aplicaciones

En la experiencia heredada, para registrar una aplicación era necesario proporcionar lo siguiente: **Nombre**, **Tipo de aplicación** y **URL de inicio de sesión/URI de redirección**. Las aplicaciones que se creaban eran aplicaciones de inquilino único de Azure AD solamente, por lo que solo admitían cuentas de organización del directorio en el que se había registrado la aplicación.

En la nueva experiencia, debe proporcionar un **nombre** para la aplicación y elegir los **tipos de cuenta admitidos**. También puede proporcionar un **URI de redirección**. Si proporciona un URI de redirección, tendrá que especificar si es web o público (móvil y escritorio). Para más información sobre cómo registrar una aplicación mediante la nueva experiencia de registros de aplicaciones, consulte esta [guía de inicio rápido](quickstart-register-app.md).

## <a name="the-legacy-properties-page"></a>Página de propiedades heredada

La experiencia heredada tenía una página **Propiedades** que no tiene la nueva experiencia. La hoja **Propiedades** tenía los siguientes campos: **Nombre**, **Id. de objeto**, **Id. de aplicación**, **URI de id. de aplicación**, **Logotipo**, **URL de página principal**, **URL de cierre de sesión**, **Dirección URL de Condiciones del servicio**, **URL de la declaración de privacidad**, **Tipo de aplicación** y **Multiempresa**.

Aquí es donde puede encontrar la funcionalidad equivalente en la nueva experiencia:

- **Nombre**, **Logotipo**, **URL de página principal**, **Dirección URL de Condiciones del servicio** y **URL de la declaración de privacidad** ahora están en la página **Personalización de marca**.
- **Id. de objeto** e **Id. de aplicación (cliente)** se encuentran en la página **Introducción**.
- La funcionalidad controlada por el botón **Multiempresa** de la experiencia heredada se reemplazó por la opción **Supported account types** (Tipos de cuenta admitidos) de la página **Autenticación**. Para más información sobre cómo multiempresa se asigna a las opciones de tipo de cuenta admitidos, consulte [esta guía de inicio rápido](quickstart-modify-supported-accounts.md).
- **URL de cierre de sesión** ahora está en la página **Autenticación**.
- **Tipo de aplicación** ya no es un campo válido. En su lugar, los URI de redirección (en la página **Autenticación**) determinan qué tipos de aplicaciones se admiten.
- **URI de id. de aplicación** **** ahora se encuentra en la hoja **Exponer una API**. En la experiencia heredada, esta propiedad se registraba automáticamente con el formato siguiente: `https://{tenantdomain}/{appID}` (por ejemplo, `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). En el nuevo formato, se genera automáticamente como `api://{appID}`, pero se debe guardar explícitamente.

## <a name="reply-urlsredirect-urls"></a>URL de respuesta y URI de redirección

En la experiencia heredada, una aplicación tenía una página **URL de redirección**. En la nueva experiencia, las direcciones URL de respuesta se encuentran en la sección **Autenticación** de una aplicación. Además, se conocen como **URI de redirección**. Asimismo, ha cambiado el formato de los URI de redirección. Estos deben estar asociados con un tipo de aplicación (web o pública). Además, por motivos de seguridad, los caracteres comodín y los esquemas http:// no se admiten (con la excepción de http://localhost).

## <a name="keyscertificates--secrets"></a>Claves y certificados y secretos

En la experiencia heredada, una aplicación tenía una página **Claves**. En la nueva experiencia, su nombre se cambió a **Certificados y secretos**. Además, las **claves públicas** se conocen como **certificados** y las **contraseñas** se conocen como **secretos de cliente**.

## <a name="required-permissionsapi-permissions"></a>Permisos obligatorios y permisos de API

- En la experiencia heredada, una aplicación tenía una página **Permisos necesarios**. En la nueva experiencia, el nombre cambió a **Permisos de API**.
- Al seleccionar una API en la experiencia heredada, podía elegir entre una pequeña lista de API de Microsoft o buscar en entidades de servicio en el inquilino. En la nueva experiencia, puede elegir entre varias pestañas: **API de Microsoft**, **API usadas en mi organización** o **Mis API**. La barra de búsqueda de la pestaña **API usadas en mi organización** busca por entidades de servicio en el inquilino. 

   > [!NOTE]
   > Si la aplicación no está asociada a un inquilino, no verá esta pestaña. Para más información sobre cómo solicitar permisos mediante la nueva experiencia, consulte esta [guía de inicio rápido](quickstart-configure-app-access-web-apis.md).

- La experiencia heredada tenía un botón **Conceder permisos** situado en la parte superior de la página **Permisos solicitados**. En la nueva experiencia, hay una sección **Otorgar consentimiento** con un botón **Conceder consentimiento de administrador** en la sección **Permisos de API** de una aplicación. Además, hay algunas diferencias en el funcionamiento de los botones:
   - En la experiencia heredada, la lógica variaba según el usuario que había iniciado sesión y los permisos que se solicitan. La lógica era la siguiente:
      - Si solo se solicitaban permisos capaz de consentimiento del usuario y el usuario que había iniciado sesión no era administrador, el usuario podía conceder consentimiento del usuario para los permisos solicitados.
      - Si se solicitaba al menos un permiso que requería el consentimiento del administrador y el usuario que había iniciado sesión no era administrador, el usuario recibía un error al intentar conceder consentimiento.
      - Si el usuario que había iniciado sesión era administrador, se concedía el consentimiento del administrador para todos los permisos solicitados.
   - En la nueva experiencia, solo un administrador puede conceder consentimiento. Cuando un administrador selecciona el botón **Conceder consentimiento de administrador**, el consentimiento del administrador se concede a todos los permisos solicitados.

## <a name="deleting-an-app-registration"></a>Eliminar un registro de aplicación

En la experiencia heredada, una aplicación tenía que ser un inquilino único para poder eliminarse. El botón Eliminar estaba deshabilitado para aplicaciones multiempresa. En la nueva experiencia, las aplicaciones se pueden eliminar en cualquier estado, pero debe confirmar la acción. Para más información sobre la eliminación de registros de aplicaciones, consulte esta [guía de inicio rápido](quickstart-remove-app.md).

## <a name="application-manifest"></a>Manifiesto de aplicación

La experiencia heredada y las nuevas usan versiones diferentes para el formato de JSON en el editor de manifiestos. Para más información, consulte [Manifiesto de aplicación](reference-app-manifest.md).

## <a name="new-ui"></a>Nueva interfaz de usuario

Hay una nueva interfaz de usuario para las propiedades que anteriormente solo se podían establecer mediante el editor de manifiestos o la API, o que no existían.

- La opción **Implicit grant flow** (Flujo de concesión implícita) (oauth2AllowImplicitFlow) se encuentra en la página **Autenticación**. A diferencia de la experiencia heredada, puede habilitar **tokens de acceso** o **tokens de identificador**, o ambos.
- Las opciones **Ámbitos definidos con esta API** (oauth2Permissions) y **Aplicaciones cliente autorizadas** (preAuthorizedApplications) se pueden configurar a través de la página **Exponer una API**. Para más información sobre cómo configurar una aplicación para que sea una API web y exponer permisos o ámbitos, consulte esta [guía de inicio rápido](quickstart-configure-app-expose-web-apis.md).
- La opción **Dominio del publicador** (que se muestra a los usuarios en la [petición de consentimiento de la aplicación](application-consent-experience.md)) se encuentra en la página **Personalización de marca**. Para más información sobre cómo configurar un dominio de publicador, consulte este [artículo sobre procedimientos](howto-configure-publisher-domain.md).

## <a name="limitations"></a>Limitaciones

La nueva experiencia tiene las siguientes limitaciones:

- Actualmente, la nueva experiencia no está disponible en los inquilinos de Azure AD B2C.
- El formato de los secretos de cliente (contraseñas de aplicación) es diferente de la experiencia heredada e interrumpe la CLI.
- En la interfaz de usuario no se admite el cambio del valor para las cuentas admitidas. Debe usar el manifiesto de aplicación, a menos que se va a cambiar entre Azure AD de inquilino único y multiempresa.
