---
title: Administración de la autenticación en Azure Maps | Microsoft Docs
description: Puede usar Azure Portal para administrar la autenticación en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 68c3c8ac39f5803e01ee1038ec85ddb96ac80b30
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242692"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador cliente para admitir la autenticación de Clave compartida o Azure Active Directory (Azure AD).

Puede encontrar la información de autenticación accediendo a la página **Autenticación**, en **Configuración**, en Azure Portal. Vaya a la cuenta. Seleccione **Autenticación** en el menú.


## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Para ver la información de autenticación, vaya a la opción **Autenticación** del menú Configuración.

![Visualización de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)

 Para obtener información sobre la autenticación y Azure Maps, consulte [Autenticación con Azure Maps](https://aka.ms/amauth).


## <a name="configure-azure-ad-app-registration"></a>Configuración del registro de aplicación de Azure AD

Una vez que se crea una cuenta de Azure Maps, se requiere un vínculo entre su inquilino de Azure AD y el recurso de Azure Maps. 

1. Vaya a la hoja de Azure AD y cree un registro de aplicación con un nombre y la dirección URL de inicio de sesión como la página principal de la aplicación web o API, como "https://localhost/". Si ya tiene una aplicación registrada, vaya al paso 2.

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

    ![Registro de aplicación](./media/how-to-manage-authentication/app-create.png)

2. Asigne permisos delegados de API a Azure Maps yendo a la aplicación en Registros de aplicaciones y haga clic en **Configuración**.  Seleccione **Permisos necesarios** y luego **Agregar**. Busque y seleccione Azure Maps en **Seleccionar una API** y haga clic en **Seleccionar**.

    ![Permisos de API de aplicación](./media/how-to-manage-authentication/app-permissions.png)

3. Por último, en Seleccionar permisos, elija Acceder a Azure Maps y haga clic en Seleccionar.

    ![Selección de permisos de API de aplicación](./media/how-to-manage-authentication/select-app-permissions.png)

4. Siga el paso a o b, dependiendo de la implementación de autenticación.

    1. Si la aplicación se ha diseñado para utilizar la autenticación de token de usuario con nuestro SDK web de Azure Maps, debe habilitar `oauthEnableImplicitFlow` estableciendo el valor en True en la sección Manifiesto de la página de detalles del registro de aplicación.
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    2. Si su aplicación utiliza autenticación de servidor o aplicación, vaya a la hoja **Claves** en el registro de aplicación y cree una contraseña o cargue un certificado de clave pública en el registro de aplicación. Si crea una contraseña, una vez que **guarde**, copie la contraseña para usarla más adelante y la almacene de forma segura, se usará para adquirir tokens de Azure AD.

       ![Claves de la aplicación](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Concesión de RBAC en Azure Maps

Una vez que una cuenta de Azure Maps se ha asociado con el inquilino de Azure AD, se puede conceder control de acceso mediante la asignación del usuario o la aplicación a los roles de control de acceso de Azure Maps disponibles.

1. Vaya a la opción **Control de acceso**, haga clic en **Asignaciones de roles** y **Agregar asignación de roles**.

    ![Concesión de RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. En la ventana emergente Asignación de roles, seleccione el **rol** Lector de datos (versión preliminar) de Azure Maps, **Asignar acceso a**: Usuario, grupo o entidad de servicio de Azure AD, **seleccione** Usuario o aplicación en la lista desplegable, y **Guardar**.

    ![Agregar asignación de roles](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Visualización de los roles de RBAC de Azure Maps disponibles

Para ver los roles de control de acceso basado en roles disponibles para que se pueda conceder acceso a Azure Maps, vaya a la opción **Control de acceso (IAM)**, haga clic en **Roles** y busque los roles que empiecen por **Azure Maps**.

![Visualización de roles disponibles](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-role-based-access-control-rbac"></a>Visualización del control de acceso basado en roles (RBAC) de Azure Maps

Azure AD permite controlar el acceso de forma granular a través del control de acceso basado en roles (RBAC). 

Para ver los usuarios o las aplicaciones a los que se han concedido roles RBAC de Azure Maps, vaya a la opción **Control de acceso (IAM)**, seleccione **Asignaciones de roles** y filtre por **Azure Maps**. Todos los roles disponibles aparecerán a continuación.

![Visualización de RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Solicitud de tokens de Azure Maps

Una vez que la aplicación se ha registrado y asociado con Azure Maps, ahora puede solicitar tokens de acceso.

* Si la aplicación tiene la intención de usar la autenticación de token de usuario con nuestro Azure Maps Web SDK (Web), necesita configurar su página HTML con el identificador de cliente de Azure Maps y el identificador de aplicación de Azure AD.

* Para las aplicaciones que utilizan la autenticación de servidor/aplicación, solicite un token del punto de conexión de inicio de sesión de Azure AD `https://login.microsoftonline.com` con el identificador de recurso de Azure AD `https://atlas.microsoft.com/`, el identificador de cliente de Azure Maps, el identificador de aplicación de Azure AD y la contraseña o certificado de registro de Azure AD App.

Para obtener más información sobre la solicitud de tokens de acceso de Azure AD para usuarios y entidades de servicio, consulte el artículo sobre [escenarios de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la autenticación de Azure AD y el SDK web de Azure Maps, consulte el artículo sobre [Azure AD y el SDK web de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).