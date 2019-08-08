---
title: Personalización de notificaciones de token de SAML para aplicaciones empresariales en Azure AD | Microsoft Docs
description: Aprenda a personalizar las notificaciones emitidas en el token SAML para aplicaciones empresariales en Azure AD.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: ryanwi
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c0625a233b3b4a949feff2e289361a26fc8dc5a
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68835347"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Procedimientos para: Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales

Hoy en día, Azure Active Directory (Azure AD) admite el inicio de sesión único (SSO) con la mayoría de las aplicaciones empresariales, incluidas las aplicaciones previamente integradas en la galería de aplicaciones de Azure AD, así como las aplicaciones personalizadas. Cuando un usuario se autentica en una aplicación a través de Azure AD con el protocolo SAML 2.0, Azure AD envía un token a la aplicación (mediante HTTP POST). A continuación, la aplicación valida y usa el token para que el usuario inicie sesión en lugar de solicitar un nombre de usuario y una contraseña. Estos tokens SAML contienen trozos de información sobre el usuario conocidos como *notificaciones*.

Una *notificación* es información que un proveedor de identidades declara sobre un usuario dentro del token que se emite para dicho usuario. En [token SAML](https://en.wikipedia.org/wiki/SAML_2.0), estos datos suelen incluirse en la instrucción SAML Attribute. El identificador único del usuario suele representarse en SAML Subject, también denominado NameIdentifier.

De forma predeterminada, Azure AD emite un token SAML a la aplicación que contiene una notificación `NameIdentifier` con un valor de nombre de usuario del usuario (también denominado nombre principal de usuario) en Azure AD, que puede identificar al usuario de forma única. El token SAML también contiene notificaciones adicionales con la dirección de correo electrónico, el nombre y el apellido del usuario.

Para ver o editar las notificaciones emitidas en el token SAML a la aplicación, abra la aplicación en Azure Portal. Después, abra la sección **Atributos y notificaciones de usuario**.

![Apertura de la sección Atributos y notificaciones de usuario en Azure Portal](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

Hay dos razones posibles por las que tendría que editar las notificaciones emitidas en el token SAML:

* La aplicación requiere que la notificación `NameIdentifier` o NameID tenga un valor que no sea el del nombre de usuario (o nombre principal de usuario) almacenado en Azure AD.
* La aplicación se ha creado para requerir un conjunto diferente de URI o valores de notificación.

## <a name="editing-nameid"></a>Edición de NameID

Para editar la notificación NameID (valor de identificador de nombre):

1. Abra la página **Valor de identificador de nombre**.
1. Seleccione el atributo o la transformación que quiera aplicar al atributo. Si quiere, puede especificar el formato que quiere que tenga la notificación NameID.

   ![Edición del valor de la notificación NameID (identificador de nombre)](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>Formato de NameID

Si la solicitud SAML contiene el elemento NameIDPolicy con un formato específico, Azure AD respetará el formato en la solicitud.

Si la solicitud SAML no contiene ningún elemento para NameIDPolicy, Azure AD emitirá la notificación NameID con el formato que especifique. Si no se especifica ningún formato, Azure AD usará el formato de origen predeterminado asociado con el origen de notificación seleccionado.

En el menú desplegable **Elija el formato del identificador de nombre**, puede seleccionar una de las opciones siguientes.

| Formato de NameID | DESCRIPCIÓN |
|---------------|-------------|
| **Valor predeterminado** | Azure AD usará el formato de origen predeterminado. |
| **Persistent** | Azure AD usará Persistent como el formato de NameID. |
| **EmailAddress** | Azure AD usará EmailAddress como el formato de NameID. |
| **Unspecified** | Azure AD usará Unspecified como el formato de NameID. |
| **Transient** | Azure AD usará Transient como el formato de NameID. |

Para obtener más información sobre el atributo NameIDPolicy, consulte [Protocolo SAML de inicio de sesión único](single-sign-on-saml-protocol.md).

### <a name="attributes"></a>Atributos

Seleccione el origen que desee para la notificación `NameIdentifier` (o NameID). Puede seleccionar entre las opciones siguientes:

| NOMBRE | DESCRIPCIÓN |
|------|-------------|
| Email | Dirección de correo electrónico del usuario |
| userprincipalName | Nombre principal de usuario (UPN) del usuario. |
| onpremisessamaccount | Nombre de cuenta SAM que se ha sincronizado desde Azure AD local. |
| objectid | Valor de objectid del usuario en Azure AD. |
| employeeid | Valor de employeeid del usuario. |
| Sincronización de Azure AD Connect: Extensiones de directorio | Extensiones de directorio [sincronizadas desde Active Directory local con Azure AD Connect Sync](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| Atributos de extensión 1-15 | Los atributos de extensión locales usados para extender el esquema de AD Azure. |

Para obtener más información, consulte [Tabla 3: Valores de Id. válidos por origen](active-directory-claims-mapping.md#table-3-valid-id-values-per-source).

### <a name="special-claims---transformations"></a>Notificaciones especiales: transformaciones

También puede usar las funciones de transformaciones de notificaciones.

| Función | DESCRIPCIÓN |
|----------|-------------|
| **ExtractMailPrefix()** | Quita el sufijo de dominio de la dirección de correo electrónico o el nombre principal de usuario. De este modo se extrae solo la primera parte del nombre de usuario por la que se pasa (por ejemplo, "joe_smith" en lugar de joe_smith@contoso.com). |
| **Join()** | Combina un atributo con un dominio comprobado. Si el valor de identificador de usuario seleccionado tiene un dominio, extraerá el nombre de usuario para anexar el dominio comprobado seleccionado. Por ejemplo, si selecciona el correo electrónico (joe_smith@contoso.com) como valor de identificador de usuario y selecciona contoso.onmicrosoft.com como dominio comprobado, el resultado será joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Convierte los caracteres del atributo seleccionado en caracteres en minúscula. |
| **ToUpper()** | Convierte los caracteres del atributo seleccionado en caracteres en mayúscula. |

## <a name="adding-application-specific-claims"></a>Incorporación de notificaciones específicas de la aplicación

Para agregar notificaciones específicas de la aplicación:

1. En **Atributos y notificaciones de usuario**, seleccione **Agregar nueva notificación** para abrir la página **Administrar las notificaciones del usuario**.
1. Escriba el **nombre** de las notificaciones. El valor no tiene que seguir un patrón de URI estrictamente, de acuerdo con la especificación SAML. Si necesita un patrón de URI, puede colocarlo en el campo **Espacio de nombres**.
1. Seleccione el **origen** en el que la notificación va a recuperar su valor. Puede seleccionar un atributo de usuario en la lista desplegable de atributos de origen o aplicar una transformación al atributo de usuario antes de emitirlo como una notificación.

### <a name="application-specific-claims---transformations"></a>Notificaciones específicas de la aplicación: transformaciones

También puede usar las funciones de transformaciones de notificaciones.

| Función | DESCRIPCIÓN |
|----------|-------------|
| **ExtractMailPrefix()** | Quita el sufijo de dominio de la dirección de correo electrónico o el nombre principal de usuario. De este modo se extrae solo la primera parte del nombre de usuario por la que se pasa (por ejemplo, "joe_smith" en lugar de joe_smith@contoso.com). |
| **Join()** | Crea un nuevo valor al combinar dos atributos. Si quiere, puede usar un separador entre los dos atributos. |
| **ToLower()** | Convierte los caracteres del atributo seleccionado en caracteres en minúscula. |
| **ToUpper()** | Convierte los caracteres del atributo seleccionado en caracteres en mayúscula. |
| **Contains()** | Genera un atributo o una constante si la entrada coincide con el valor especificado. En caso contrario, puede especificar otra salida si no hay ninguna coincidencia.<br/>Por ejemplo, si quiere emitir una notificación en la que el valor es la dirección de correo electrónico del usuario si contiene el dominio "@contoso.com"; en caso contrario, quiere obtener el nombre principal de usuario. Para ello, configuraría los siguientes valores:<br/>*Parámetro 1 (entrada)* : user.email<br/>*Valor*: "@contoso.com"<br/>Parámetro 2 (salida): user.email<br/>Parámetro 3 (salida si no hay ninguna coincidencia): user.userprincipalname |
| **EndWith()** | Genera un atributo o una constante si la entrada finaliza con el valor especificado. En caso contrario, puede especificar otra salida si no hay ninguna coincidencia.<br/>Por ejemplo, si quiere emitir una notificación en la que el valor sea el employeeid del usuario si employeeid termina con "000"; en caso contrario, quiere obtener un atributo de extensión. Para ello, configuraría los siguientes valores:<br/>*Parámetro 1 (entrada)* : user.employeeid<br/>*Valor*: "000"<br/>Parámetro 2 (salida): user.employeeid<br/>Parámetro 3 (salida si no hay ninguna coincidencia): user.extensionattribute1 |
| **StartWith()** | Genera un atributo o una constante si la entrada empieza con el valor especificado. En caso contrario, puede especificar otra salida si no hay ninguna coincidencia.<br/>Por ejemplo, si quiere emitir una notificación en la que el valor sea el employeeid del usuario si el país o la región empieza con "US"; en caso contrario, quiere obtener un atributo de extensión. Para ello, configuraría los siguientes valores:<br/>*Parámetro 1 (entrada)* : user.country<br/>*Valor*: "US"<br/>Parámetro 2 (salida): user.employeeid<br/>Parámetro 3 (salida si no hay ninguna coincidencia): user.extensionattribute1 |
| **Extract(): después de la coincidencia** | Devuelve el valor de substring que aparece después de la coincidencia con el valor especificado.<br/>Por ejemplo, si el valor de la entrada es "Finance_BSimon", el valor coincidente es "Finance_" y, por lo tanto, el resultado de la notificación es "BSimon". |
| **Extract(): antes de la coincidencia** | Devuelve el valor de substring que aparece antes de la coincidencia con el valor especificado.<br/>Por ejemplo, si el valor de la entrada es "BSimon_US", el valor coincidente es "_US" y, por lo tanto, el resultado de la notificación es "BSimon". |
| **Extract(): entre coincidencias** | Devuelve el valor de substring que aparece antes de la coincidencia con el valor especificado.<br/>Por ejemplo, si el valor de la entrada es "Finance_BSimon_US", el valor coincidente es "Finance_", el segundo valor coincidente es "_US" y, por lo tanto, el resultado de la notificación es "BSimon". |
| **ExtractAlpha(): prefijo** | Devuelve la parte alfabética del prefijo de la cadena.<br/>Por ejemplo, si el valor de la entrada es "BSimon_123", devuelve "BSimon". |
| **ExtractAlpha(): sufijo** | Devuelve la parte alfabética del sufijo de la cadena.<br/>Por ejemplo, si el valor de la entrada es "123_Simon", devuelve "Simon". |
| **ExtractNumeric(): prefijo** | Devuelve la parte numérica del prefijo de la cadena.<br/>Por ejemplo, si el valor de la entrada es "123_BSimon", devuelve "123". |
| **ExtractNumeric(): sufijo** | Devuelve la parte numérica del sufijo de la cadena.<br/>Por ejemplo, si el valor de la entrada es "BSimon_123", devuelve "123". |
| **IfEmpty()** | Genera un atributo o una constante si la entrada es nula o está vacía.<br/>Por ejemplo, si quiere obtener un atributo que está almacenado en un valor de extensionattribute si el valor de employeeid de un usuario determinado está vacío. Para ello, configuraría los siguientes valores:<br/>Parámetro 1 (entrada): user.employeeid<br/>Parámetro 2 (salida): user.extensionattribute1<br/>Parámetro 3 (salida si no hay ninguna coincidencia): user.employeeid |
| **IfNotEmpty()** | Genera un atributo o una constante si la entrada no es nula ni está vacía.<br/>Por ejemplo, si quiere obtener un atributo que está almacenado en un valor de extensionattribute si el valor de employeeid de un usuario determinado no está vacío. Para ello, configuraría los siguientes valores:<br/>Parámetro 1 (entrada): user.employeeid<br/>Parámetro 2 (salida): user.extensionattribute1 |

Si necesita transformaciones adicionales, envíe su idea al [foro de comentarios de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599), en la categoría *Aplicación SaaS*.

## <a name="next-steps"></a>Pasos siguientes

* [Administración de aplicaciones en Azure AD](../manage-apps/what-is-application-management.md)
* [Configuración del inicio de sesión único en aplicaciones que no están en la galería de aplicaciones de Azure AD](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Solución de problemas del inicio de sesión único basado en SAML](howto-v1-debug-saml-sso-issues.md)
