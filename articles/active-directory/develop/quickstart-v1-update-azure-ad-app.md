---
title: Integración de aplicaciones con Azure Active Directory
description: Obtenga información acerca de cómo actualizar una aplicación en Azure Active Directory (Azure AD).
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: f311f951e09e064b8eac779b1082c666fe029479
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977257"
---
# <a name="quickstart-update-an-application-in-azure-active-directory"></a>Inicio rápido: Actualización de una aplicación en Azure Active Directory

Es posible que los desarrolladores empresariales y los proveedores de software como servicio (SaaS) que han registrado aplicaciones con Azure Active Directory (Azure AD) deban configurar las aplicaciones para obtener acceso a otros recursos, como las API web, para hacer que estén disponibles en otras organizaciones y mucho más.

En este inicio rápido, obtendrá información sobre las distintas formas para poder configurar o actualizar la aplicación para que cumpla los requisitos o las necesidades de otras organizaciones.

## <a name="prerequisites"></a>Requisitos previos

Para comenzar, asegúrese de que ha completado estos pasos:

* Lea la información general sobre el [marco de consentimiento de Azure AD](consent-framework.md), el cual es importante entender al crear aplicaciones que necesitan ser utilizadas por otros usuarios o aplicaciones.
* Necesita un inquilino de Azure AD que tenga las aplicaciones registradas en él.
  * Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).
  * Si no dispone de las aplicaciones registradas en el inquilino, [obtenga información sobre cómo agregar una aplicación a Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="configure-a-client-application-to-access-web-apis"></a>Configuración de una aplicación cliente para tener acceso a las API web

Para que una aplicación cliente web o confidencial pueda participar en un flujo de concesión de autorización que requiera autenticación (y obtener un token de acceso), debe establecer credenciales seguras. El método de autenticación predeterminado compatible con Azure Portal es un identificador de cliente y una clave secreta. En esta sección se describen los pasos de configuración necesarios para proporcionar la clave secreta con las credenciales del cliente.

Para que un cliente pueda tener acceso a una API web expuesta por una aplicación de recursos (como Microsoft Graph API), el marco de consentimiento garantiza que el cliente obtenga la concesión de los permisos necesarios en función de los solicitados. De forma predeterminada, todas las aplicaciones pueden elegir permisos de **Azure Active Directory** (Graph API) y del modelo de implementación clásica de Azure. El [permiso de "inicio de sesión y lectura del perfil de usuario" de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes#PermissionScopeDetails) también se selecciona de forma predeterminada. Si el cliente está registrado en un inquilino que tiene cuentas con suscripciones a Office 365, las API web y los permisos para SharePoint y Exchange Online están disponibles para seleccionarse. Puede seleccionar entre dos tipos de permisos para cada API web que desee:

- Permisos de la aplicación: la aplicación cliente necesita acceso directo a la API web como sí misma (sin contexto de usuario). Este tipo de permiso requiere el consentimiento del administrador y no está disponible para aplicaciones cliente nativas.
- Permisos delegados: la aplicación cliente necesita acceso a la API web como el usuario que inició sesión, pero con acceso limitado por el permiso seleccionado. Este tipo de permiso lo puede conceder un usuario a menos que el permiso requiera el consentimiento del administrador.

  > [!NOTE]
  > La adición de un permiso delegado a una aplicación no concede consentimiento automáticamente a los usuarios del inquilino. Los usuarios tienen todavía que conceder manualmente el consentimiento para los permisos delegados agregados en tiempo de ejecución, a menos que lo haga el administrador en nombre de todos los usuarios.

### <a name="add-application-credentials-or-permissions-to-access-web-apis"></a>Adición de credenciales o permisos para tener acceso a las API web

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory**, seleccione **Registros de aplicaciones** y encuentre o seleccione la aplicación que desea configurar.

   ![Actualización del registro de una aplicación](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. Pasará a la página de registro principal de la aplicación, que contiene la página **Configuración** de la aplicación. Para agregar una credencial para la aplicación web:
  1. Seleccione la sección **Claves** en la página **Configuración**.
  2. Para agregar un certificado:
    - Seleccione **Cargar clave pública**.
    - Seleccione el archivo que desea cargar. Debe ser uno de los siguientes tipos de archivo: .cer, .pem, .crt.
  - Para agregar una contraseña:
    - Agregue una descripción para la clave.
    - Seleccione una duración.
    - Seleccione **Guardar**. La columna situada más a la derecha contendrá el valor de clave después de que guarde los cambios de configuración. **Asegúrese de copiar la clave** para su uso en el código de la aplicación cliente, ya que no es accesible una vez se sale de esta página.

5. Para agregar permisos para tener acceso a las API de recursos desde el cliente
  1. Seleccione la sección **Permisos necesarios** en la página **Configuración** y, a continuación, seleccione **Agregar**.
  1. Seleccione **Seleccionar una API** para seleccionar el tipo de recursos entre los que desea elegir.
  1. Examine la lista de las API disponibles o use el cuadro de búsqueda para seleccionar entre las aplicaciones de recursos disponibles del directorio que exponen una API web. Seleccione el recurso que le interese y, luego, haga clic en **Seleccionar**.
  1. En la página **Habilitar acceso**, seleccione los permisos de la aplicación o permisos delegados que necesita la aplicación al tener acceso a la API.
   
  ![Actualización del registro de una aplicación: api de permisos](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-api.png)

  ![Actualización del registro de una aplicación: permisos](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms.png)

6. Cuando termine, seleccione el botón **Seleccionar** de la página **Habilitar acceso** y, a continuación, el botón **Listo** de la página **Agregar acceso de API**. Se le redirigirá a la página **Permisos necesarios**, donde se agrega el nuevo recurso a la lista de las API.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configuración de una aplicación de recursos para exponer las API web

Puede desarrollar una API web y ponerla a disposición de las aplicaciones cliente si expone los [ámbitos](developer-glossary.md#scopes) y los [roles](developer-glossary.md#roles) de acceso. Una API web configurada correctamente se pone a disposición de otras aplicaciones del mismo modo que otras API web de Microsoft, incluidas Graph API y las API de Office 365. Los ámbitos y los roles de acceso se exponen a través del [manifiesto de la aplicación](developer-glossary.md#application-manifest), que es un archivo JSON que representa la configuración de identidad de la aplicación.

En la siguiente sección se muestra cómo exponer ámbitos de acceso modificando el manifiesto de la aplicación de recursos.

### <a name="add-access-scopes-to-your-resource-application"></a>Adición de ámbitos de acceso a la aplicación de recursos

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, seleccione **Azure Active Directory > Registros de aplicaciones** y, a continuación, encuentre o seleccione la aplicación que desea configurar.

   ![Actualización del registro de una aplicación](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

4. Pasará a la página de registro principal de la aplicación, lo que abre la página **Configuración** de la aplicación. Cambie a la página **Editar manifiesto** haciendo clic en **Manifiesto** desde la página de registro de la aplicación. Se abre un editor de manifiestos basado en web que le permite **Editar** el manifiesto desde el portal. Si lo desea, puede hacer clic en **Descargar**, editar de forma local y, a continuación, usar **Cargar** para volver a aplicarlo a la aplicación.
5. En este ejemplo, se expondrá un ámbito nuevo llamado `Employees.Read.All` en el recurso o API, agregando el siguiente elemento JSON a la colección `oauth2Permissions`. El ámbito `user_impersonation` existente se proporciona de forma predeterminada durante el registro. `user_impersonation` permite a una aplicación cliente solicitar permiso para tener acceso al recurso, bajo la identidad del usuario que inició sesión. No olvide agregar la coma después del elemento de ámbito `user_impersonation` existente y cambiar los valores de las propiedades para satisfacer las necesidades de su recurso. 

  ```json
  {
    "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
    "adminConsentDisplayName": "Read-only access to Employee records",
    "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
    "isEnabled": true,
    "type": "User",
    "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
    "userConsentDisplayName": "Read-only access to your Employee records",
    "value": "Employees.Read.All"
  }
  ```

  > [!NOTE]
  > El valor `id` debe generarse mediante programación o con una herramienta de generación de GUID, como [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx). `id` representa un identificador único para el ámbito que se expone mediante la API web. Una vez que un cliente se configura apropiadamente con permisos para tener acceso a la API web, Azure AD emite un token de acceso de OAuth2.0. Cuando el cliente llama a la API web, presenta el token de acceso que tiene la notificación de ámbito (scp) establecida para los permisos solicitados en el registro de la aplicación.
  >
  > Puede exponer ámbitos adicionales posteriormente si es necesario. Tenga en cuenta que la API web podría exponer varios ámbitos asociados a diversas funciones diferentes. El recurso puede controlar el acceso a la API web en tiempo de ejecución, mediante la evaluación de las notificaciones de ámbito (`scp`) del token de acceso de OAuth 2.0 recibido.

6. Cuando termine, haga clic en **Guardar**. Ahora la API web está configurada para que la usen otras aplicaciones del directorio.

  ![Actualización del registro de una aplicación](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-manifest.png)

### <a name="verify-the-web-api-is-exposed-to-other-applications-in-your-tenant"></a>Comprobación de que la web API se expone a otras aplicaciones del inquilino

1. Vuelva al inquilino de Azure AD, seleccione **Registros de aplicaciones** de nuevo y, a continuación, busque o seleccione la aplicación cliente que desea configurar.

   ![Actualización del registro de una aplicación](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration.png)

2. Repita el paso 5 como hizo en [Configuración de una aplicación cliente para tener acceso a las API web](#configure-a-client-application-to-access-web-apis). Cuando llegue al paso **Seleccionar una API**, busque el recurso escribiendo el nombre de la aplicación en el campo de búsqueda y haga clic en **Seleccionar**.

3. En la página **Habilitar acceso** debería ver el nuevo ámbito, disponible para las solicitudes de permiso de clientes.

  ![Se muestran los nuevos permisos](./media/quickstart-v1-integrate-apps-with-azure-ad/update-app-registration-settings-permissions-perms-newscopes.png)

### <a name="more-on-the-application-manifest"></a>Más sobre el manifiesto de aplicación

El manifiesto de aplicación actúa como un mecanismo para actualizar la entidad de aplicación, que define todos los atributos de configuración de identidad de una aplicación de Azure AD, incluidos los ámbitos de acceso de API que analizamos. Para más información sobre la entidad Application y su esquema, consulte la [documentación sobre la entidad Application de Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity). El artículo contiene una completa información de referencia sobre los miembros de la entidad Application utilizados para especificar los permisos para la API, incluidos:  

- El miembro appRoles, que es una colección de entidades [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type) usadas para definir los [permisos de aplicación](developer-glossary.md#permissions) para una API web. 
- El miembro oauth2Permissions, que es una colección de entidades [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type) usadas para definir los [permisos delegados](developer-glossary.md#permissions) para una API web.

Para obtener más información sobre los conceptos del manifiesto de aplicación en general, consulte [Manifiesto de aplicación de Azure AD](reference-app-manifest.md).

## <a name="accessing-the-azure-ad-graph-and-office-365-via-microsoft-graph-apis"></a>Acceso a Azure AD Graph y Office 365 mediante Microsoft Graph API  

Como se mencionó anteriormente, además de exponer y tener acceso a las API en sus propias aplicaciones, también puede registrar la aplicación cliente para tener acceso a las API expuestas por los recursos de Microsoft. Microsoft Graph API, llamada "Microsoft Graph" en lista de recursos y API del portal, está disponible para todas las aplicaciones registradas en Azure AD. Si va a registrar la aplicación cliente en un inquilino que contiene cuentas registradas en una suscripción de Office 365, también puede tener acceso a los ámbitos expuestos por los distintos recursos de Office 365.

Para obtener una descripción completa sobre los ámbitos expuestos por Microsoft Graph API, consulte el artículo [Referencia de permisos de Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference).

> [!NOTE]
> Debido a una limitación actual, las aplicaciones cliente nativas solo pueden llamar a Graph API de Azure AD si usan el permiso "Acceso al directorio de la organización". Esta restricción no se aplica a las aplicaciones web.

## <a name="configuring-multi-tenant-applications"></a>Configuración de aplicaciones multiempresa

Al registrar una aplicación en Azure AD, tal vez quiera que solo tengan acceso a la aplicación los usuarios de la organización. Como alternativa, tal vez quiera que los usuarios de organizaciones externas tengan acceso a la aplicación. Estos dos tipos de aplicaciones se denominan aplicaciones de un solo inquilino y aplicaciones multiinquilino. Esta sección describe cómo modificar la configuración de una aplicación de un solo inquilino para convertirla en una aplicación multiinquilino.

Es importante tener en cuenta las diferencias entre una aplicación de un solo inquilino y otra multiinquilino:  

- La finalidad de una aplicación de un solo inquilino es su uso en una sola organización. Suele tratarse de aplicaciones de línea de negocio (LOB) escritas por un desarrollador de la empresa. Una aplicación de un solo inquilino solo es accesible por usuarios con cuentas en el mismo inquilino que el de registro de aplicación. Como resultado, solo debe haberse aprovisionado en un directorio.
- La finalidad de una aplicación multiinquilino es su uso en muchas organizaciones. Se trata de aplicaciones web de software como servicio (SaaS) que suelen ser creadas por un proveedor de software independiente (ISV). Las aplicaciones multiinquilino deben haberse aprovisionado en cada inquilino en el que los usuarios necesiten tener acceso. Para los inquilinos distintos de aquél donde se registró la aplicación, es necesario para registrarlos el consentimiento de un usuario o administrador. Tenga en cuenta que las aplicaciones cliente nativas son multiinquilino de forma predeterminada porque están instaladas en el dispositivo del propietario del recurso. Consulte la sección [Información general sobre el marco de consentimiento](#overview-of-the-consent-framework) presentada anteriormente para más detalles sobre el marco de consentimiento.

Hacer que una aplicación sea multiinquilino requiere cambios tanto en el registro de la aplicación como en la propia aplicación web. Las secciones siguientes tratan ambos.

### <a name="changing-the-application-registration-to-support-multi-tenant"></a>Cambio del registro de la aplicación para que sea multiinquilino

Si está escribiendo una aplicación que quiere poner a disposición de clientes o asociados externos a la organización, debe actualizar la definición de la aplicación en Azure Portal.

> [!IMPORTANT]
> Azure AD necesita que el URI del identificador de las aplicaciones multiinquilino sea único globalmente. El URI de id. de aplicación es una de las maneras en que una aplicación se identifica en los mensajes de protocolo. Cuando la aplicación es de un solo inquilino, es suficiente con que el URI de id. de aplicación sea único en dicho inquilino. En el caso de una aplicación multiempresa, debe ser único a nivel global de forma que Azure AD pueda encontrar la aplicación entre todos los inquilinos.
> El carácter globalmente único viene impuesto por la necesidad de que el URI de id. de aplicación tenga un nombre de host que coincida con un dominio comprobado del inquilino de Azure AD. Por ejemplo, si el nombre del inquilino es contoso.onmicrosoft.com, un identificador URI de id. de aplicación válido sería https://contoso.onmicrosoft.com/myapp. Si el inquilino tiene el dominio comprobado contoso.com, un URI del identificador de aplicación válido también sería https://contoso.com/myapp. Si el URI de id. de aplicación no sigue este patrón, la configuración de una aplicación como multiinquilino dará error.

Para dar a los usuarios externos acceso a la aplicación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, haga clic en el servicio **Azure Active Directory**, haga clic en **Registros de aplicaciones** y encuentre y haga clic en la aplicación que desea configurar. Pasará a la página de registro principal de la aplicación, lo que abre la página **Configuración** de la aplicación.
4. En la página **Configuración**, haga clic en **Propiedades** y coloque el conmutador **Multiinquilino** en la posición **Sí**.

Después de realizar los cambios, los usuarios y administradores de otras organizaciones pueden conceder a los usuarios la posibilidad de iniciar sesión en la aplicación, permitiendo que la aplicación tenga acceso a recursos protegidos por su inquilino.

### <a name="changing-the-application-to-support-multi-tenant"></a>Cambio de la aplicación para que sea multiinquilino

La compatibilidad para aplicaciones multiinquilino se basa principalmente en el marco de consentimiento de Azure AD. El consentimiento es el mecanismo que permite que un usuario de otro inquilino conceda a la aplicación acceso a recursos protegidos por el inquilino del usuario. Esta experiencia se conoce como "consentimiento del usuario."

La aplicación web también puede ofrecer:

- La capacidad para que los administradores puedan "suscribir mi compañía." Esta experiencia, que se conoce como "consentimiento del administrador", ofrece al administrador la funcionalidad de conceder consentimiento en nombre de *todos los usuarios* de su organización. Solo un usuario que se autentique con una cuenta que pertenezca al rol Administrador global puede proporcionar el consentimiento del administrador; otros usuarios reciben un error.
- Una experiencia de inicio de sesión para los usuarios. Se espera que se proporcione al usuario un botón "Inicio de sesión" que redirigirá el explorador al punto de conexión `/authorize` de OAuth2.0 de Azure AD o al punto de conexión `/userinfo` de OpenID Connect. Estos extremos permiten que la aplicación obtenga información sobre el nuevo usuario inspeccionando el id_token. Después de la fase de inicio de sesión, se presentará al usuario una petición de consentimiento similar a la que se mostró en la sección [Información general sobre el marco de consentimiento](#overview-of-the-consent-framework).

Para más información sobre los cambios de aplicación necesarios para admitir el acceso multiinquilino y experiencias de inicio de sesión y suscripción, consulte:

- [Inicio de sesión de cualquier usuario de Azure Active Directory (AD) mediante el patrón de aplicación multiempresa](howto-convert-app-to-be-multi-tenant.md)
- La lista de [ejemplos de código multiinquilino](https://azure.microsoft.com/documentation/samples/?service=active-directory&term=multi-tenant).
- [Guía de inicio rápido: Incorporación de personalización de marca de empresa a la página de inicio de sesión de Azure AD](../fundamentals/customize-branding.md)

## <a name="enabling-oauth-20-implicit-grant-for-single-page-applications"></a>Habilitación de la concesión implícita de OAuth 2.0 para aplicaciones de página única

Las aplicaciones de página única (SPA) normalmente tienen una estructura con un front-end de JavaScript que se ejecuta en el explorador, que llama al back-end de la API web de la aplicación para llevar a cabo la lógica de negocios. Para las SPA hospedadas en Azure AD, se usa la concesión implícita de OAuth 2.0 para autenticar al usuario en Azure AD y obtener un token que puede usar para proteger las llamadas desde el cliente JavaScript de la aplicación hasta la API web de back-end.

Después de que el usuario haya dado su consentimiento, este mismo protocolo de autenticación se puede usar para obtener tokens para proteger las llamadas entre el cliente y otros recursos de API web configurados para la aplicación. Para conocer más sobre la concesión de autorización implícita y ayudarle a decidir si es adecuada para el escenario de su aplicación, consulte [Descripción del flujo de concesión implícita de OAuth2 de Azure Active Directory (AD)](v1-oauth2-implicit-grant-flow.md).

De forma predeterminada, la concesión implícita de OAuth 2.0 está deshabilitada para las aplicaciones. Puede habilitar la concesión implícita de OAuth 2.0 para la aplicación estableciendo el valor `oauth2AllowImplicitFlow` en el [manifiesto de aplicación](reference-app-manifest.md).

### <a name="to-enable-oauth-20-implicit-grant"></a>Para habilitar la concesión implícita de OAuth 2.0

> [!NOTE]
> Para más información sobre cómo editar el manifiesto de aplicación, asegúrese primero de revisar la sección anterior, [Configuración de una aplicación de recursos para exponer las API web](#configuring-a-resource-application-to-expose-web-apis).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, haga clic en el servicio **Azure Active Directory**, haga clic en **Registros de aplicaciones** y encuentre y haga clic en la aplicación que desea configurar. Pasará a la página de registro principal de la aplicación, lo que abre la página **Configuración** de la aplicación.
4. Cambie a la página **Editar manifiesto**, haciendo clic en **Manifiesto** desde la página de registro de la aplicación. Se abre un editor de manifiestos basado en web que le permite **Editar** el manifiesto desde el portal. Busque y establezca el valor de "oauth2AllowImplicitFlow" en "true." De forma predeterminada, se establece en "false."
   
  ```json
  "oauth2AllowImplicitFlow": true,
  ```
5. Guarde el manifiesto actualizado. Una vez guardado, la API web estará ahora configurada para usar la concesión implícita de OAuth 2.0 para autenticar a los usuarios.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre estas otras guías de inicio rápido de administración de aplicaciones relacionadas para aplicaciones con el punto de conexión de Azure AD v1.0:
- [Adición de una aplicación en Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Eliminación de una aplicación de Azure AD](quickstart-v1-remove-azure-ad-app.md)

Para obtener más información acerca de los dos objetos de Azure AD que representan una aplicación registrada y la relación entre ellos, consulte [Objetos de aplicación y de entidad de servicio](app-objects-and-service-principals.md).

Para obtener más información acerca de las directrices de personalización de marca que hay que usar al desarrollar aplicaciones con Azure Active Directory, consulte [Directrices de personalización de marca para aplicaciones](howto-add-branding-in-azure-ad-apps.md).