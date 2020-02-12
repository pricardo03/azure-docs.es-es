---
title: Administración de la autenticación | Microsoft Azure Maps
description: Puede usar Azure Portal para administrar la autenticación en Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: f856aebe5e3acaca142e460d18ec8c6498b18787
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989311"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador cliente para admitir la autenticación de Clave compartida y Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Después de crear la cuenta de Azure Maps, se generan las claves principal y secundaria. Se recomienda usar la clave principal como clave de suscripción al llamar a Azure Maps mediante la [autenticación de clave compartida](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication). La clave secundaria se puede usar, por ejemplo, para cambios de clave graduales. Para obtener más información, consulte [Autenticación con Azure Maps](https://aka.ms/amauth).

Puede ver sus detalles de autenticación en Azure Portal. Vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="configure-azure-ad-app-registration"></a>Configuración del registro de aplicación de Azure AD

1. Seleccione **Azure Active Directory** de la lista de servicios de Azure en Azure Portal.  Seleccione **Registros de aplicaciones** y, luego, **Nuevo registro**.  A continuación, Rellene el campo **Nombre**, elija **Support account type** (Tipo de cuenta de soporte técnico) y haga clic en **Registrarse**.  Si ya tiene una aplicación registrada, vaya al paso 2. 

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

    ![Detalles de registro de la aplicación](./media/how-to-manage-authentication/app-create.png)

2. Para asignar permisos de API delegados a Azure Maps, vaya a la aplicación en **Registros de aplicaciones**. Luego, seleccione **Permisos de API** y seleccione **Agregar un permiso**. Busque y seleccione **Azure Maps** en **APIs my organization uses** (API que usa mi organización).

    ![Permisos de API de aplicación](./media/how-to-manage-authentication/app-permissions.png)

3. Marque **Access Azure Maps** (Acceder a Azure Maps) y, luego, haga clic en **Agregar permisos**.

    ![Selección de permisos de API de aplicación](./media/how-to-manage-authentication/select-app-permissions.png)

4. Complete el paso A o B, dependiendo de su método de autenticación. 

    1. Si la aplicación usa la autenticación mediante tokens de usuario con el SDK web de Azure Maps, habilite `oauth2AllowImplicitFlow`. Para habilitar `oauth2AllowImplicitFlow`, establézcalo en true en la sección de manifiesto del registro de la aplicación. 
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    2. Si la aplicación usa una autenticación de servidor o aplicación, vaya a la hoja **Certificates & secrets** (Certificados y secretos) en la página de registro de la aplicación y haga clic en **New client secret** (Nuevo secreto de cliente) o cargue un certificado de clave pública en el registro de la aplicación. Si crea una contraseña, después de hacer clic en **Agregar**, cópiela para más tarde y guárdela de forma segura. Usará esta contraseña para adquirir tokens de Azure AD.

       ![Claves de la aplicación](./media/how-to-manage-authentication/app-keys.png)

       ![Agregar clave](./media/how-to-manage-authentication/add-key.png)


## <a name="grant-role-based-access-control-rbac-to-azure-maps"></a>Concesión control de acceso basado en roles (RBAC) a Azure Maps

Después de asociar una cuenta de Azure Maps a su inquilino de Azure AD, puede conceder el control de acceso. El control de acceso se concede mediante la asignación de un usuario, un grupo o una aplicación a uno o varios roles de control de acceso de Azure Maps.

1. Vaya a su **cuenta de Azure Maps**. Seleccione **Control de acceso (IAM)** y seleccione **Asignación de roles**.

    ![Concesión de RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. En la ventana **Asignación de roles**, en **Rol**, seleccione **Lector de fechas de Azure Maps (versión preliminar)** . En **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**. Seleccione el usuario o la aplicación. Seleccione **Guardar**.

    ![Agregar asignación de roles](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Visualización de los roles de RBAC de Azure Maps disponibles

Para ver los roles de control de acceso basados en roles (RBAC) que están disponibles para Azure Maps, vaya a **Control de acceso (IAM)** , seleccione **Roles** y luego busque los roles que comiencen con **Azure Maps**. Esos son los roles a los que puede conceder acceso.

![Visualización de roles disponibles](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Ver RBAC de Azure Maps

RBAC proporciona control de acceso pormenorizado.

Para ver los usuarios y las aplicaciones a las que se les han concedido roles de RBAC para Azure Maps, vaya a **Control de acceso (IAM)** , seleccione **Asignaciones de roles** y luego filtre con **Azure Maps**.

![Ver usuarios y aplicaciones con roles concedidos en RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Solicitud de tokens de Azure Maps

Después de registrar su aplicación y asociarla a Azure Maps, puede solicitar los tokens de acceso.

* Si su aplicación usa una autenticación de token de usuario con el SDK web de Azure Maps, debe configurar su página HTML con el id. de cliente de Azure Maps y el id. de aplicación de Azure AD.

* Si su aplicación usa una autenticación de servidor o aplicación, debe solicitar un token del punto de conexión de token de Azure AD `https://login.microsoftonline.com` con el identificador del recurso de Azure AD `https://atlas.microsoft.com/`, el identificador de cliente de Azure Maps, el identificador de la aplicación de Azure AD y la contraseña o certificado de registro de la aplicación de Azure AD.

| Entorno de Azure   | Punto de conexión del token de Azure AD | Id. de recurso de Azure |
| --------------------|-------------------------|-------------------|
| Azure Public        | https://login.microsoftonline.com | https://atlas.microsoft.com/ |
| Azure Government    | https://login.microsoftonline.us  | https://atlas.microsoft.com/ | 

Para obtener más información sobre la solicitud de tokens de acceso de Azure AD para usuarios y entidades de servicio, consulte [Escenarios de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la autenticación de Azure AD y el SDK web de Azure Maps, consulte el artículo sobre [Azure AD y el SDK web de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Aprenda a ver las métricas de uso de API para la cuenta de Azure Maps:
> [!div class="nextstepaction"] 
> [Visualización de métricas de uso](how-to-view-api-usage.md)

Para ver una lista de ejemplos que muestran cómo integrar Azure Active Directory (AAD) con Azure Maps, consulte:

> [!div class="nextstepaction"]
> [Muestras de autenticación de Azure AD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)
