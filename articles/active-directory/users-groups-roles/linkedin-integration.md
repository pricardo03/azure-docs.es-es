---
title: 'Consentimiento del Administrador de conexiones de cuentas de LinkedIn: Azure Active Directory | Microsoft Docs'
description: Explica cómo habilitar o deshabilitar las conexiones de cuentas de integración de LinkedIn en aplicaciones de Microsoft en Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e07c53192ea2c8b792256af944c81c9c909dc55
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368133"
---
# <a name="consent-to-linkedin-account-connections-for-an-azure-active-directory-organization"></a>Dar su consentimiento a las conexiones de cuentas de LinkedIn para una organización de Azure Active Directory

Puede permitir que a los usuarios de su organización para tener acceso a sus conexiones de LinkedIn dentro de algunas aplicaciones de Microsoft. No se comparte ningún dato hasta que los usuarios consentir para conectar sus cuentas. Puede integrar su organización en Azure Active Directory (Azure AD) [centro de administración](https://aad.portal.azure.com).

> [!IMPORTANT]
> La configuración de conexiones de LinkedIn cuenta actualmente se está implantando a organizaciones de Azure AD. Cuando se implanta a su organización, está habilitado de forma predeterminada.
> 
> Excepciones:
> * La opción no está disponible para los clientes que usan Microsoft Cloud for US Government, Microsoft Cloud Germany, o bien Azure y Office 365 operado por 21Vianet en China.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Alemania. Tenga en cuenta que la opción no está disponible para los clientes que usan Microsoft Cloud Germany.
> * La opción está desactivada de forma predeterminada para los inquilinos que se aprovisionan en Francia.
>
> Una vez que se habilitan las conexiones de cuentas de LinkedIn para su organización, las conexiones de cuenta funcionen después los usuarios den su consentimiento a aplicaciones que acceden a datos de la empresa en su nombre. Para obtener información acerca de la configuración de consentimiento de usuario, consulte [cómo quitar el acceso de un usuario a una aplicación](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="use-the-azure-portal-to-enable-linkedin-account-connections"></a>Usar el portal de Azure para habilitar las conexiones de cuentas de LinkedIn

Puede habilitar las conexiones de cuentas de LinkedIn para solo los usuarios que desea tener acceso a partir de toda la organización a solo los usuarios seleccionados de su organización.

1. Inicie sesión en el [centro de administración de Azure AD](https://aad.portal.azure.com/) con una cuenta que sea un administrador global para la organización de Azure AD.
2. Seleccione **Usuarios**.
3. En la hoja **Usuarios**, seleccione **Configuración de usuario**.
4. En **las conexiones de cuentas de LinkedIn**, permitir que los usuarios se conecten sus cuentas para tener acceso a sus conexiones de LinkedIn dentro de algunas aplicaciones de Microsoft. No se comparte ningún dato hasta que los usuarios consentir para conectar sus cuentas.

  * Seleccione **Sí** dé su consentimiento para el servicio para todos los usuarios de la organización
  * Seleccione **seleccionados** dar su consentimiento para que solo los usuarios seleccionados de la organización
  * Seleccione **No** para retirar el consentimiento para que los usuarios de su organización

    ![Integrar las conexiones de cuentas de LinkedIn en la organización](./media/linkedin-integration/linkedin-integration.png)

5. Cuando haya terminado, seleccione **guardar** para guardar la configuración.
     
## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>Usar Directiva de grupo para habilitar las conexiones de cuentas de LinkedIn

1. Descargue los [archivos de plantilla de administración de Office 2016 (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Extraiga los archivos **ADMX** y cópielos en el almacén central.
3. Abra la Administración de directivas de grupo.
4. Cree un objeto de directiva de grupo con la configuración siguiente: **Configuración de usuario** > **Plantillas administrativas** > **Microsoft Office 2016** > **Varios** > **Mostrar características de LinkedIn en aplicaciones de Office**.
5. Seleccione **Habilitado** o **Deshabilitado**.
  
   Estado | Efecto
   ------ | ------
   **Enabled** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está habilitada. Los usuarios de su organización pueden usar las características de LinkedIn en sus aplicaciones de Office 2016.
   **Deshabilitada** | La opción **Mostrar características de LinkedIn en aplicaciones de Office** de Opciones de Office 2016 está deshabilitada y los usuarios finales no pueden cambiarla. Los usuarios de la organización no podrán usar las características de LinkedIn en las aplicaciones de Office 2016.

Esta directiva de grupo afecta solo a las aplicaciones de Office 2016 de los equipos locales. Si los usuarios deshabilitan LinkedIn en sus aplicaciones de Office 2016, pueden ver las características de LinkedIn en Office 365.

## <a name="next-steps"></a>Pasos siguientes

* [Consentimiento del usuario y los datos de uso compartido de LinkedIn](linkedin-user-consent.md)

* [Información y características de LinkedIn en las aplicaciones de Microsoft](https://go.microsoft.com/fwlink/?linkid=850740)

* [Centro de ayuda de LinkedIn](https://www.linkedin.com/help/linkedin)

* [Visualización de la configuración de integración con LinkedIn actual en Azure Portal](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)
