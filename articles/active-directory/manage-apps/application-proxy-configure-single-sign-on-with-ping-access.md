---
title: Autenticación basada en encabezados con PingAccess para el proxy de la aplicación de Azure AD | Microsoft Docs
description: Publique aplicaciones con PingAccess y el proxy de la aplicación que admitan la autenticación basada en encabezados.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 365f017fe7d71500c17d0a9ccd9c5a0a26a78b75
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989590"
---
# <a name="header-based-authentication-for-single-sign-on-with-application-proxy-and-pingaccess"></a>Autenticación basada en el encabezado para el inicio de sesión único con el proxy de aplicación y PingAccess

Azure Active Directory (Azure AD) Application Proxy se ha asociado con PingAccess para que los clientes de Azure AD pueden tener acceso a más de sus aplicaciones. PingAccess expande las [ofertas actuales del proxy de aplicación](application-proxy.md) para incluir el acceso de inicio sesión único a las aplicaciones que utilicen encabezados para la autenticación.

## <a name="whats-pingaccess-for-azure-ad"></a>¿Qué es PingAccess para Azure AD?

Con PingAccess para Azure AD, puede dar acceso a los usuarios y el inicio de sesión único (SSO) a las aplicaciones que utilizan encabezados para la autenticación. El proxy de aplicación trata estas aplicaciones como a las demás, usa Azure AD para autenticar el acceso y, después, pasa el tráfico a través del servicio de conector. PingAccess se coloca delante de las aplicaciones y el token de acceso de Azure AD traduce en un encabezado. A continuación, la aplicación recibe la autenticación en el formato que pueda leer.

Los usuarios no notarán ninguna diferencia al iniciar sesión para usar las aplicaciones corporativas. Podrán seguir trabajando desde cualquier lugar y en cualquier dispositivo. Los conectores del Proxy de aplicación dirigen el tráfico remoto a todas las aplicaciones sin tener en cuenta su tipo de autenticación, por lo que deberá seguir equilibrar las cargas de automáticamente.

## <a name="how-do-i-get-access"></a>¿Cómo puedo obtener acceso?

Puesto que este escenario proviene de una asociación entre Azure Active Directory y PingAccess, necesita licencias para ambos servicios. Sin embargo, las suscripciones Premium de Azure Active Directory incluyen una licencia básica de PingAccess que abarca hasta 20 aplicaciones. Si tiene que publicar más de 20 aplicaciones basadas en el encabezado, puede adquirir una licencia adicional de PingAccess.

Para obtener más información, consulte [Ediciones de Azure Active Directory](../fundamentals/active-directory-whatis.md).

## <a name="publish-your-application-in-azure"></a>Publicación de una aplicación en Azure

