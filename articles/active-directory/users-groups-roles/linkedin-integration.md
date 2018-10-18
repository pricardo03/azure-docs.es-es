---
title: Habilitación de la integración de conexiones de LinkedIn en Azure Active Directory | Microsoft Docs
description: Se explica cómo habilitar o deshabilitar conexiones de cuentas de LinkedIn para aplicaciones de Microsoft en Azure Active Directory.
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 0eaa2656313ecd9b64503051265dc16285f0a1f3
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44492877"
---
# <a name="linkedin-account-connections"></a>Conexiones de cuentas de LinkedIn

En este artículo, obtendrá información sobre cómo habilitar o deshabilitar conexiones de cuentas de LinkedIn para el inquilino en el centro de administración de Azure Active Directory (Azure AD).

> [!IMPORTANT]
> La configuración de conexiones de cuentas de LinkedIn se encuentra actualmente en proceso de implementación en inquilinos de Azure AD. Cuando se implemente en su inquilino, estará habilitada de forma predeterminada. 
> 
> Excepciones:
> * La opción no está disponible para los clientes que usan Microsoft Cloud for US Government, Microsoft Cloud Germany, o bien Azure y Office 365 operado por 21Vianet en China.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Alemania. Tenga en cuenta que la opción no está disponible para los clientes que usan Microsoft Cloud Germany.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Francia.

> La integración solo funciona si la habilita *y* si permite que los usuarios den su consentimiento a aplicaciones que acceden a datos de la empresa en su nombre. Para obtener información sobre la configuración de consentimiento, vea [Cómo quitar el acceso de un usuario a una aplicación](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-in-the-azure-portal"></a>Habilitación o deshabilitación de conexiones de cuentas de LinkedIn para el inquilino en Azure Portal

Puede habilitar o deshabilitar las conexiones de cuentas de LinkedIn en todo el inquilino o solo para usuarios determinados del inquilino.

1. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com/) con una cuenta de administrador global del inquilino de Azure AD.
2. Seleccione **Usuarios**.
3. En la hoja **Usuarios**, seleccione **Configuración de usuario**.
4. En **LinkedIn account connections** (Conexiones de cuentas de LinkedIn), siga estos pasos:
  * Seleccione **Sí** para habilitar las conexiones de cuentas de LinkedIn en todos los usuarios de su inquilino.
  * Elija **Selected** (Seleccionados) para habilitar las conexiones de cuentas de LinkedIn solo en determinados usuarios del inquilino.
  * Seleccione **No** para deshabilitar las conexiones de cuentas de LinkedIn en todos los usuarios. ![Habilitación de conexiones de cuentas de LinkedIn](./media/linkedin-integration/linkedin-integration.png)
5. Guardar la configuración cuando haya terminado seleccionando **Guardar**.

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-using-group-policy"></a>Habilitación o deshabilitación de conexiones de cuentas de LinkedIn para el inquilino con Directiva de grupo

1. Descargue los [archivos de plantilla de administración de Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extraiga los archivos **ADMX** y cópielos en el almacén central.
3. Abra la Administración de directivas de grupo.
4. Cree un objeto de directiva de grupo con la siguiente configuración: **Configuración de usuario** > **Plantillas administrativas** > **Microsoft Office 2016** > **Varios** > **Show LinkedIn features in Office applications**(Mostrar características de LinkedIn en las aplicaciones de Office).
5. Seleccione **Habilitado** o **Deshabilitado**.
  
 Estado | Efecto
------ | ------
**Enabled** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está habilitada. Los usuarios de la organización pueden usar las características de LinkedIn en las aplicaciones de Office.
 **Deshabilitada** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está deshabilitada y los usuarios finales no pueden cambiarla. Los usuarios de la organización no podrán usar las características de LinkedIn en las aplicaciones de Office 2016.

Esta directiva de grupo afecta solo a las aplicaciones de Office 2016 de los equipos locales. Los usuarios pueden ver las características de LinkedIn en las tarjetas de perfil de Office 365, aunque se deshabilite LinkedIn en las aplicaciones de Office 2016.

## <a name="learn-more"></a>Más información

* [Integración de LinkedIn en la organización](linkedin-user-consent.md)

* [Información y características de LinkedIn en las aplicaciones de Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ayuda de LinkedIn](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Pasos siguientes
Use el siguiente vínculo para ver la configuración actual de las conexiones de cuentas de LinkedIn en Azure Portal:

[Configuración de las conexiones de cuentas de LinkedIn](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 