---
title: Eliminación de una aplicación de Azure Active Directory
description: Obtenga más información acerca de cómo eliminar una aplicación de Azure Active Directory (Azure AD).
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
ms.openlocfilehash: 2cd77ee6df52940a800733209b7e384d72d98103
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962137"
---
# <a name="quickstart-remove-an-application-from-azure-active-directory"></a>Inicio rápido: Eliminación de una aplicación de Azure Active Directory

Es posible que los desarrolladores empresariales y proveedores de software como servicio (SaaS) que han registrado aplicaciones con Azure Active Directory (Azure AD) deban eliminar el registro de una aplicación de su inquilino de Azure AD.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

* [Eliminar una aplicación creada por su organización](#removing-an-application-authored-by-your-organization)
* [Eliminar una aplicación multiinquilino autorizada por otra organización](#removing-a-multi-tenant-application-authorized-by-another-organization)

## <a name="prerequisites"></a>Requisitos previos

Para comenzar, necesitará a un inquilino de Azure AD que tenga las aplicaciones registradas en él.

* Si aún no tiene un inquilino, [descubra cómo conseguir uno](quickstart-create-new-tenant.md).
* Para obtener información sobre cómo agregar y registrar las aplicaciones en el inquilino, consulte [Agregar una aplicación a Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="removing-an-application-authored-by-your-organization"></a>Eliminación de una aplicación creada por su organización

Las aplicaciones que su organización ha registrado aparecen en el filtro **Mis aplicaciones** en la página principal **Registros de aplicaciones** del inquilino. Se trata de las aplicaciones que ha registrado manualmente mediante Azure Portal o mediante programación a través de PowerShell o Microsoft Graph API. Más específicamente, se representan mediante un objeto de aplicación o de identidad de servicio en el inquilino. Para más información acerca de estos objetos, consulte [Objetos de aplicación y de identidad de servicio](app-objects-and-service-principals.md).

### <a name="to-remove-a-single-tenant-application-from-your-directory"></a>Para eliminar una aplicación de un solo inquilino del directorio

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory**, seleccione **Registros de aplicaciones** y encuentre y seleccione la aplicación que desea configurar.
    Pasará a la página de registro principal de la aplicación, lo que abre la página **Configuración** de la aplicación.
1. En la página de registro principal de la aplicación, seleccione **Eliminar**.
1. Seleccione **Sí** para confirmar que quiere eliminar la aplicación.

### <a name="to-remove-a-multi-tenant-application-from-its-home-directory"></a>Para eliminar una aplicación multiinquilino de su directorio particular

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Si la cuenta proporciona acceso a más de uno, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory**, seleccione **Registros de aplicaciones** y encuentre y seleccione la aplicación que desea configurar.
    Pasará a la página de registro principal de la aplicación, lo que abre la página **Configuración** de la aplicación.
1. Desde la página **Configuración**, elija **Propiedades** y coloque el conmutador **Multiinquilino** en la posición **No** para cambiar primero la aplicación a un solo inquilino y, a continuación, seleccione **Guardar**.
    Los objetos de la entidad de servicio de la aplicación permanecen en los inquilinos de todas las organizaciones que ya han dado su consentimiento.
1. En la página de registro principal de la aplicación, seleccione **Eliminar**.
1. Seleccione **Sí** para confirmar que quiere eliminar la aplicación.

## <a name="removing-a-multi-tenant-application-authorized-by-another-organization"></a>Eliminación de una aplicación multiinquilino autorizada por otra organización

Un subconjunto de las aplicaciones que aparecen en el filtro **Todas las aplicaciones** (sin incluir los registros de **Mis aplicaciones**) en la página principal **Registros de aplicaciones** del inquilino, son aplicaciones multiinquilino.

En términos técnicos, estas aplicaciones multiinquilino proceden de otro inquilino y se registraron en su inquilino durante el proceso de consentimiento. Más específicamente, se representan solo mediante un objeto de entidad de servicio en su inquilino, sin un objeto aplicación correspondiente. Para más información sobre las diferencias entre objetos de aplicación y objetos de entidad de servicio, consulte [Objetos de aplicación y de entidad de servicio en Azure AD](app-objects-and-service-principals.md).

Para eliminar el acceso de una aplicación multiinquilino a su directorio (después de concederle consentimiento), el administrador de la compañía debe eliminar la entidad de servicio de la aplicación. El administrador debe tener acceso de administrador global y puede eliminar el acceso a través de Azure Portal o usar los [Cmdlets de PowerShell de Azure AD](http://go.microsoft.com/fwlink/?LinkId=294151).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre estas otras guías de inicio rápido de administración de aplicaciones relacionadas para aplicaciones con el punto de conexión de Azure AD v1.0:

- [Adición de una aplicación en Azure AD](quickstart-v1-integrate-apps-with-azure-ad.md)
- [Actualización de una aplicación en Azure AD](quickstart-v1-update-azure-ad-app.md)
