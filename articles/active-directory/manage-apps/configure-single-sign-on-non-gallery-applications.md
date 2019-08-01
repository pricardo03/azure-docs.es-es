---
title: 'Inicio de sesión único en aplicaciones que no están en la galería: plataforma de identidad de Microsoft | Microsoft Docs'
description: Configuración del inicio de sesión único (SSO) en aplicaciones que no estén en la galería en la plataforma de identidad de Microsoft (Azure AD)
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
ms.openlocfilehash: a72cb7bc7feeba984d568a0465d4f23a494496e8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807650"
---
# <a name="configure-single-sign-on-to-non-gallery-applications-in-microsoft-identity-platform"></a>Configuración del inicio de sesión único en aplicaciones que no estén en la galería en la plataforma de identidad de Microsoft

En este artículo se trata de una característica que permite a los administradores configurar un inicio de sesión único en aplicaciones que no están presentes en la galería de aplicaciones de la plataforma de identidad de Microsoft *sin escribir código*.

Si por el contrario desea obtener instrucciones para desarrolladores sobre cómo integrar aplicaciones personalizadas con Azure AD a través de código, consulte [Escenarios de autenticación para Azure AD](../develop/authentication-scenarios.md).

La galería de aplicaciones de la plataforma de identidad de Microsoft proporciona una lista de las aplicaciones que se sabe que admiten un formulario de inicio de sesión único en la plataforma de identidad de Microsoft, tal como se describe en [este artículo](what-is-single-sign-on.md). Una vez que un especialista en TI o un integrador de sistemas de la organización ha encontrado la aplicación que desea conectar, puede empezar por seguir las instrucciones detalladas que se proporcionan en Azure Portal para habilitar el inicio de sesión único.

