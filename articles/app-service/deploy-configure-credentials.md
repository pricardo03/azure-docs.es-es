---
title: 'Configuración de credenciales de implementación: Azure App Service | Microsoft Docs'
description: Aprenda a usar las credenciales de implementación de Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnoc
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/10/2019
ms.author: cephalin;byvinyal
ms.custom: seodec18
ms.openlocfilehash: df874ab77c88f05b048b1f9d10873943b7bebf36
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884394"
---
# <a name="configure-deployment-credentials-for-azure-app-service"></a>Configuración de credenciales de implementación para Azure App Service
[Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714) admite dos tipos de credenciales para la [implementación de GIT local](deploy-local-git.md) y la [implementación FTP/S](deploy-ftp.md). Estas credenciales no son el mismo que las credenciales de Azure Active Directory.

* **Credenciales de nivel de usuario**: un conjunto de credenciales para toda la cuenta de Azure. Se puede utilizar para implementar App Service en cualquier aplicación o suscripción para la que la cuenta de Azure tenga permiso de acceso. Este es también el conjunto predeterminado que aparece en la interfaz gráfica de usuario del portal (como la **información general** y las **propiedades** de la [página de recursos](../azure-resource-manager/manage-resources-portal.md#manage-resources) de la aplicación). Cuando a un usuario se le otorga acceso a la aplicación a través del Control de acceso basado en rol (RBAC) o mediante permisos de coadministrador, ese usuario puede usar sus propias credenciales de nivel de usuario hasta que se le revoque el acceso. No comparta estas credenciales con otros usuarios de Azure.

* **Credenciales de nivel de aplicación**: un conjunto de credenciales para cada aplicación. Se puede utilizar para implementar únicamente en esa aplicación. Las credenciales de cada aplicación se generan automáticamente cuando se crea la aplicación. Recuerde que no se pueden configurar manualmente, pero se pueden restablecer en cualquier momento. Para que un usuario obtenga acceso a las credenciales de nivel de aplicación a través del control de acceso basado en rol, ese usuario debe ser un colaborador o tener un rol superior en la aplicación. A los lectores no se les permite publicar contenido y no pueden obtener acceso a dichas credenciales.

## <a name="userscope"></a>Establecimiento y restablecimiento de credenciales de nivel de usuario

Puede configurar las credenciales de nivel de usuario en cualquier [página de recursos](../azure-resource-manager/manage-resources-portal.md#manage-resources) de la aplicación. Independientemente de en qué aplicación configure estas credenciales, son válidas para todas las aplicaciones y para todas las suscripciones de su cuenta de Azure. 

Para configurar las credenciales de nivel de usuario:

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, haga clic en **App Services** > **&lt;any_app>**  > **Centro de implementaciones** > **Credenciales de implementación**.

    En el portal, debe tener al menos una aplicación para acceder a la página de credenciales de implementación. Sin embargo, con la [CLI de Azure](/cli/azure/webapp/deployment/user?view=azure-cli-latest#az-webapp-deployment-user-set), puede configurar las credenciales de nivel de usuario sin tener ninguna aplicación.

2. Haga clic en **Credenciales de usuario**, configure el nombre de usuario y la contraseña, y luego haga clic en **Guardar credenciales**.

    ![](./media/app-service-deployment-credentials/deployment_credentials_configure.png)

Una vez que haya configurado las credenciales de implementación, puede encontrar el nombre de usuario de implementación de *Git* en la **Información general** de la aplicación

![](./media/app-service-deployment-credentials/deployment_credentials_overview.png)

y el nombre de usuario de implementación de *FTP* en las **propiedades** de la aplicación.

![](./media/app-service-deployment-credentials/deployment_credentials_properties.png)

> [!NOTE]
> Azure no muestra la contraseña de la implementación de nivel de usuario. Si se le olvida la contraseña, podrá restablecer las credenciales siguiendo los pasos descritos en esta sección.
>
>  

## <a name="use-user-level-credentials-with-ftpftps"></a>Utilizar las credenciales de nivel de usuario con FTP o FTPS

Autenticación en un punto de conexión FTP o FTPS con un nombre de usuario de requirers las credenciales de nivel de usuario en el formato siguiente: `<app-name>\<user-name>`

Puesto que las credenciales de nivel de usuario están vinculadas al usuario y no un recurso específico, el nombre de usuario debe tener este formato para dirigir la acción de inicio de sesión para el punto de conexión de la aplicación adecuada.

## <a name="appscope"></a>Obtención y restablecimiento de las credenciales de nivel de aplicación
Para obtener las credenciales de nivel de aplicación:

1. En [Azure Portal](https://portal.azure.com), en el menú de la izquierda, haga clic en **App Services** > **&lt;any_app>**  > **Centro de implementaciones** > **Credenciales de implementación**.

2. Haga clic en **Credenciales de la aplicación** y en el vínculo **Copiar** para copiar el nombre de usuario o la contraseña.

    ![](./media/app-service-deployment-credentials/deployment_credentials_app_level.png)

Para restablecer las credenciales a nivel de aplicación, haga clic en **Restablecer credenciales** en el mismo cuadro de diálogo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar estas credenciales para implementar la aplicación desde [GIT local](deploy-local-git.md) o con [FTP/S](deploy-ftp.md).
