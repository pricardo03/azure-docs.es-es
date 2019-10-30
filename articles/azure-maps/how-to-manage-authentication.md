---
title: Administración de la autenticación en Azure Maps | Microsoft Docs
description: Puede usar Azure Portal para administrar la autenticación en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/11/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2f4a3d791e6b5d6ff20c09408d1a0bf5995c32fd
ms.sourcegitcommit: 8074f482fcd1f61442b3b8101f153adb52cf35c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72756555"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, se crean las claves y el identificador cliente para admitir la autenticación de Clave compartida o Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Puede ver sus detalles de autenticación en Azure Portal. Vaya a su cuenta y seleccione **Autenticación** en el menú **Configuración**.

![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)

 Para obtener más información, consulte [Autenticación con Azure Maps](https://aka.ms/amauth).


## <a name="set-up-azure-ad-app-registration"></a>Configurar el registro de aplicaciones de Azure AD

Después de crear una cuenta de Azure Maps, debe establecer un vínculo entre su inquilino de Azure AD y el recurso de Azure Maps.

1. Vaya a la hoja de Azure AD y cree un registro de aplicación. Proporcione un nombre para el registro. En el cuadro de la **dirección URL de inicio de sesión**, proporcione la página principal de la aplicación web y la API (por ejemplo, https:\//localhost/). Si ya tiene una aplicación registrada, vaya al paso 2.

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

    ![Detalles de registro de la aplicación](./media/how-to-manage-authentication/app-create.png)

2. Para asignar permisos de API delegados a Azure Maps, vaya a la aplicación en **Registros de aplicaciones** y luego seleccione **Configuración**.  Seleccione **Permisos necesarios** y **Agregar**. Busque y seleccione **Azure Maps** en **Seleccione una API** y, a continuación, pulse el botón **Seleccionar**.

    ![Permisos de API de aplicación](./media/how-to-manage-authentication/app-permissions.png)

3. En **Seleccionar permisos**, seleccione **Acceder a Azure Maps** y, a continuación, pulse el botón **Seleccionar**.

    ![Selección de permisos de API de aplicación](./media/how-to-manage-authentication/select-app-permissions.png)

4. Complete el paso A o B, dependiendo de su método de autenticación.

    1. Si su aplicación usa una autenticación de token de usuario con el SDK web de Azure Maps, habilite `oauthEnableImplicitFlow` estableciéndolo en "true" en la sección "Manifiesto" de la página de detalles de registro de la aplicación.
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    2. Si su aplicación usa una autenticación de servidor o aplicación, vaya a la hoja **Claves** en el registro de aplicación y cree una contraseña o cargue un certificado de clave pública en el registro de aplicación. Si crea una contraseña, después de seleccionar **Guardar**, cópiela para más tarde y guárdela de forma segura. Usará esta contraseña para adquirir tokens de Azure AD.

       ![Claves de la aplicación](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Concesión de RBAC en Azure Maps

Después de asociar una cuenta de Azure Maps con su inquilino de Azure AD, puede conceder el control de acceso si asigna un usuario, un grupo o una aplicación a uno o más roles de control de acceso de Azure Maps.

1. Vaya a **Control de acceso (IAM)** , seleccione **Asignaciones de roles** y luego seleccione **Agregar asignación de roles**.

    ![Concesión de RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. En la ventana **Agregar asignación de roles**, en **Rol**, seleccione **Lector de fechas de Azure Maps (versión preliminar)** . En **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**. En **Seleccionar**, seleccione el usuario o la aplicación. Seleccione **Guardar**.

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

* Si su aplicación usa una autenticación de servidor o aplicación, debe solicitar un token del punto de conexión de inicio de sesión de Azure AD `https://login.microsoftonline.com` con el id. del recurso de Azure AD `https://atlas.microsoft.com/`, el id. de cliente de Azure Maps, el id. de la aplicación de Azure AD y la contraseña o certificado de registro de la aplicación de Azure AD.

Para obtener más información sobre la solicitud de tokens de acceso de Azure AD para usuarios y entidades de servicio, consulte el artículo sobre los [escenarios de autenticación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la autenticación de Azure AD y el SDK web de Azure Maps, consulte el artículo sobre [Azure AD y el SDK web de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Aprenda a ver las métricas de uso de API para la cuenta de Azure Maps:
> [!div class="nextstepaction"] 
> [Visualización de métricas de uso](how-to-view-api-usage.md)

Para ver una lista de ejemplos que muestran cómo integrar Azure Active Directory (AAD) con Azure Maps, consulte:

> [!div class="nextstepaction"]
> [Ejemplos de autenticación de AAD](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples)