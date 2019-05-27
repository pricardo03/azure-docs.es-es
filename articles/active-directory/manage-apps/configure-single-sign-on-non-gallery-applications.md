---
title: Inicio de sesión único - ajena a la Galería de aplicaciones - plataforma Microsoft identity | Microsoft Docs
description: Configurar el inicio de sesión único (SSO) a ajena a la Galería aplicaciones en la plataforma de identidad de Microsoft (Azure AD)
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: article
ms.workload: identity
ms.date: 05/08/2019
ms.author: celested
ms.reviewer: arvinh,luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: e34e6257b4800387470cdc1b7d624bf3ebd1d3e6
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65989239"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-microsoft-identity-platform"></a>Configurar inicio de sesión único para aplicaciones que no sean de la galería en la plataforma Microsoft identity

Este artículo trata sobre una característica que permite a los administradores configurar el inicio de sesión único para las aplicaciones que no se encuentra en la Galería de aplicaciones de plataforma de identidad de Microsoft *sin escribir código*.

Si por el contrario desea obtener instrucciones para desarrolladores sobre cómo integrar aplicaciones personalizadas con Azure AD a través de código, consulte [Escenarios de autenticación para Azure AD](../develop/authentication-scenarios.md).

La Galería de aplicaciones de plataforma de identidad de Microsoft proporciona una lista de aplicaciones que se sabe que admiten un formulario de inicio de sesión único con la plataforma de identidad de Microsoft, como se describe en [en este artículo](what-is-single-sign-on.md). Una vez que un especialista en TI o un integrador de sistemas de la organización ha encontrado la aplicación que desea conectar, puede empezar por seguir las instrucciones detalladas que se proporcionan en Azure Portal para habilitar el inicio de sesión único.

Las capacidades siguientes también están disponibles, según el contrato de licencia. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/active-directory/)para obtener más información.

