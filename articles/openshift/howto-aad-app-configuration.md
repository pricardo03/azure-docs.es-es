---
title: Crear un registro de aplicación de Azure AD y un usuario de Azure Red Hat OpenShift | Microsoft Docs
description: Aprenda a crear a una entidad de servicio, generar una URL de devolución de llamada de secreto y autenticación de cliente y crear un nuevo usuario de Active Directory para probar aplicaciones en un clúster de Microsoft Azure Red Hat OpenShift.
author: tylermsft
ms.author: twhitney
ms.service: openshift
manager: jeconnoc
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/06/2019
ms.openlocfilehash: de3f3c30848d26ea399bcccc29a6149a149f6a55
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078526"
---
# <a name="create-an-azure-ad-app-registration-and-user-for-azure-red-hat-openshift"></a>Crear un registro de aplicación de Azure AD y un usuario de Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift necesita permisos para realizar tareas en nombre del clúster. Si su organización ya no tiene un registro de aplicación de Azure Active Directory (Azure AD) que se usará como la entidad de servicio, siga estas instrucciones para crear uno.

En este tema concluye con instrucciones para crear un nuevo usuario de Azure AD que necesitará para tener acceso a aplicaciones que se ejecutan en el clúster de Azure Red Hat OpenShift.

Si aún no ha creado un inquilino de Azure AD, siga las instrucciones de [crear un inquilino de Azure AD para Azure Red Hat OpenShift](howto-create-tenant.md) antes de continuar con estas instrucciones.

## <a name="create-a-new-app-registration"></a>Creación de un registro de aplicaciones

Una aplicación que desea usar las capacidades de Azure AD debe registrarse primero en un inquilino de Azure AD. Este proceso de registro implica proporcionar detalles de Azure AD acerca de la aplicación, como la dirección URL donde se encuentra la aplicación, la dirección URL para enviar respuestas cuando un usuario se autentica, el URI que identifica la aplicación y así sucesivamente.