Estas funcionalidades también están disponibles, según el contrato de licencia. Consulte la [página de precios](https://azure.microsoft.com/pricing/details/active-directory/)para obtener más información.

- Integración de autoservicio de una aplicación que utiliza un protocolo moderno como [OpenId Connect/OAuth](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols) para autenticar a los usuarios y obtener tokens para [Microsoft Graph](https://graph.microsoft.com).
- Integración de autoservicio de cualquier aplicación que admita proveedores de identidades de [Lenguaje de marcado de aserción de seguridad (SAML) 2.0](https://wikipedia.org/wiki/SAML_2.0) (iniciado por el proveedor de servicios o por el proveedor de identidades)
- Integración de autoservicio de cualquier aplicación web que tenga una página de inicio de sesión basada en HTML que use [SSO basado en contraseña](what-is-single-sign-on.md#password-based-sso)
- Conexión de autoservicio de las aplicaciones que usan el protocolo [System for Cross-Domain Identity Management (SCIM) para el aprovisionamiento de usuarios](use-scim-to-provision-users-and-groups.md)
- Capacidad para agregar vínculos a cualquier aplicación del [iniciador de aplicaciones de Office 365](https://www.microsoft.com/microsoft-365/blog/2014/10/16/organize-office-365-new-app-launcher-2/) o del [panel de acceso de Azure AD](what-is-single-sign-on.md#linked-sign-on)

Estas funcionalidades pueden incluir la integración de autoservicio de una aplicación de software como servicio (SaaS) que se utiliza, incluso si nadie ha incorporado la aplicación a la galería de aplicaciones de Azure AD todavía. Otra capacidad es la integración de autoservicio de una aplicación web de terceros que su organización ha implementado en los servidores que controla, ya sea en la nube o el entorno local.

También conocidas como *plantillas de integración de aplicaciones*, estas funcionalidades proporcionan puntos de conexión basados en estándares para las aplicaciones que admiten SAML, SCIM o autenticación basada en formularios. Las funcionalidades incluyen opciones y configuraciones flexibles para su compatibilidad con un amplio número de aplicaciones.

## <a name="adding-an-unlisted-application"></a>Adición de una aplicación que no figura en la lista

La plataforma de identidad de Microsoft proporciona dos mecanismos para registrar aplicaciones.

Una aplicación que utiliza un protocolo moderno como [OpenId Connect/OAuth](../develop/active-directory-v2-protocols.md) para autenticar a sus usuarios se registra mediante el [Portal de registro de aplicaciones](../develop/quickstart-register-app.md).

Para registrar las aplicaciones mediante todos los demás tipos de [mecanismos de autenticación admitidos](what-is-single-sign-on.md), como el protocolo [SAML](../develop/single-sign-on-saml-protocol.md), utilice la hoja **Aplicaciones empresariales** para conectarlos con la plataforma de Microsoft Identity.

Para conectar una aplicación no incluida en la lista mediante una plantilla de integración de aplicaciones, siga estos pasos:

1. Inicie sesión en el [portal de Azure Active Directory](https://aad.portal.azure.com/) con su cuenta de administrador de la plataforma de identidad de Microsoft.
1. Seleccione **Aplicaciones empresariales** > **Nueva aplicación**.
1. (Opcional pero recomendado) En el cuadro de búsqueda **Agregar desde la galería**, especifique el nombre para mostrar de la aplicación. Si la aplicación aparece en los resultados de la búsqueda, selecciónela y omita el resto de este procedimiento.
1. Seleccione **Aplicación situada fuera de la galería**. Se abre la página **Agregue su propia aplicación**.

   ![Muestra la página Agregue su propia aplicación](./media/configure-single-sign-on-non-gallery-applications/add-your-own-application.png)

1. Escriba el nombre para mostrar para la nueva aplicación.
1. Seleccione **Agregar**.

Al agregar una aplicación de esta forma, ofrece una experiencia parecida a la disponible para las aplicaciones preintegradas. Primero seleccione **Inicio de sesión único** en la barra lateral de la aplicación. En la página siguiente (**Seleccione un método de inicio de sesión único**) se muestran las opciones para configurar el inicio de sesión único:

- **SAML**
- **Basado en contraseñas**
- **Vinculado**

![Muestra la página Seleccione un método de inicio de sesión único](./media/configure-single-sign-on-non-gallery-applications/select-a-single-sign-on-method.png)

Para obtener más información sobre estas opciones, consulte las siguientes secciones de este artículo.

## <a name="saml-based-single-sign-on"></a>Inicio de sesión único basado en SAML

Seleccione la opción **SAML** para configurar la autenticación basada en SAML para la aplicación. (Esta opción requiere que la aplicación admita SAML 2.0.) Se muestra la página **Configurar el inicio de sesión único con SAML**.

![Muestra la página Configurar el inicio de sesión único con SAML](./media/configure-single-sign-on-non-gallery-applications/set-up-single-sign-on-with-saml.png)

Esta página contiene cinco encabezados diferentes:

| Número de título | Nombre de encabezado | Para obtener un resumen de este encabezado, consulte: |
| --- | --- | --- |
| 1 | **Configuración básica de SAML** | [Especificación de la configuración básica de SAML](#enter-basic-saml-configuration) |
| 2 | **Atributos y notificaciones de usuario** | [Revisión o personalización de las notificaciones emitidas en el token SAML](#review-or-customize-the-claims-issued-in-the-saml-token) |
| 3 | **Certificado de firma de SAML** | [Revisión de los datos de expiración, el estado y la notificación por correo electrónico de los certificados](#review-certificate-expiration-data-status-and-email-notification) |
| 4 | **Configuración del \<nombre de aplicación>** | [Configuración de la aplicación de destino](#set-up-target-application) |
| 5 | **Prueba de inicio de sesión único con \<nombre de aplicación>** | [Prueba de la aplicación SAML](#test-the-saml-application) |

Ahora recopile información acerca de cómo usar las funcionalidades de SAML de la aplicación antes de continuar. Complete las siguientes secciones para configurar el inicio de sesión único entre la aplicación y Azure AD.

### <a name="enter-basic-saml-configuration"></a>Especificación de la configuración básica de SAML

Para configurar Azure AD, vaya al título **Configuración básica de SAML** y seleccione el icono de **edición** (un lápiz). Puede especificar los valores manualmente o cargar un archivo de metadatos para extraer el valor de los campos.

![Muestra la página Configuración básica de SAML](./media/configure-single-sign-on-non-gallery-applications/basic-saml-configuration.png)

Se requieren los dos campos siguientes:

- **Identificador**. Este valor debería identificar de forma exclusiva la aplicación para la que se configura el inicio de sesión único. Puede encontrar este valor como el elemento **Issuer** en el elemento **AuthRequest** (solicitud SAML) enviado por la aplicación. Este valor también aparece como el **Id. de entidad** en los metadatos SAML proporcionados por la aplicación. Compruebe la documentación de SAML de la aplicación para más información sobre qué es el **identificador de entidad** o el valor de **Audiencia**.

  En el código siguiente se muestra cómo los elementos **Identifier** o **Issuer** aparecen en la solicitud SAML que envía la aplicación a Azure AD:

  ```xml
  <samlp:AuthnRequest
  xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
  ID="id6c1c178c166d486687be4aaf5e482730"
  Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
  xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
  </samlp:AuthnRequest>
  ```

- **URL de respuesta**. Es la dirección URL de respuesta es el lugar donde la aplicación espera recibir el token SAML. También se conoce como dirección URL del servicio de consumidor de aserciones (ACS). Compruebe la documentación de SAML de la aplicación para más información sobre qué es la URL de respuesta del token SAML o la URL de ACS.

  Puede usar el siguiente script de PowerShell para configurar varias URL de respuesta.

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

- **URL de inicio de sesión** (iniciado solo por el proveedor de servicios). Este valor indica cuándo el usuario va a iniciar sesión en esta aplicación. Si la aplicación realiza el inicio de sesión único iniciado por el proveedor de servicios, cuando un usuario navega a esta dirección URL, el proveedor de servicios realiza la redirección necesaria a Azure AD para autenticar al usuario e iniciar sesión. Si especifica este campo, Azure AD utilizará esta dirección URL para iniciar la aplicación desde Office 365 y el panel de acceso de Azure AD. Si omite este campo, Azure AD realiza el inicio de sesión iniciado por el proveedor de identidades durante el inicio de la aplicación desde Office 365, el Panel de acceso de Azure AD o la dirección URL del inicio de sesión único de Azure AD (que puede copiar desde la página **Panel**).

- **Estado de la retransmisión**. Puede especificar un estado de la retransmisión en SAML para indicar a la aplicación dónde redirigir a los usuarios después de la autenticación. El valor es típicamente una dirección URL o ruta URL que lleva a los usuarios a una ubicación específica dentro de la aplicación.

- **Dirección URL de cierre de sesión** Este valor se usa para devolver las respuestas de cierre de sesión de SAML a la aplicación.

Para más información, consulte [Protocolo SAML de inicio de sesión único](../develop/single-sign-on-saml-protocol.md).

### <a name="review-or-customize-the-claims-issued-in-the-saml-token"></a>Revisión o personalización de las notificaciones emitidas en el token SAML

Cuando un usuario se autentique en la aplicación, Azure AD emite un token SAML a la aplicación con información (o notificaciones) sobre el usuario que lo identifica de forma única. De forma predeterminada, dicha información incluye el nombre de usuario, la dirección de correo electrónico, el nombre y los apellidos del usuario.

Para ver o editar las notificaciones enviadas en el token SAML a la aplicación:

- Vaya al título **Atributos y notificaciones de usuario** y seleccione el icono de **edición**. Se muestra la página **Atributos y notificaciones de usuario**.

![Muestra la página Atributos y notificaciones de usuario](./media/configure-single-sign-on-non-gallery-applications/user-attributes-and-claims.png)

Tendría que editar las notificaciones emitidas en el token SAML por dos motivos:

- La aplicación requiere un conjunto diferente de URI o valores de notificación.
- La aplicación requiere que la notificación **Valor de identificador de nombre** sea algo distinto del nombre de usuario (también conocido como nombre principal de usuario) almacenado en la plataforma de identidad de Microsoft.

Para obtener más información, consulte [Instrucciones: Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](../develop/active-directory-saml-claims-customization.md).

### <a name="review-certificate-expiration-data-status-and-email-notification"></a>Revisión de los datos de expiración, el estado y la notificación por correo electrónico de los certificados

Al crear una aplicación de la galería o de fuera de la galería, Azure AD crea un certificado específico de la aplicación que expira a los tres años de la fecha de creación. Necesita este certificado para establecer la confianza entre Azure AD y la aplicación. Para obtener detalles sobre el formato del certificado, consulte la documentación de SAML de la aplicación.

Desde Azure AD, puede descargar el certificado activo en formato Base64 o Raw directamente desde la página principal **Configurar el inicio de sesión único con SAML**. Además, puede obtener el certificado activo al descargar el archivo XML de metadatos de la aplicación o mediante el uso de la URL de metadatos de la federación de aplicaciones.

Para ver, crear o descargar los certificados (activos o inactivos), vaya al título **Certificado de firma de SAML** y seleccione el icono de **edición**. Se muestra **Certificado de firma de SAML**.

![Muestra la página Certificado de firma de SAML](./media/configure-single-sign-on-non-gallery-applications/saml-signing-certificate.png)

Compruebe que el certificado tiene:

- *La fecha de expiración deseada.* Puede configurar la fecha de expiración hasta tres años en el futuro.
- *Un estado de activo para el certificado que desee.* Si el estado es **Inactivo**, cambie el estado a **Activo**. Para cambiar el estado, haga clic con el botón derecho en la fila del certificado deseado y seleccione **Activar el certificado**.
- *La opción de firma y el algoritmo correctos.*
- *Los correos electrónicos de notificación correctos.* Cuando el certificado activo esté cerca de la fecha de expiración, Azure AD envía una notificación a la dirección de correo electrónico configurada en este campo.  

Para más información, consulte [Administración de certificados para el inicio de sesión único federado](manage-certificates-for-federated-single-sign-on.md) y [Opciones avanzadas de firma de certificados en el token SAML](certificate-signing-options.md).

### <a name="set-up-target-application"></a>Configuración de la aplicación de destino

Para configurar la aplicación para el inicio de sesión único, busque la documentación de la aplicación. Para encontrar la documentación, vaya al encabezado **Configurar \<nombreDeAplicación>** y seleccione **Ver instrucciones detalladas**. La documentación aparece en la página **Configurar inicio de sesión**. Esta página le guiará para rellenar los valores de **Dirección URL de inicio de sesión**, **Identificador de Azure AD** y **URL de cierre de sesión** en el encabezado **Configurar \<nombreDeAplicación>** .

Los valores necesarios varían dependiendo de la aplicación. Para más información, consulte la documentación de SAML de la aplicación. Los valores **Dirección URL de inicio de sesión** y **URL de cierre de sesión** se resuelven en el mismo punto de conexión, que es el punto de conexión de control de solicitudes SAML de su instancia de Azure AD. **Identificador de Azure AD** es el valor del **Emisor** en el token SAML emitido a la aplicación.

### <a name="assign-users-and-groups-to-your-saml-application"></a>Asignación de usuarios y grupos a una aplicación SAML

Tras configurar la aplicación para que use Azure AD como proveedor de identidades basado en SAML, ya casi se está listo para probarla. Como control de seguridad, Azure AD solo emite un token que permita al usuario iniciar sesión en la aplicación si Azure AD le ha concedido acceso al usuario. Los usuarios pueden obtener acceso directamente o a través de la pertenencia al grupo.

Para asignar un nuevo usuario o grupo a la aplicación:

1. En la barra lateral de la aplicación, seleccione **Usuarios y grupos**. Aparece la página **\<nombreDeAplicación>- Usuarios y grupos**, que muestra la lista actual de usuarios y grupos asignados.
1. Seleccione **Agregar usuarios**. Se muestra la página **Agregar asignación**.
1. Seleccione **Usuarios y grupos (\<número> Seleccionados)** . Aparece la página **Usuarios y grupos** y muestra una lista de usuarios y grupos disponibles.
1. Escriba o desplácese para buscar el usuario o grupo que desea asignar en la lista.
1. Seleccione cada usuario o grupo que desea agregar y, a continuación, seleccione el botón **Seleccionar**. Desaparece la página **Usuarios y grupos**.
1. En la página **Agregar asignación**, seleccione **Asignar**. Se muestra la página **\<nombreDeAplicación>- Usuarios y grupos** con los usuarios adicionales que se muestra en la lista.

   ![Muestra la página Usuarios y grupos de la aplicación](./media/configure-single-sign-on-non-gallery-applications/application-users-and-groups.png)

En esta lista, puede hacer lo siguiente:

- Eliminar un usuario.
- Editar su rol.
- Actualizar sus credenciales (nombre de usuario y contraseña) para que el usuario pueda autenticarse en la aplicación desde el Panel de acceso del usuario.

Editar o quitar varios usuarios o grupos a la vez.

Asignar un usuario permite a Azure AD emitir un token al usuario. También hace que aparezca un icono para esta aplicación en el panel de acceso del usuario. Si el usuario usa Office 365, también aparece un icono de la aplicación en el iniciador de aplicaciones de Office 365.

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

### <a name="test-the-saml-application"></a>Prueba de la aplicación SAML

Antes de probar la aplicación SAML, debe tener ya configurada la aplicación con Azure AD y usuarios o grupos asignados a la aplicación. Para probar la aplicación SAML, seleccione **Inicio de sesión único**, que le devolverá a la página **Inicio de sesión basado en SAML**. (Si otro método de inicio de sesión único ya estaba en vigor, seleccione también **Cambiar los modos de inicio de sesión único** > **SAML**). Después, en el encabezado **Probar el inicio de sesión único con \<nombreDeAplicación>** , seleccione **Probar**. Para más información, consulte [Depuración del inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md).

## <a name="password-single-sign-on"></a>Inicio de sesión único con contraseña

Seleccione esta opción para configurar [el inicio de sesión único basado en contraseña](what-is-single-sign-on.md) para una aplicación web con una página de inicio de sesión HTML. El SSO basado en contraseña, también conocido como almacenamiento de contraseñas, permite administrar el acceso y las contraseñas de los usuarios en aplicaciones web que no admiten la federación de identidades. También es útil para escenarios en los que varios usuarios necesitan compartir una sola cuenta, como las cuentas de aplicaciones de redes sociales de la organización.

Después de seleccionar **Basado en contraseña**, se le pedirá que escriba la dirección URL de la página de inicio de sesión basada en web de la aplicación.

![Muestra la página URL de inicio de sesión para especificar la dirección URL de inicio de sesión](./media/configure-single-sign-on-non-gallery-applications/password-based-sso.png)

A continuación, siga estos pasos:

1. Escriba la dirección URL. Esta cadena debe ser la página que incluye el campo de entrada del nombre de usuario.
1. Seleccione **Guardar**. Azure AD intenta analizar la página de inicio de sesión para detectar si se han escrito un nombre de usuario y una contraseña.
1. Si se produce un error en el intento de análisis de Azure AD, seleccione **Establecer configuración de inicio de sesión único con contraseña de \<nombreDeAplicación** para mostrar la página**Configurar inicio de sesión**. (Si el intento se realiza correctamente, puede hacer caso omiso del resto de este procedimiento).
1. Seleccione **Detectar campos de inicio de sesión manualmente**. Aparecen instrucciones adicionales que describen la detección manual de los campos de inicio de sesión.

   ![Configuración manual del inicio de sesión único con contraseña](./media/configure-single-sign-on-non-gallery-applications/password-configure-sign-on.png)

1. Seleccione **Capturar campos de inicio de sesión**. Se abre una página de estado de la captura en una nueva pestaña, que muestra el mensaje **Actualmente la captura de metadatos está en curso**.
1. Si la casilla que indica que se **requiere una extensión del panel de acceso** aparece en una nueva pestaña, seleccione **Instalar ahora** para instalar la extensión del explorador **Extensión de inicio de sesión seguro de mis aplicaciones**. (La extensión del explorador requiere Microsoft Edge, Chrome o Firefox). A continuación, instale, inicie y active la extensión, y actualice la página de estado de la captura.

   La extensión del explorador abre otra pestaña que muestra la dirección URL especificada.

1. En la pestaña con la dirección URL especificada, recorra el proceso de inicio de sesión. Rellene los campos de nombre de usuario y contraseña e intente iniciar sesión. (No es necesario proporcionar la contraseña correcta).

   Se le pedirá que guarde los campos de inicio de sesión capturados.

1. Seleccione **Aceptar**. La pestaña se cierra, la extensión del explorador actualiza la página de estado de la captura con el mensaje **Los metadatos se han actualizado para la aplicación** y esa pestaña del explorador también se cierra.
1. En la página **Configurar inicio de sesión** de Azure AD, seleccione **Bien. Pude iniciar sesión en la aplicación correctamente**.
1. Seleccione **Aceptar**.

Después de capturar la página de inicio de sesión, puede asignar los usuarios y grupos, y puede configurar las directivas de credenciales como si fueran [aplicaciones normales de inicio de sesión único con contraseña](what-is-single-sign-on.md).

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

## <a name="existing-single-sign-on"></a>Inicio de sesión único existente

Seleccione esta opción si desea agregar un vínculo a una aplicación en el panel de acceso de Azure AD o en el portal de Office 365 de la organización. Puede usar este método para agregar vínculos a aplicaciones web personalizadas que actualmente utilizan los Servicios de federación de Active Directory (u otro servicio de federación), en lugar de Azure AD para la autenticación. O bien, puede agregar vínculos profundos a páginas específicas de SharePoint o a otras páginas web que desee que aparezcan en los paneles de acceso de su usuario.

Después de seleccionar **Vinculado**, se le pedirá que escriba la dirección URL de la aplicación con la que se va a establecer el vínculo. Escriba la dirección URL y seleccione **Guardar**. Puede asignar usuarios y grupos a la aplicación, lo que hace que la aplicación aparezca en el [iniciador de aplicaciones de Office 365](https://blogs.office.com/2014/10/16/organize-office-365-new-app-launcher-2/) o en el [panel de acceso de Azure AD](end-user-experiences.md) de dichos usuarios.

> [!NOTE]
> Para cargar un logotipo de icono de la aplicación, pulse el botón **Cargar logotipo** en la pestaña **Configura**r de la aplicación.

## <a name="related-articles"></a>Artículos relacionados

- [Uso de Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](../develop/active-directory-saml-claims-customization.md).
- [Depuración del inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory](../develop/howto-v1-debug-saml-sso-issues.md)
- [Plataforma de identidad de Microsoft (anteriormente llamada Azure Active Directory para desarrolladores)](../develop/index.yml)
