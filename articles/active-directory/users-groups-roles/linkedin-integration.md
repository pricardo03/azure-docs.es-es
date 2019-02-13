---
title: Habilitación de la integración de aplicaciones Microsoft con LinkedIn en Azure Active Directory | Microsoft Docs
description: Explica cómo habilitar o deshabilitar la integración de LinkedIn para las aplicaciones de Microsoft en Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 6b62fde7f9b01fbeb133c92b1306f70f56d71d7a
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55509633"
---
# <a name="linkedin-integration"></a>Integración con LinkedIn

En este artículo, aprenderá sobre cómo habilitar o deshabilitar la integración con LinkedIn para el inquilino en el centro de administración de Azure Active Directory (Azure AD).

> [!IMPORTANT]
> La configuración de la integración con LinkedIn se encuentra actualmente en proceso de implementación en inquilinos de Azure AD. Cuando se implemente en su inquilino, estará habilitada de forma predeterminada.
> 
> Excepciones:
> * La opción no está disponible para los clientes que usan Microsoft Cloud for US Government, Microsoft Cloud Germany, o bien Azure y Office 365 operado por 21Vianet en China.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Alemania. Tenga en cuenta que la opción no está disponible para los clientes que usan Microsoft Cloud Germany.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Francia.

> La integración solo funciona si la habilita *y* si permite que los usuarios den su consentimiento a aplicaciones que acceden a datos de la empresa en su nombre. Para obtener información sobre la configuración de consentimiento, vea [Cómo quitar el acceso de un usuario a una aplicación](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-the-azure-portal"></a>Habilitación o deshabilitación de la integración con LinkedIn para los usuarios en Azure Portal

Puede habilitar o deshabilitar la integración con LinkedIn en todo el inquilino o solo para usuarios determinados del inquilino.

1. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com/) con una cuenta de administrador global del inquilino de Azure AD.
2. Seleccione **Usuarios**.
3. En la hoja **Usuarios**, seleccione **Configuración de usuario**.
4. En **Integración con LinkedIn**:
  * Seleccione **Sí** para habilitar la integración con LinkedIn en todos los usuarios del inquilino.
  * Elija **Seleccionados** para habilitar la integración con LinkedIn solo en usuarios del inquilino seleccionados.
  * Seleccione **No** para deshabilitar la integración con LinkedIn para todos los usuarios ![Habilitación de la integración con LinkedIn](./media/linkedin-integration/linkedin-integration.png).
5. Guardar la configuración cuando haya terminado seleccionando **Guardar**.

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-group-policy"></a>Habilitación o deshabilitación de la integración con LinkedIn para los usuarios en la directiva de grupo

1. Descargue los [archivos de plantilla de administración de Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extraiga los archivos **ADMX** y cópielos en el almacén central.
3. Abra la Administración de directivas de grupo.
4. Cree un objeto de directiva de grupo con la configuración siguiente: **Configuración de usuario** > **Plantillas administrativas** > **Microsoft Office 2016** > **Varios** > **Mostrar características de LinkedIn en aplicaciones de Office**.
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

Utilice el siguiente vínculo para ver la configuración de integración de LinkedIn actual en Azure Portal:

[Visualización de la configuración de integración con LinkedIn actual en Azure Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