En este artículo es para que las personas publicar una aplicación con este escenario por primera vez. Además de que se detallan los pasos de publicación, le guía en la introducción a Proxy de aplicación y PingAccess. Si ya ha configurado ambos servicios, pero desea revisar los pasos de publicación, vaya a la [agregar la aplicación a Azure AD con el Proxy de aplicación](#add-your-application-to-azure-ad-with-application-proxy) sección.

> [!NOTE]
> Dado que este escenario es una asociación entre Azure AD y PingAccess, algunas de las instrucciones se encuentran en el sitio de Ping Identity.

### <a name="install-an-application-proxy-connector"></a>Instalación de un conector del proxy de la aplicación

Si ha habilitado el Proxy de aplicación habilitado y ya instalado un conector, puede omitir esta sección y vaya a [agregar la aplicación a Azure AD con el Proxy de aplicación](#add-your-application-to-azure-ad-with-application-proxy).

El conector del Proxy de aplicación es un servicio de Windows Server que dirige el tráfico de los empleados remotos a las aplicaciones publicadas. Para obtener instrucciones de instalación detalladas, consulte [Tutorial: Agregar una aplicación de forma local para el acceso remoto a través del Proxy de aplicación en Azure Active Directory](application-proxy-add-on-premises-application.md).

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) como administrador de aplicaciones. El **centro de administración de Azure Active Directory** aparece la página.
2. Seleccione **Azure Active Directory** > **proxy de aplicación** > **descargar servicio de conector**. El **Application Proxy Connector Download** aparece la página.

   ![Descarga del conector de proxy de aplicación](./media/application-proxy-configure-single-sign-on-with-ping-access/application-proxy-connector-download.png)
3. Siga las instrucciones de instalación.

Descargar el conector debería habilitar automáticamente el Proxy de aplicación para su directorio, pero si no, puede seleccionar **habilitación del Proxy de aplicación**.

### <a name="add-your-application-to-azure-ad-with-application-proxy"></a>Agregar la aplicación a Azure AD con el Proxy de aplicación

Hay dos acciones que se deben tomar en Azure Portal. En primer lugar, debe publicar la aplicación con el proxy de aplicación. A continuación, deberá recopilar información acerca de la aplicación que puede usar en los pasos de PingAccess.

#### <a name="publish-your-application"></a>Publicación de la aplicación

En primer lugar, deberá publicar su aplicación. Esta acción implica:

- Agregar la aplicación en el entorno local a Azure AD
- Asignar a un usuario para probar la aplicación y seleccionar SSO basado en encabezados
- Configuración de dirección URL de redireccionamiento de la aplicación
- Conceder permisos para los usuarios y otras aplicaciones para usar la aplicación en el entorno local

Para publicar su propia aplicación de forma local:

1. Si no lo hizo en la última sección, inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) como administrador de aplicaciones.
2. Seleccione **aplicaciones empresariales** > **nueva aplicación** > **aplicación local**. El **agregar su propia aplicación en el entorno local** aparece la página.

   ![Agregar aplicación local propia](./media/application-proxy-configure-single-sign-on-with-ping-access/add-your-own-on-premises-application.png)
