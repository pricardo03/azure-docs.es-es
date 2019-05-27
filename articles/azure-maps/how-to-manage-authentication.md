---
title: Administración de la autenticación en Azure Maps | Microsoft Docs
description: Puede usar Azure Portal para administrar la autenticación en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/14/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 617adbcda70799aa07248945bbc27f9d95aa77a3
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65952570"
---
# <a name="manage-authentication-in-azure-maps"></a>Administración de la autenticación en Azure Maps

Después de crear una cuenta de Azure Maps, un identificador de cliente y las claves se crean para admitir la autenticación Shared Key o Azure Active Directory (Azure AD).

## <a name="view-authentication-details"></a>Visualización de los detalles de la autenticación

Puede ver los detalles de autenticación en el portal de Azure. Vaya a su cuenta y seleccione **autenticación** en el **configuración** menú.

![Detalles de la autenticación](./media/how-to-manage-authentication/how-to-view-auth.png)

 Para obtener más información, consulte [autenticación con Azure Maps](https://aka.ms/amauth).


## <a name="set-up-azure-ad-app-registration"></a>Configurar el registro de aplicación de Azure AD

Después de crear una cuenta de Azure Maps, deberá establecer un vínculo entre su inquilino de Azure AD y el recurso de Azure Maps.

1. Vaya a la hoja de Azure AD y cree un registro de aplicación. Proporcione un nombre para el registro. En el **dirección URL de inicio de sesión** cuadro, proporcione la página principal de la aplicación web / API (por ejemplo, https:\//localhost/). Si ya tiene una aplicación registrada, vaya al paso 2.

    ![Registro de aplicación](./media/how-to-manage-authentication/app-registration.png)

    ![Detalles del registro de aplicación](./media/how-to-manage-authentication/app-create.png)

2. Para asignar permisos delegados de API a Azure Maps, vaya a la aplicación sometida a **registros de aplicaciones**y, a continuación, seleccione **configuración**.  Seleccione **permisos necesarios**y, a continuación, seleccione **agregar**. Busque y seleccione **Azure Maps** en **seleccionar una API**y, a continuación, seleccione el **seleccione** botón.

    ![Permisos de aplicación de API](./media/how-to-manage-authentication/app-permissions.png)

3. En **seleccionar permisos**, seleccione **acceso Azure Maps**y, a continuación, seleccione el **seleccione** botón.

    ![Seleccione los permisos de aplicación de API](./media/how-to-manage-authentication/select-app-permissions.png)

4. Complete el paso un o b, dependiendo del método de autenticación.

    1. Si su aplicación utiliza la autenticación de token de usuario con el SDK de Web de Azure Maps, habilitar `oauthEnableImplicitFlow` si se establece en true en la sección de manifiesto de la página de detalles del registro de aplicación.
    
       ![Manifiesto de aplicación](./media/how-to-manage-authentication/app-manifest.png)

    2. Si su aplicación utiliza la autenticación de servidor o la aplicación, vaya a la **claves** hoja en el registro de la aplicación y crear una contraseña o cargar un certificado de clave pública en el registro de aplicación. Si crea una contraseña, después de seleccionar **guardar**, copie la contraseña para su uso posterior y almacenarlo de forma segura. Usará esta contraseña para adquirir tokens de Azure AD.

       ![Claves de la aplicación](./media/how-to-manage-authentication/app-keys.png)


## <a name="grant-rbac-to-azure-maps"></a>Concesión de RBAC en Azure Maps

Después de asociar una cuenta de Azure Maps con su inquilino de Azure AD, puede otorgar el control de acceso mediante la asignación de un usuario o aplicación a uno o varios roles de control de acceso de Azure Maps.

1. Vaya a **control de acceso (IAM)**, seleccione **las asignaciones de roles**y, a continuación, seleccione **Agregar asignación de roles**.

    ![Concesión de RBAC](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. En el **Agregar asignación de roles** ventana, en **rol**, seleccione **fecha lector (versión preliminar) de Azure Maps**. En **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**. En **seleccione**, seleccione el usuario o aplicación. Seleccione **Guardar**.

    ![Agregar asignación de roles](./media/how-to-manage-authentication/add-role-assignment.png)

## <a name="view-available-azure-maps-rbac-roles"></a>Visualización de los roles de RBAC de Azure Maps disponibles

Para ver los roles (RBAC) de control de acceso basado en roles que están disponibles para Azure Maps, vaya a **control de acceso (IAM)**, seleccione **Roles**, y, a continuación, a partir de búsqueda para los roles con **deAzureMaps**. Estos son los roles que se puede conceder acceso a.

![Visualización de roles disponibles](./media/how-to-manage-authentication/how-to-view-avail-roles.png)


## <a name="view-azure-maps-rbac"></a>Ver asignaciones de Azure RBAC

RBAC proporciona control de acceso granular.

Para ver los usuarios y aplicaciones que se han concedido RBAC de Azure Maps, vaya a **Access Control (IAM)**, seleccione **las asignaciones de roles**y, a continuación, filtre por **Azure Maps**.

![Ver los usuarios y aplicaciones conceden RBAC](./media/how-to-manage-authentication/how-to-view-amrbac.png)


## <a name="request-tokens-for-azure-maps"></a>Solicitud de tokens de Azure Maps

Después de registrar la aplicación y lo ha asociado con Azure Maps, puede solicitar tokens de acceso.

* Si la aplicación utiliza la autenticación de token de usuario con el SDK de Web de Azure Maps, deberá configurar la página HTML con el identificador de cliente de Azure Maps y el identificador de aplicación de Azure AD.

* Si su aplicación utiliza la autenticación de servidor o la aplicación, deberá solicitar un token de punto de conexión de inicio de sesión de Azure AD `https://login.microsoftonline.com` con el identificador de recurso de Azure AD `https://atlas.microsoft.com/`, el identificador de cliente de Azure Maps, el identificador de aplicación de Azure AD y el registro de aplicación de Azure AD la contraseña o certificado.

Para obtener más información sobre las solicitudes de tokens de acceso de Azure AD para los usuarios y entidades de servicio, consulte [escenarios de autenticación para Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre la autenticación de Azure AD y el SDK web de Azure Maps, consulte el artículo sobre [Azure AD y el SDK web de Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-use-map-control).

Aprenda a ver las métricas de uso de API para la cuenta de Azure Maps:
> [!div class="nextstepaction"] 
> [Visualización de métricas de uso](how-to-view-api-usage.md)