- Integración de autoservicio de una aplicación que use un protocolo modernos como [OpenId Connect y OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) para autenticar sus usuarios y obtener tokens para [Microsoft Graph](https://graph.microsoft.com).
- Integración de autoservicio de cualquier aplicación que admita [lenguaje de marcado de aserción de seguridad (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) proveedores de identidades (iniciado por SP o iniciado por IdP)
- Integración de autoservicio de cualquier aplicación web que tenga una página de inicio de sesión basada en HTML que use [SSO basado en contraseña](what-is-single-sign-on.md#password-based-sso)
- Conexión autoservicio de las aplicaciones que usan el [sistema para el protocolo de Cross-Domain Identity Management (SCIM) para el aprovisionamiento de usuarios](use-scim-to-provision-users-and-groups.md)
- Capacidad para agregar vínculos a cualquier aplicación del [iniciador de aplicaciones de Office 365](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) o del [panel de acceso de Azure AD](what-is-single-sign-on.md#linked-sign-on)

Estas capacidades pueden incluir la integración de autoservicio de un software como una aplicación de servicio (SaaS) que use, incluso si nadie tiene incorporada la aplicación aún a la Galería de aplicaciones de Azure AD. Otra capacidad es la integración de autoservicio de una aplicación web de terceros que su organización ha implementado en los servidores que controla, ya sea en la nube o local.

También se denomina *plantillas de integración de aplicaciones*, estas capacidades proporcionan puntos de conexión basada en estándares para las aplicaciones que admiten SAML, SCIM o autenticación basada en formularios. Las funciones tienen opciones flexibles y configuración para la compatibilidad con un amplio número de aplicaciones.

## <a name="adding-an-unlisted-application"></a>Adición de una aplicación que no figura en la lista

La plataforma Microsoft identity proporciona dos mecanismos para registrar aplicaciones.

Una aplicación que utiliza un protocolo modernos como [OpenId Connect y OAuth](../develop/active-directory-v2-protocols.md) autenticar sus usuarios se ha registrado mediante el [portal de registro de aplicación](../develop/quickstart-register-app.md).

Para registrar las aplicaciones que usan todos los demás tipos de [admite mecanismos de autenticación](what-is-single-sign-on.md), como el [SAML](../develop/single-sign-on-saml-protocol.md) de protocolo, utilice el **aplicaciones empresariales** hoja para conectarse ellos con la plataforma Microsoft identity.

Para conectar una aplicación no enumerada con una plantilla de integración de aplicaciones, siga estos pasos:

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) con su cuenta de administrador de plataforma de identidad de Microsoft.
2. Seleccione **aplicaciones empresariales** > **nueva aplicación**.
3. (Opcional pero recomendado) En el **agregar desde la galería** cuadro de búsqueda, escriba el nombre para mostrar de la aplicación. Si la aplicación aparece en los resultados de búsqueda, selecciónelo y omita el resto de este procedimiento.
4. Seleccione **aplicación ajena a la galería**. El **agregar su propia aplicación** aparece la página.

   ![Agregar aplicación](./media/configure-single-sign-on-non-gallery-applications/add-your-own-application.png)
5. Escriba el nombre para mostrar para la nueva aplicación.
6. Seleccione **Agregar**.

Mediante la adición de una aplicación de este modo, se proporciona una experiencia similar a la disponible para aplicaciones previamente integradas. Primero seleccione **inicio de sesión único** desde la barra lateral de la aplicación. La página siguiente (**seleccionar un método de inicio de sesión único**) presenta las opciones de configuración de SSO:

- **SAML**
- **Basado en contraseñas**
- **Vinculado**

![Seleccione un método de inicio de sesión único](./media/configure-single-sign-on-non-gallery-applications/select-a-single-sign-on-method.png)

Para obtener más información acerca de estas opciones, consulte las siguientes secciones de este artículo.

## <a name="saml-based-single-sign-on"></a>Inicio de sesión único basado en SAML

Seleccione el **SAML** opción para configurar la autenticación basada en SAML para la aplicación. (Esta opción requiere que la aplicación admita SAML 2.0). El **establecer seguridad de sesión único con SAML** aparece la página.

![Configurar inicio de sesión único con SAML](./media/configure-single-sign-on-non-gallery-applications/set-up-single-sign-on-with-saml.png)

Esta página contiene cinco encabezados diferentes:

| Número de título | Nombre de encabezado | Para obtener un resumen de este encabezado, vea: |
| --- | --- | --- |
| 1 | **Configuración básica de SAML** | [Escriba la configuración básica de SAML](#enter-basic-saml-configuration) |
| 2 | **Atributos de usuario y notificaciones** | [Revisar o personalizar las notificaciones emitidas en el token SAML](#review-or-customize-the-claims-issued-in-the-saml-token) |
| 3 | **Certificado de firma de SAML** | [Datos de expiración del certificado de revisión, el estado y la notificación por correo electrónico](#review-certificate-expiration-data-status-and-email-notification) |
| 4 | **Configurar \<nombre de aplicación >** | [Configurar la aplicación de destino](#set-up-target-application) |
| 5 | **Probar el inicio de sesión único con \<nombre de aplicación >** | [Probar la aplicación SAML](#test-the-saml-application) |

Ahora puede recopilar información sobre cómo usar las capacidades SAML de la aplicación antes de continuar. Complete las secciones siguientes para configurar el inicio de sesión único entre Azure AD y la aplicación.

### <a name="enter-basic-saml-configuration"></a>Especificación de la configuración básica de SAML

Para configurar Azure AD, vaya a la **configuración básica de SAML** encabezado y seleccione su **editar** icono (un lápiz). Puede especificar los valores manualmente o cargar un archivo de metadatos para extraer el valor de los campos.

![Configuración básica de SAML](./media/configure-single-sign-on-non-gallery-applications/basic-saml-configuration.png)

Se requieren los dos campos siguientes:

- **Identificador**. Este valor identifica la aplicación para el que está configurando el inicio de sesión único. Puede encontrar este valor como el **emisor** elemento en el **AuthnRequest** (solicitud SAML) enviado por la aplicación. Este valor también aparece como el **Id. de entidad** en los metadatos SAML proporcionados por la aplicación. Compruebe la documentación de SAML de la aplicación para obtener más información sobre lo que su **Id. de entidad** o **audiencia** es el valor.

  El siguiente código muestra cómo el **identificador** o **emisor** aparece en la solicitud SAML que envía la aplicación a Azure AD:

  ```xml
  <samlp:AuthnRequest
  xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
  ID="id6c1c178c166d486687be4aaf5e482730"
  Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
  </samlp:AuthnRequest>
  ```

- **Dirección URL de respuesta**. La dirección URL de respuesta es donde la aplicación espera recibir el token SAML. Esta dirección URL también se conoce como la URL del servicio (ACS) de consumidor de aserciones. Compruebe la documentación de SAML de la aplicación para más información sobre qué es la URL de respuesta del token SAML o la URL de ACS.

  Para configurar varias direcciones URL de respuesta, puede usar el siguiente script de PowerShell.

  ```powershell
  $sp = Get-AzureADServicePrincipal -SearchString "<Exact app name>"
  $app = Get-AzureADApplication -SearchString "<Exact app name>"
  $urllist = New-Object "System.Collections.Generic.List[String]"
  $urllist.Add("<reply URL 1>")
  $urllist.Add("<reply URL 2>")
  $urllist.Add("<reply URL 3>")
  Set-AzureADApplication -ObjectId $app.ObjectId -ReplyUrls $urllist
  Set-AzureADServicePrincipal -ObjectId $sp.ObjectId -ReplyUrls $urllist
  ```

Los tres campos siguientes son opcionales:

- **Dirección URL de inicio de sesión (iniciado por SP sólo)**. Este valor indica que cuando el usuario va a iniciar sesión en esta aplicación. Si la aplicación lleva a cabo SSO iniciado por SP, a continuación, cuando un usuario navega a esta dirección URL, el SP realiza la redirección necesaria a Azure AD para autenticar e iniciar sesión el usuario. Si especifica este campo, Azure AD usa esta dirección URL para iniciar la aplicación desde el Panel de acceso de Azure AD y Office 365. Si se omite este campo, Azure AD realiza en su lugar de sesión iniciado por el IdP durante los inicios de la aplicación de Office 365, el Panel de acceso de Azure AD o la dirección URL de inicio de sesión único de Azure AD (que puede copiar desde el **panel** página).

- **Estado de la retransmisión**. Puede especificar un estado de la retransmisión en SAML para indicar dónde redirigir a los usuarios después de la autenticación a la aplicación. Normalmente, el valor es una dirección URL o ruta de acceso que lleva a los usuarios a una ubicación específica dentro de la aplicación.

- **Dirección URL de cierre de sesión**. Este valor se utiliza para enviar la respuesta de cierre de sesión SAML a la aplicación.

Para obtener más información, consulte [único inicio de sesión en el protocolo SAML](../develop/single-sign-on-saml-protocol.md).

### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Revisión o personalización de las notificaciones emitidas en el token SAML

Cuando un usuario se autentica en la aplicación, Azure AD emite la aplicación de un token de SAML con la información (o notificaciones) sobre el usuario que lo identifica. De forma predeterminada, esta información incluye el nombre de usuario, dirección de correo electrónico, nombre y apellido del usuario.

Para ver o editar las notificaciones enviadas en el token SAML a la aplicación:

- Vaya a la **atributos de usuario y notificaciones** encabezado y seleccione el **editar** icono. El **atributos de usuario y notificaciones** aparece la página.

![Atributos y notificaciones de usuario](./media/configure-single-sign-on-non-gallery-applications/user-attributes-and-claims.png)

Es posible que deba editar las notificaciones emitidas en el token SAML por dos motivos:

- La aplicación requiere un conjunto diferente de URI o valores de notificación.
- La aplicación requiere la **denominar el valor de identificador** dicen que algo que no sea el nombre de usuario (también conocido como el nombre principal de usuario) almacenado en la plataforma Microsoft identity.

Para obtener más información, consulte [Cómo Personalizar las notificaciones emitidas en el token SAML para aplicaciones empresariales](../develop/active-directory-saml-claims-customization.md).

### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Revisión de los datos de expiración, el estado y la notificación por correo electrónico de los certificados

Al crear una galería o una aplicación ajena a la galería, Azure AD crea un certificado específico de la aplicación que expira tres años desde su fecha de creación. Necesita este certificado para establecer la confianza entre Azure AD y la aplicación. Para obtener detalles sobre el formato del certificado, consulte la documentación de SAML de la aplicación.

De Azure AD, puede descargar el certificado en formato Base64 o Raw directamente desde el principal activo **establecer seguridad de sesión único con SAML** página. Como alternativa, puede obtener el certificado activo, descargue el archivo XML de metadatos de aplicación o mediante el uso de la URL de metadatos de federación de aplicación.

Para ver, crear o descargar los certificados (activos o inactivos), vaya a la **certificado de firma SAML** encabezado y seleccione el **editar** icono. El **certificado de firma SAML** aparece.

![Certificado de firma de SAML](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)

Compruebe que el certificado tiene:

- *La fecha de caducidad deseada.* Puede configurar la fecha de expiración hasta tres años en el futuro.
- *Un estado de activo para el certificado que desee.* Si el estado es **inactivo**, cambie el estado a **Active**. Para cambiar el estado, haga clic en la fila del certificado deseado y seleccione **activar el certificado**.
- *La opción de firma correcta y el algoritmo.*
- *Las direcciones de correo electrónico de notificación correcto.* Cuando el certificado activo está cerca de la fecha de expiración, Azure AD envía una notificación a la dirección de correo electrónico configurada en este campo.  

Para obtener más información, consulte [administrar certificados para un inicio de sesión único federado](manage-certificates-for-federated-single-sign-on.md) y [opciones en el token SAML de firma de certificado avanzada](certificate-signing-options.md).

### <a name="set-up-target-application"></a>Configuración de la aplicación de destino

Para configurar la aplicación para inicio de sesión único, busque la documentación de la aplicación. Para encontrar la documentación, vaya a la **configurar \<nombreDeAplicación >** encabezado y seleccione **ver instrucciones paso a paso**. La documentación que aparece en el **configurar inicio de sesión** página. Esa página le guía en rellenar la **dirección URL de inicio de sesión**, **identificador de Azure AD**, y **dirección URL de cierre de sesión** valores en el **configurar \<nombre de la aplicación >** encabezado.

Los valores necesarios varían dependiendo de la aplicación. Para más información, consulte la documentación de SAML de la aplicación. El **dirección URL de inicio de sesión** y **Logout URL** valores que se resuelven en el mismo punto de conexión, que es el punto de conexión de control de la solicitud SAML para la instancia de Azure AD. El **identificador de Azure AD** es el valor de la **emisor** en el token SAML emitidas a la aplicación.

### <a name="assign-users-and-groups-to-your-saml-application"></a>Asignación de usuarios y grupos a una aplicación SAML

Una vez haya configurado la aplicación para usar Azure AD como proveedor de identidades basado en SAML, está casi listo para probar. Como control de seguridad, Azure AD solo emite un token que permiten al usuario iniciar sesión en la aplicación si Azure AD concedió acceso al usuario. Los usuarios pueden obtener acceso directamente o a través de una pertenencia a grupos.

Para asignar un nuevo usuario o grupo a la aplicación:

1. En la barra lateral de la aplicación, seleccione **usuarios y grupos**. El  **\<nombre de aplicación >-los usuarios y grupos** aparece la página, que muestra la lista actual de usuarios y grupos asignados.
2. Seleccione **agregar usuarios**. El **agregar asignaciones** aparece la página.
3. Seleccione **usuarios y grupos (\<número > seleccionados)**. El **usuarios y grupos** aparece en la página y muestra una lista de usuarios y grupos disponibles.
4. Escriba o desplácese para buscar el usuario o grupo que desea asignar en la lista.
5. Seleccione cada usuario o grupo que desea agregar y, a continuación, seleccione el **seleccione** botón. El **usuarios y grupos** desaparece de la página.
6. En el **agregar asignaciones** página, seleccione **asignar**. El  **<application name> -los usuarios y grupos** aparecerá la página con los usuarios adicionales que se muestra en la lista.

   ![Grupos y usuarios de aplicación](./media/configure-single-sign-on-non-gallery-applications/application-users-and-groups.png)

En esta lista, hacer lo siguiente:

- Quitar un usuario.
- Edite su rol.
- Actualice sus credenciales (nombre de usuario y contraseña) para que el usuario pueda autenticarse en la aplicación desde dentro del Panel de acceso del usuario.

Puede editar o quitar varios usuarios o grupos a la vez.

Asignar a un usuario permite que Azure AD para emitir un token de usuario. También hace que aparezca un icono para esta aplicación en el panel de acceso del usuario. También aparece un icono de la aplicación en el iniciador de aplicaciones de Office 365 si el usuario está usando Office 365.

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

### <a name="test-the-saml-application"></a>Prueba de la aplicación SAML

Antes de probar la aplicación SAML, debe ya configurado la aplicación con Azure AD y asignar usuarios o grupos a la aplicación. Para probar la aplicación SAML, seleccione **inicio de sesión único**, volverá a la **basado en SAML sign-on** página. (Si un método de inicio de sesión único diferente estaba en efecto, seleccione **cambiar los modos de inicio de sesión únicos** > **SAML** demasiado.) A continuación, en el **probar el inicio de sesión único con \<nombreDeAplicación >** encabezado, seleccione **prueba**. Para obtener más información, consulte [basado en SAML depurar único inicio de sesión en aplicaciones de Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Inicio de sesión único con contraseña

Seleccione esta opción para configurar [basado en contraseña única sesión](what-is-single-sign-on.md) para una aplicación web con una página de inicio de sesión HTML. El SSO basado en contraseña, también conocido como almacenamiento de contraseñas, permite administrar el acceso y las contraseñas de los usuarios en aplicaciones web que no admiten la federación de identidades. También es útil para escenarios donde varios usuarios necesitan compartir una sola cuenta, como cuentas de aplicación de medios sociales de su organización.

Después de seleccionar **basado en contraseña**, se le pedirá que escriba la dirección URL de página de la aplicación basada en web de inicio de sesión.

![Inicio de sesión único basado en contraseña](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

A continuación, siga estos pasos:

1. Escriba la dirección URL. Esta cadena debe ser la página que incluye el campo de entrada de nombre de usuario.
2. Seleccione **Guardar**. Azure AD intenta analizar la página de inicio de sesión para un nombre de usuario y una contraseña de entrada.
3. Si Azure AD del análisis de error al intentar, seleccione **configurar \<nombreDeAplicación > configuración de inicio de sesión único con contraseña** para mostrar el **configurar inicio de sesión** página. (Si el intento se realiza correctamente, puede pasar por alto el resto de este procedimiento).
4. Seleccione **detectan manualmente campos de inicio de sesión**. Aparecen instrucciones adicionales que describe la detección manual de campos de inicio de sesión.

   ![Configuración manual de basado en contraseña single sign-on](./media/configure-single-sign-on-non-gallery-applications/password-configure-sign-on.png)
5. Seleccione **capturar campos de inicio de sesión**. Se abre una página de estado de captura en una nueva pestaña que muestra el mensaje **captura de metadatos está actualmente en curso**.
6. Si el **necesita la extensión Access Panel** cuadro aparece en una nueva pestaña, seleccione **instalar ahora** para instalar el **seguro de mis aplicaciones de inicio de sesión de la extensión** extensión del explorador. (La extensión del explorador requiere Microsoft Edge, Chrome o Firefox). A continuación, instalar, iniciar y habilitar la extensión y actualice la página de estado de captura.

   La extensión del explorador, a continuación, abre otra ficha que muestra la dirección URL especificada.
7. En la pestaña con la dirección URL especificada, vaya a través del proceso de inicio de sesión. Rellene los campos de nombre de usuario y contraseña e intente iniciar sesión en. (No es necesario que proporcionar la contraseña correcta.)

   Un símbolo del sistema en el que se le pide que guarde los campos de inicio de sesión capturados.
8. Seleccione **Aceptar**. Cierra la ficha, la extensión del explorador actualiza la página de estado de captura con el mensaje **metadatos se han actualizado para la aplicación**y ese explorador pestaña también se cierra.
9. En Azure AD **configurar inicio de sesión** página, seleccione **bueno, he podido iniciar sesión en la aplicación correctamente**.
10. Seleccione **Aceptar**.

Después de la captura de la página de inicio de sesión, puede asignar usuarios y grupos, y puede configurar directivas de credencial como normal [aplicaciones de inicio de sesión único con contraseña](what-is-single-sign-on.md).

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

## <a name="existing-single-sign-on"></a>Inicio de sesión único existente

Seleccione esta opción para agregar un vínculo a la aplicación en el portal de Azure AD Access Panel u Office 365 de su organización. Puede usar este método para agregar vínculos a las aplicaciones web personalizadas que actualmente usan Active Directory Federation Services (u otro servicio de federación) en lugar de Azure AD para la autenticación. O bien, puede agregar vínculos profundos a páginas específicas de SharePoint o a otras páginas web que desee que aparezcan en los paneles de acceso de su usuario.

Después de seleccionar **vinculado**, se le pedirá que escriba la dirección URL de la aplicación para vincular a. Escriba la dirección URL y seleccione **guardar**. Puede asignar usuarios y grupos a la aplicación, lo que hace que la aplicación aparezca en el [iniciador de aplicaciones de Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) o [panel de acceso de Azure AD](end-user-experiences.md) para esos usuarios.

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

- [Instrucciones: Personalizar las notificaciones emitidas en el token SAML para aplicaciones empresariales](../develop/active-directory-saml-claims-customization.md)
- [Depurar basado en SAML único inicio de sesión en aplicaciones de Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md)
- [Plataforma de identidad de Microsoft (anteriormente Azure Active Directory para desarrolladores)](../develop/index.yml)