1. En el [portal Azure](https://portal.azure.com), asegúrese de que el inquilino aparece bajo el nombre de usuario en la parte superior derecha del portal:

    ![Captura de pantalla del portal de inquilino aparece en la esquina superior derecha][tenantcallout] si se muestra el inquilino incorrecto, haga clic en el nombre de usuario en la esquina superior derecha, a continuación, haga clic en **Cambiar directorio**y seleccione el inquilino correcto de la **todas Directorios** lista.

2. Abra el [hoja registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) y haga clic en **nuevo registro de aplicaciones**.
3. En el **crear** panel, escriba un nombre descriptivo para el objeto de aplicación.
4. Asegúrese de que **tipo de aplicación** está establecido en *aplicación Web o API*.
5. Crear un **dirección URL de inicio de sesión** con el siguiente patrón:

    ```
    https://<cluster-name>.<azure-region>.cloudapp.azure.com
    ```

    . . . donde `<cluster-name>` es el nombre deseado del clúster de Azure Red Hat OpenShift y `<azure-region>` es el [región de Azure que hospeda el clúster de Azure Red Hat OpenShift](supported-resources.md#azure-regions). Por ejemplo, si el nombre del clúster que se va a `contoso`, y va a crear en el `eastus` región, el nombre de dominio completo (FQDN) que especifique para la **dirección URL de inicio de sesión** sería `https://contoso.eastus.cloudapp.azure.com`

    > [!IMPORTANT]
    > El nombre del clúster debe estar en minúscula y la dirección URL del FQDN debe ser única.
    > Asegúrese de que elegir un nombre lo suficientemente distinto para el clúster.

    Anote el nombre del clúster y la dirección URL de inicio de sesión, necesitará más adelante a aplicaciones de acceso que se ejecutan en el clúster. Se hará referencia al nombre del clúster como `CLUSTER_NAME`y esta URL de inicio de sesión como `FQDN` en el [crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.

6. Garantizar su **dirección URL de inicio de sesión** valida el valor con una marca de verificación verde. (Presione la tecla Tab para el foco fuera de la *dirección URL de inicio de sesión* campo y ejecutar la comprobación de validación.)
7. Haga clic en el **crear** botón para crear el objeto de aplicación de Azure AD.
8. En el **aplicación registrada** página que aparece, copie el **Id. de aplicación**. Nos referiremos a este valor como `APPID` en el [crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.

    ![Captura de pantalla del cuadro de texto Id. de aplicación][appidimage]
    
Para obtener más información sobre los objetos de aplicación de Azure, consulte [Application y objetos de entidad de servicio en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals).

Para obtener más información sobre cómo crear una nueva aplicación de Azure AD, consulte [registrar una aplicación con el punto de conexión de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

### <a name="create-a-client-secret"></a>Creación de un secreto de cliente

Ahora está listo para generar un secreto de cliente para autenticar la aplicación a Azure Active Directory.

1. Desde el **aplicación registrada** página, haga clic en **configuración** para abrir la configuración de la aplicación registrada.
2. En el **configuración** panel que aparece, haga clic en **claves**.  El **claves** aparecerá el panel.
![Captura de pantalla de la página de clave create en el portal][createkeyimage]
3. Proporcione una clave **descripción**.
4. Establecer un valor para **Expires**, por ejemplo *en 2 años*.
5. Haga clic en **guardar** y aparecerá el valor de clave.
6. Copie el valor de clave. Nos referiremos a este valor como `SECRET` en el [crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.

### <a name="create-a-reply-url"></a>Crear una dirección URL de respuesta

Azure AD usa el objeto app *dirección URL de respuesta* para especificar la devolución de llamada se utiliza para autorizar la aplicación. En el caso de una API web o aplicación web, la dirección URL de respuesta es la ubicación donde Azure AD enviará la respuesta de autenticación, incluido un token si la autenticación fue correcta.

Incluso la falta de coincidencia más pequeño (finales no tiene una barra diagonal, distinguen mayúsculas de minúsculas) provocará un error en la operación de emisión de tokens y no podrá iniciar sesión.

1. Vuelva a la **configuración** panel (puede hacer clic en **configuración** en la ruta de navegación en la parte superior del portal) y haga clic en **direcciones URL de respuesta** a la derecha.  El **direcciones URL de respuesta** aparecerá el panel.
2. Será la primera dirección URL de respuesta en la lista el `FQDN` valor del paso 6 en [crear un nuevo registro de aplicación](#create-a-new-app-registration). Editar y anexar `/oauth2callback/Azure%20AD`.  Por ejemplo, la dirección URL de respuesta ahora debe ser similar `https://mycluster.eastus.cloudapp.azure.com/oauth2callback/Azure%20AD`
3. Haga clic en **guardar** para guardar la dirección URL de respuesta.

## <a name="create-a-new-active-directory-user"></a>Crear un nuevo usuario de Active Directory

Crear un nuevo usuario en Active Directory para usarla para iniciar sesión en la aplicación se ejecuta en el clúster de Azure Red Hat OpenShift.

1. Vaya a la [usuarios: todos los usuarios](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) hoja.
2. Haga clic en **+ Nuevo usuario**. El **usuario** aparecerá el panel.
3. Escriba un **nombre** que le gustaría para este usuario.
4. Crear un **nombre de usuario** según el nombre del inquilino ha creado con `.onmicrosoft.com` anexado al final. Por ejemplo, `yourUserName@yourTenantName.onmicrosoft.com`. Anote este nombre de usuario. La necesitará para iniciar sesión para usar la aplicación en el clúster.
5. Haga clic en **rol del directorio** y seleccione **administrador Global** y, a continuación, haga clic en **Aceptar** en la parte inferior del panel.
6. En el medio de la **usuario** panel, haga clic en **Mostrar contraseña** y anote la contraseña temporal. Después de iniciar sesión por primera vez, se le pedirá que la restablezca.
7. En la parte inferior del panel, haga clic en **crear** para crear el usuario.

## <a name="resources"></a>Recursos

* [Las aplicaciones y los objetos de entidad de servicio en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals)  
* [Inicio rápido: Registrar una aplicación con el punto de conexión de Azure Active Directory v1.0](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app)  

[appidimage]: ./media/howto-create-tenant/get-app-id.png
[createkeyimage]: ./media/howto-create-tenant/create-key.png
[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="next-steps"></a>Pasos siguientes

Si se han cumplido todos los [requisitos previos de Azure Red Hat OpenShift](howto-setup-environment.md), está listo para crear su primer clúster!

Pruebe el tutorial:
> [!div class="nextstepaction"]
> [Crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md)
