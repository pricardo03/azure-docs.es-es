---
title: 'Tutorial: Configuración de Cornerstone OnDemand para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar las cuentas de usuario a Cornerstone OnDemand automáticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: 85ddcf3aff7d15c946230cedb0da190bca6aeab7
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127504"
---
# <a name="tutorial-configure-cornerstone-ondemand-for-automatic-user-provisioning"></a>Tutorial: Configuración de Cornerstone OnDemand para el aprovisionamiento automático de usuarios

Este tutorial muestra los pasos necesarios para realizar en Cornerstone OnDemand y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y desaprovisionar los usuarios o grupos a Cornerstone OnDemand automáticamente.

> [!NOTE]
> Este tutorial describe un conector que se basa en el servicio de aprovisionamiento de usuario de Azure AD. Para obtener información sobre lo que hace este servicio, cómo funciona y las preguntas más frecuentes, consulte [automatización del aprovisionamiento y Desaprovisionamiento de usuarios para aplicaciones de software-como-servicio (SaaS) con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

El escenario descrito en este tutorial se supone que tiene:

* Un inquilino de Azure AD.
* Un inquilino de Cornerstone OnDemand.
* Una cuenta de usuario de Cornerstone OnDemand con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en el [servicio web de Cornerstone OnDemand](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_-_Summary_of_Web_Services_v20151106.pdf). Este servicio está disponible para los equipos de Cornerstone OnDemand.

## <a name="add-cornerstone-ondemand-from-the-azure-marketplace"></a>Agregar Cornerstone OnDemand desde Azure Marketplace

Antes de configurar Cornerstone OnDemand para aprovisionar automáticamente usuarios con Azure AD, debe agregar Cornerstone OnDemand desde Marketplace a la lista de aplicaciones SaaS administradas.

Para agregar Cornerstone OnDemand desde Marketplace, siga estos pasos.

1. En el [portal Azure](https://portal.azure.com), en el panel de navegación de la izquierda, seleccione **Azure Active Directory**.

    ![El icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Cornerstone OnDemand** y seleccione **Cornerstone OnDemand** desde el panel de resultados. Para agregar la aplicación, seleccione **agregar**.

    ![Cornerstone OnDemand en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-cornerstone-ondemand"></a>Asignar a usuarios a Cornerstone OnDemand

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se asignaron a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decidir qué usuarios o grupos en Azure AD necesitan acceder a Cornerstone OnDemand. Para asignar estos usuarios o grupos a Cornerstone OnDemand, siga las instrucciones de [asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-cornerstone-ondemand"></a>Sugerencias importantes para asignar usuarios a Cornerstone OnDemand

* Se recomienda asignar un único usuario de Azure AD a Cornerstone OnDemand para probar la configuración del aprovisionamiento automático de usuarios. Puede asignar usuarios o grupos adicionales más adelante.

* Al asignar un usuario a Cornerstone OnDemand, seleccione un rol válido específico de la aplicación, si está disponible, en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-cornerstone-ondemand"></a>Configurar el aprovisionamiento automático de usuarios a Cornerstone OnDemand

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Usar para crear, actualizar y deshabilitar usuarios o grupos en Cornerstone OnDemand basándose en las asignaciones de grupo o usuario de Azure AD.

Para configurar el aprovisionamiento automático de usuarios para Cornerstone OnDemand en Azure AD, siga estos pasos.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales** > **todas las aplicaciones** > **Cornerstone OnDemand**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Cornerstone OnDemand**.

    ![En la lista de aplicaciones en el vínculo de Cornerstone OnDemand](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Modo de aprovisionamiento de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningCredentials.png)

5. En el **las credenciales de administrador** sección, escriba el nombre de usuario administrador y contraseña de administrador de dominio de cuenta de su Cornerstone OnDemand:

    * En el **Admin Username** cuadro, rellene el dominio o el nombre de usuario de la cuenta de administrador en el inquilino de Cornerstone OnDemand. Un ejemplo es contoso\admin.

    * En el **contraseña de administrador** cuadro, rellene la contraseña que se corresponde con el nombre de usuario de administrador.

    * En el **dominio** cuadro, rellene la dirección URL del servicio web del inquilino de Cornerstone OnDemand. Por ejemplo, el servicio se encuentra en `https://ws-[corpname].csod.com/feed30/clientdataservice.asmx`, y para Contoso el dominio es `https://ws-contoso.csod.com/feed30/clientdataservice.asmx`. Para obtener más información sobre cómo recuperar la dirección URL del servicio web, consulte [este pdf](https://help.csod.com/help/csod_0/Content/Resources/Documents/WebServices/CSOD_Web_Services_-_User-OU_Technical_Specification_v20160222.pdf).

6. Una vez que rellene los cuadros que se muestra en el paso 5, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a Cornerstone OnDemand. Si se produce un error en la conexión, asegúrese de que la cuenta de Cornerstone OnDemand tiene permisos de administrador y vuelva a intentarlo.

    ![Conexión de Cornerstone OnDemand prueba](./media/cornerstone-ondemand-provisioning-tutorial/TestConnection.png)

7. En el **correo electrónico de notificación** cuadro, escriba la dirección de correo electrónico de la persona o grupo para recibir las notificaciones de error de aprovisionamiento. Seleccione el **enviar una notificación por correo electrónico cuando se produce un error** casilla de verificación.

    ![Correo electrónico de notificación de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/EmailNotification.png)

8. Seleccione **Guardar**.

9. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Cornerstone OnDemand (Sincronizar usuarios de Azure Active Directory con Cornerstone OnDemand)**.

    ![Sincronización de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserMapping.png)

10. Revise los atributos de usuario que se sincronizan entre Azure AD y Cornerstone OnDemand en el **asignaciones de atributos** sección. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Cornerstone OnDemand con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Asignaciones de Cornerstone OnDemand atributo](./media/cornerstone-ondemand-provisioning-tutorial/UserMappingAttributes.png)

11. Para configurar filtros de ámbito, siga las instrucciones de la [tutorial de filtro de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

12. Para habilitar el servicio para Cornerstone OnDemand, el aprovisionamiento en Azure AD la **configuración** sección, cambie **estado de aprovisionamiento** a **en**.

    ![Estado de aprovisionamiento de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/ProvisioningStatus.png)

13. Definir los usuarios o grupos que desea que se aprovisionen en Cornerstone OnDemand. En el **configuración** , seleccione los valores que desee en **ámbito**.

    ![Ámbito de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/SyncScope.png)

14. Cuando esté listo para aprovisionar, seleccione **guardar**.

    ![Guardar de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/Save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **ámbito** en el **configuración** sección. La sincronización inicial tardará más tiempo en realizarse que las sincronizaciones posteriores. Se producen aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento. El informe describe todas las acciones realizadas por el servicio en Cornerstone OnDemand de aprovisionamiento de Azure AD.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

El atributo **Position** de Cornerstone OnDemand espera un valor que corresponde a los roles del portal de Cornerstone OnDemand. Para obtener una lista de válido **posición** valores, vaya a **editar registro de usuario > estructura de organización > posición** en el portal de Cornerstone OnDemand.

![Aprovisionamiento de Cornerstone OnDemand editar registro de usuario](./media/cornerstone-ondemand-provisioning-tutorial/UserEdit.png)

![Posición de aprovisionamiento de Cornerstone OnDemand](./media/cornerstone-ondemand-provisioning-tutorial/UserPosition.png)

![Lista de Cornerstone OnDemand aprovisionamiento posición](./media/cornerstone-ondemand-provisioning-tutorial/PostionId.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cornerstone-ondemand-provisioning-tutorial/tutorial_general_03.png
