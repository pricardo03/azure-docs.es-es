---
title: Eliminación de una aplicación registrada con la plataforma de identidad de Microsoft | Azure
description: Aprenda a quitar una aplicación registrada con la plataforma de identidad de Microsoft.
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
ms.date: 10/25/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: c10aa28dc83d96a99da40b7d95d8e7051108d4c0
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988863"
---
# <a name="quickstart-remove-an-application-registered-with-the-microsoft-identity-platform-preview"></a>Guía de inicio rápido: Eliminación de una aplicación registrada con la plataforma de identidad de Microsoft (versión preliminar)

Es posible que los desarrolladores empresariales y proveedores de software como servicio (SaaS) que han registrado aplicaciones con la plataforma de identidad de Microsoft deban quitar el registro de una aplicación.

En esta guía de inicio rápido, aprenderá a hacer lo siguiente:

* [Quitar una aplicación creada por el usuario o por la organización](#remove-an-application-authored-by-your-organization)
* [Quitar una aplicación creada por otra organización](#remove-an-application-authoried-by-another-organization)

## <a name="prerequisites"></a>Requisitos previos

Para comenzar, asegúrese de que completa estos requisitos previos:

* Tiene un inquilino que tenga las aplicaciones registradas en él. Para saber cómo agregar y registrar aplicaciones, consulte [Registro de una aplicación con la plataforma de identidad de Microsoft](quickstart-register-app.md).
* Participe en la experiencia de versión preliminar para los registros de aplicaciones en Azure Portal. Los pasos descritos en esta guía de inicio rápido corresponden a la nueva interfaz de usuario y solo funcionan si participa en la experiencia de versión preliminar.

## <a name="remove-an-application-authored-by-you-or-your-organization"></a>Eliminación de una aplicación creada por el usuario o por la organización

Las aplicaciones que el usuario o la empresa han registrado están representadas tanto por un objeto de aplicación como por un objeto de entidad de servicio en el inquilino. Para obtener más información, vea [Objetos de aplicación y objetos de entidad de servicio](active-directory-application-objects.md).

### <a name="to-remove-an-application"></a>Para quitar una aplicación

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
2. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
3. En el panel de navegación izquierdo, seleccione el servicio **Azure Active Directory** y, después, seleccione **Registros de aplicaciones (versión preliminar)**. Busque y seleccione la aplicación que desea configurar. Cuando haya seleccionado la aplicación, verá la página **Introducción** de la aplicación.
4. En la página **Introducción**, seleccione **Eliminar**.
5. Seleccione **Sí** para confirmar que quiere eliminar la aplicación.

  > [!NOTE]
  > Para eliminar una aplicación, es necesario que aparezca como propietario de la aplicación o que tenga privilegios de administrador.

## <a name="remove-an-application-authored-by-another-organization"></a>Eliminación de una aplicación creada por otra organización

Si está viendo **Registros de aplicaciones** en el contexto de un inquilino, un subconjunto de las aplicaciones que aparecen en la pestaña **Todas las aplicaciones** son de otro inquilino y se registraron en su inquilino durante el proceso de consentimiento. Más específicamente, se representan solo mediante un objeto de entidad de servicio en su inquilino, sin un objeto aplicación correspondiente. Para más información sobre las diferencias entre objetos de aplicación y objetos de entidad de servicio, consulte [Objetos de aplicación y de entidad de servicio en Azure AD](active-directory-application-objects.md).

Para quitar el acceso de una aplicación a su directorio (después de concederle consentimiento), el administrador de la compañía debe eliminar su entidad de servicio. El administrador debe tener acceso de administrador global y puede quitar la aplicación mediante Azure Portal o usar los [cmdlets de PowerShell de Azure AD](http://go.microsoft.com/fwlink/?LinkId=294151) para eliminarlo.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de estas otras guías de inicio rápido de administración de aplicaciones relacionadas:

* [Registro de una aplicación con la plataforma de identidad de Microsoft](quickstart-register-app.md)
* [Configuración de una aplicación cliente para tener acceso a las API web](quickstart-configure-app-access-web-apis.md)
* [Configuración de una aplicación para exponer las API web](quickstart-configure-app-expose-web-apis.md)
* [Modificación de las cuentas compatibles con una aplicación](quickstart-modify-supported-accounts.md)
