---
title: 'Acceso condicional: Exigir autenticación multifactor para los administradores (Azure Active Directory)'
description: Cree una directiva de acceso condicional personalizada para exigir que los administradores realicen la autenticación multifactor.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 35a6ac955ade3eab77f01c4e234b35744f0a7be3
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170089"
---
# <a name="conditional-access-require-mfa-for-administrators"></a>Acceso condicional: Exigir autenticación multifactor para administradores

Las cuentas a las que se asignan derechos administrativos son el objetivo de los atacantes. Exigir la autenticación multifactor (MFA) en esas cuentas es una forma sencilla de reducir el riesgo de que las cuentas se vean comprometidas.

Microsoft recomienda exigir MFA en los roles siguientes como mínimo:

* Administrador global
* Administrador de SharePoint
* Administrador de Exchange
* Administrador de acceso condicional
* Administrador de seguridad
* Administrador del departamento de soporte técnico (contraseñas)
* Administrador de contraseñas
* Administrador de facturación
* Administrador de usuarios

Las organizaciones pueden optar por incluir o excluir roles según sea adecuado.

## <a name="user-exclusions"></a>Exclusiones de usuarios

Las directivas de acceso condicional son herramientas eficaces, por lo que se recomienda excluir las siguientes cuentas de la directiva:

* Cuentas de **acceso de emergencia** **** para evitar el bloqueo de cuentas en todo el inquilino. En el caso improbable de que todos los administradores estén bloqueados en el inquilino, la cuenta administrativa de acceso de emergencia se puede usar para iniciar sesión en el inquilino y realizar los pasos para recuperar el acceso.
   * Se puede encontrar más información en el artículo [Administración de cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Cuentas de servicio** y **entidades de servicio**, como la cuenta de sincronización de Azure AD Connect. Las cuentas de servicio son cuentas no interactivas que no están asociadas a ningún usuario en particular. Las usan normalmente los servicios back-end y permiten el acceso a las aplicaciones mediante programación. Las cuentas de servicio deben excluirse porque MFA no se puede completar mediante programación.
   * Si su organización usa estas cuentas en scripts o código, piense en la posibilidad de reemplazarlas por [identidades administradas](../managed-identities-azure-resources/overview.md). Como solución temporal, puede excluir estas cuentas específicas de la directiva de línea de base.

## <a name="create-a-conditional-access-policy"></a>Creación de una directiva de acceso condicional

Los pasos siguientes le ayudarán a crear una directiva de acceso condicional que exija que esos roles administrativos asignados realicen la autenticación multifactor.

1. Inicie sesión en **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. Seleccione **Nueva directiva**.
1. Asigne un nombre a la directiva. Se recomienda que las organizaciones creen un estándar significativo para los nombres de sus directivas.
1. En **Asignaciones**, seleccione **Usuarios y grupos**.
   1. En **Incluir**, seleccione **Roles de directorio (versión preliminar)** y elija los siguientes roles como mínimo:
      * Administrador global
      * Administrador de SharePoint
      * Administrador de Exchange
      * Administrador de acceso condicional
      * Administrador de seguridad
      * Administrador del departamento de soporte técnico
      * Administrador de contraseñas
      * Administrador de facturación
      * Administrador de usuarios
   1. En **Excluir**, seleccione **Usuarios y grupos** y, luego, elija las cuentas de acceso de emergencia de la organización. 
   1. Seleccione **Listo**.
1. En **Aplicaciones en la nube o acciones** > **Incluir**, seleccione **Todas las aplicaciones en la nube** y, luego, **Listo**.
1. En **Controles de acceso** > **Conceder**, seleccione **Conceder acceso**, **Requerir autenticación multifactor** y **Seleccionar**.
1. Confirme la configuración y establezca **Habilitar directiva** en **Activado**.
1. Seleccione **Crear** para crear la directiva.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[Simulación del comportamiento de inicio de sesión mediante la herramienta What If de acceso condicional](troubleshoot-conditional-access-what-if.md)