3. Rellene los campos obligatorios con información sobre la nueva aplicación. Utilice las instrucciones siguientes para la configuración.

   > [!NOTE]
   > Para obtener un tutorial más detallado de este paso, consulte [agregar una aplicación local a Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).

   1. **Dirección URL interna**: Normalmente, especifica la dirección URL que lleva a la página de inicio de sesión de la aplicación cuando esté en la red corporativa. Para este escenario, el conector debe tratar al proxy PingAccess como la página principal de la aplicación. Use este formato: `https://<host name of your PingAccess server>:<port>`. El puerto predeterminado es 3000, pero puedo configurar uno distinto en PingAccess.

      > [!WARNING]
      > Para este tipo de inicio de sesión único, debe usar la dirección URL interna `https` y no se puede usar `http`.

   2. **Método de autenticación previa**: Elija **Azure Active Directory**.
   3. **Traducir URL en encabezados**: Elija **No**.

   > [!NOTE]
   > Si esta es su primera aplicación, utilice el puerto 3000 para empezar y regrese para actualizar este valor si cambia la configuración de PingAccess. Para las siguientes aplicaciones, el puerto debe coincidir con el agente de escucha que ha configurado en PingAccess. Obtenga más información sobre [escucha en PingAccess](https://documentation.pingidentity.com/pingaccess/pa31/index.shtml#Listeners.html).
4. Seleccione **Agregar**. Aparece la página de información general de la nueva aplicación.

Ahora, asignar a un usuario de prueba de la aplicación y elija basada en el encabezado de sesión único:

1. En la barra lateral de la aplicación, seleccione **usuarios y grupos** > **Agregar usuario** > **usuarios y grupos (\<número > seleccionados)** . Aparece una lista de usuarios y grupos para que los que elegir.

   ![Usuarios y grupos](./media/application-proxy-configure-single-sign-on-with-ping-access/users-and-groups.png)
2. Seleccione un usuario de prueba de la aplicación y seleccione **seleccione**. Asegúrese de que esta cuenta de prueba tiene acceso a la aplicación local.
3. Seleccione **Asignar**.
4. En la barra lateral de la aplicación, seleccione **inicio de sesión único** > **basada en encabezados**.

   > [!TIP]
   > Si se trata de la primera vez que usa el inicio de sesión único basado en el encabezado, debe instalar PingAccess. Para asegurarse de que su suscripción de Azure se asocia automáticamente con la instalación de PingAccess, utilice el vínculo de esta página de inicio de sesión único para descargar PingAccess. Puede abrir el sitio de descarga ahora o volver a esta página más adelante.

   ![Inicio de sesión basado en el encabezado](./media/application-proxy-configure-single-sign-on-with-ping-access/sso-header.png)
5. Seleccione **Guardar**.

A continuación, asegúrese de que la redirección de que URL se establece en la dirección URL externa:

1. Desde el **centro de administración de Azure Active Directory** barra lateral, seleccione **Azure Active Directory** > **registros de aplicaciones**. Aparece una lista de aplicaciones.

   ![Registros de aplicaciones](./media/application-proxy-configure-single-sign-on-with-ping-access/app-registrations.png)
2. Seleccione la aplicación.
3. Seleccione el vínculo junto a **URI de redirección**, que muestra el número de redirección URI configuración para la web y los clientes públicos. El  **\<nombre de aplicación >-autenticación** aparece la página.
4. Compruebe si la dirección URL externa que asignó a la aplicación anteriormente en el **URI de redirección** lista. Si no lo está, agregue la dirección URL externa ahora, con un tipo URI de redireccionamiento de **Web**y seleccione **guardar**.

Por último, configurar la aplicación en el entorno local para que los usuarios tienen acceso de lectura y otras aplicaciones tienen acceso de lectura/escritura:

1. Desde el **registros de aplicaciones** barra lateral de la aplicación, seleccione **permisos de API** > **agregar un permiso**  >   **API de Microsoft** > **Microsoft Graph**. El **permisos de solicitud API** página **Microsoft Graph** aparece, que contiene las API de Windows Azure Active Directory.

   ![Solicitud de permisos de API](./media/application-proxy-configure-single-sign-on-with-ping-access/required-permissions.png)
2. Seleccione **permisos delegados** > **usuario** > **User.Read**.
3. Seleccione **permisos de la aplicación** > **aplicación** > **Application.ReadWrite.All**.
4. Seleccione **agregar permisos**.
5. En el **permisos de API** página, seleccione **conceder consentimiento del administrador para \<el nombre de directorio >**.

#### <a name="collect-information-for-the-pingaccess-steps"></a>Recopilación de información para los pasos de PingAccess

Es preciso recopilar estos tres tipos de información (todos los GUID) para configurar la aplicación con PingAccess:

| Nombre del campo de Azure AD | Nombre del campo de PingAccess | Formato de datos |
| --- | --- | --- |
| **Id. de aplicación (cliente)** | **Id. de cliente** | GUID |
| **Id. de directorio (inquilino)** | **Issuer** | GUID |
| `PingAccess key` | **Secreto de cliente** | Cadena aleatoria |

Para recopilar esta información:

1. Desde el **centro de administración de Azure Active Directory** barra lateral, seleccione **Azure Active Directory** > **registros de aplicaciones**. Aparece una lista de aplicaciones.
2. Seleccione la aplicación. El **registros de aplicaciones** página aparece de la aplicación.

   ![Introducción al registro de una aplicación](./media/application-proxy-configure-single-sign-on-with-ping-access/registration-overview-for-an-application.png)
3. Junto a la **Id. de aplicación (cliente)** valor, seleccione el **copiar al Portapapeles** icono, a continuación, copie y guárdela. Especifique este valor más adelante como identificador de cliente. de PingAccess
4. Siguiente el **Id. de directorio (inquilino)** de valor, seleccione también **copiar al Portapapeles**, a continuación, copie y guárdela. Especifique este valor más adelante como emisor de PingAccess.
5. En la barra lateral de la **registros de aplicaciones** para su aplicación, seleccione **certificados y secretos** > **nuevo secreto de cliente**. El **agregar un secreto de cliente** aparece la página.

   ![Agregar un secreto de cliente](./media/application-proxy-configure-single-sign-on-with-ping-access/add-a-client-secret.png)
6. En **descripción**, tipo `PingAccess key`.
7. En **Expires**, seleccione cómo especificar la clave de PingAccess: **En 1 año**, **en 2 años**, o **nunca**.
8. Seleccione **Agregar**. El PingAccess clave aparece en la tabla de secretos de cliente, con uno aleatorio cadena que se rellena automáticamente en el **valor** campo.
9. Junto a la clave de PingAccess **valor** campos, seleccione el **copiar al Portapapeles** icono, a continuación, copie y guárdela. Especifique este valor más adelante como secreto de cliente de PingAccess.

### <a name="update-graphapi-to-send-custom-fields-optional"></a>Actualice GraphAPI para enviar los campos personalizados (opcional)

Para obtener una lista de tokens de seguridad que Azure AD envía para la autenticación, consulte [tokens de identificador de plataforma de identidad de Microsoft](../develop/id-tokens.md). Si necesita una notificación personalizada que envíe otros tokens, establezca el `acceptMappedClaims` campo de la aplicación a `True`. Puede utilizar el Explorador de Graph o manifiesto de aplicación del portal de Azure AD para realizar este cambio.

En este ejemplo se usa el Explorador de Graph:

```
PATCH https://graph.windows.net/myorganization/applications/<object_id_GUID_of_your_application>

{
  "acceptMappedClaims":true
}
```

Este ejemplo se usa el [portal de Azure Active Directory](https://aad.portal.azure.com/) para actualizar el `acceptMappedClaims` campo:

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) como administrador de aplicaciones.
2. Seleccione **Azure Active Directory** > **Registros de aplicaciones**. Aparece una lista de aplicaciones.
3. Seleccione la aplicación.
4. En la barra lateral de la **registros de aplicaciones** página de la aplicación, seleccione **manifiesto**. Aparece el código JSON de manifiesto para el registro de la aplicación.
5. Busque el `acceptMappedClaims` campo y cambie el valor a `True`.
6. Seleccione **Guardar**.

### <a name="use-a-custom-claim-optional"></a>Usar una notificación personalizada (opcional)

Para que la aplicación use una notificación personalizada e incluya campos adicionales, asegúrese de haber también [crea una directiva de asignación de notificaciones personalizadas y ha asignado a la aplicación](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

> [!NOTE]
> Para usar una notificación personalizada, también debe tener una directiva personalizada definida y asignada a la aplicación. Esta directiva debe incluir todos los atributos personalizados necesarios.
>
> Puede hacer la definición de directiva y la asignación a través de PowerShell, Azure AD Graph Explorer o Microsoft Graph. Si va a realizar ellos en PowerShell, debe usar primero `New-AzureADPolicy` y, a continuación, asignarlo a la aplicación con `Add-AzureADServicePrincipalPolicy`. Para obtener más información, consulte [asignación de directiva de asignación de notificaciones](../develop/active-directory-claims-mapping.md#claims-mapping-policy-assignment).

## <a name="download-pingaccess-and-configure-your-application"></a>Descarga de PingAccess y configuración de la aplicación

Ahora que ha completado todos los pasos de instalación de Azure Active Directory, puede pasar a configurar PingAccess.

Seguir los pasos detallados para la parte de PingAccess de este escenario en la documentación de Ping Identity. Siga las instrucciones de [configurar PingAccess para Azure AD proteger las aplicaciones publicadas mediante Microsoft Azure AD Application Proxy](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html) en el sitio web de Ping Identity.

Esos pasos le permiten instalar PingAccess y configurar una cuenta de PingAccess (si aún no tiene uno). A continuación, para crear una conexión de Azure AD OpenID Connect (OIDC), configura un proveedor de tokens con el **Id. de directorio (inquilino)** que copió desde el portal de Azure AD. A continuación, para crear una sesión web en PingAccess, utilice el **Id. de aplicación (cliente)** y `PingAccess key` valores. Después, puede configurar la asignación de identidades y crear un host virtual, un sitio y una aplicación.

### <a name="test-your-application"></a>Prueba de la aplicación

Cuando haya completado todos estos pasos, la aplicación debe ser en marcha. Para probarla, abra un explorador y vaya a la dirección URL externa que creó al publicar la aplicación en Azure. Inicie sesión con la cuenta de prueba que asignó a la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [Configurar PingAccess para Azure AD proteger las aplicaciones publicadas mediante el Proxy de aplicación de Microsoft Azure AD](https://docs.pingidentity.com/bundle/paaad_m_ConfigurePAforMSAzureADSolution_paaad43/page/pa_c_PAAzureSolutionOverview.html)
- [Inicio de sesión único en aplicaciones en Azure Active Directory](what-is-single-sign-on.md)
- [Solución de problemas y mensajes de error de Proxy de aplicación](application-proxy-troubleshoot.md)
