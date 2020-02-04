---
title: Administración de la autenticación | Microsoft Azure Maps
description: Puede usar Azure Portal para administrar la autenticación en Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 1f7f128898089292a8ccd92686af5d68fe328f3c
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76766208"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador cliente para admitir la autenticación de Clave compartida y Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Después de crear la cuenta de Azure Maps, se generan las claves principal y secundaria. Use la clave principal como clave de suscripción, en ocasiones, estos nombres se usan indistintamente. La clave secundaria se puede usar, por ejemplo, para cambios de clave graduales. En cualquier caso, se necesita una clave para llamar a Azure Maps. Este proceso se denomina [autenticación de clave compartida](https://docs.microsoft.com/azure/azure-maps/azure-maps-authentication#shared-key-authentication). Para obtener más información sobre la autenticación de clave compartida y Azure AD, consulte [Autenticación con Azure Maps](https://aka.ms/amauth).

Puede ver sus detalles de autenticación en Azure Portal. Vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)


## <a name="set-up-azure-ad-app-registration"></a>Configurar el registro de aplicaciones de Azure AD

Después de crear una cuenta de Azure Maps, debe establecer un vínculo entre su inquilino de Azure AD y el recurso de Azure Maps.

1. Seleccione **Azure Active Directory** en el menú del portal. Proporcione un nombre para el registro. Haga clic en **Registros de aplicaciones** y, luego, haga clic en **Nuevo registro**. En el cuadro **URI de redirección**, proporcione la página principal de la aplicación web. Por ejemplo, https://localhost/. Si ya tiene una aplicación registrada, vaya al paso 2.

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

    ![Detalles de registro de la aplicación](./media/how-to-manage-authentication/app-create.png)

2. Para asignar permisos de API delegados a Azure Maps, vaya a la aplicación en **Registros de aplicaciones** y luego seleccione **Permisos de API**. Seleccione **Agregar permiso**. Busque y seleccione **Azure Maps** en **Seleccionar una API**.

    ![Permisos de API de aplicación](./media/how-to-manage-authentication/app-permissions.png)

3. En **Seleccionar permisos**, active la casilla **suplantación de usuario** y, a continuación, haga clic en el botón **Seleccionar** en la parte inferior.

    ![Selección de permisos de API de aplicación](./media/how-to-manage-authentication/select-app-permissions.png)

4. Complete el paso A o B, dependiendo de su método de autenticación.

    1. Si su aplicación usa una autenticación de token de usuario con el SDK web de Azure Maps, habilite `oauth2AllowImplicitFlow` estableciéndolo en "true" en la sección "Manifiesto" del registro de la aplicación.
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    2. Si la aplicación usa una autenticación de servidor o aplicación, vaya a la hoja **Certificados y secretos** en el registro de la aplicación y cree una contraseña o cargue un certificado de clave pública en el registro de la aplicación. Si crea una contraseña, guárdela en un lugar seguro para utilizarla más adelante. Usará esta contraseña para adquirir tokens de Azure AD.

       ![Claves de la aplicación](./media/how-to-manage-authentication/app-keys.png)


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
