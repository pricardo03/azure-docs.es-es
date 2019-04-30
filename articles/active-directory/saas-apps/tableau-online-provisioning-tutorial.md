---
title: 'Tutorial: configuración de Tableau Online para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar las cuentas de usuario a Tableau Online automáticamente.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 0be9c435-f9a1-484d-8059-e578d5797d8e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2dbebfa5fa7d9b255cc685696bfe8b3f61d5cf6b
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62123761"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Tutorial: configuración de Tableau Online para el aprovisionamiento automático de usuarios

Este tutorial muestra los pasos necesarios para realizar en Tableau Online y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y cancelar el aprovisionamiento de usuarios y grupos a Tableau Online automáticamente.

> [!NOTE]
> Este tutorial describe un conector que se basa en el servicio de aprovisionamiento de usuario de Azure AD. Para obtener información sobre lo que hace este servicio, cómo funciona y las preguntas más frecuentes, consulte [automatización del aprovisionamiento y Desaprovisionamiento de usuarios para aplicaciones de software-como-servicio (SaaS) con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

El escenario descrito en este tutorial se supone que tiene:

*   Un inquilino de Azure AD.
*   Un [Tableau Online inquilino](https://www.tableau.com/).
*   Una cuenta de usuario en Tableau Online con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en el [API de Rest de Tableau Online](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm). Esta API está disponible para los desarrolladores de Tableau Online.

## <a name="add-tableau-online-from-the-azure-marketplace"></a>Agregar Tableau Online desde Azure Marketplace
Antes de configurar Tableau Online para aprovisionar automáticamente usuarios con Azure AD, agregue Tableau Online desde Azure Marketplace a la lista de aplicaciones SaaS administradas.

Para agregar Tableau Online desde Marketplace, siga estos pasos.

1. En el [portal Azure](https://portal.azure.com), en el panel de navegación de la izquierda, seleccione **Azure Active Directory**.

    ![El icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Tableau Online** y seleccione **Tableau Online** desde el panel de resultados. Para agregar la aplicación, seleccione **agregar**.

    ![Tableau Online en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-tableau-online"></a>Asignar a usuarios a Tableau Online

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se asignaron a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decidir qué usuarios o grupos en Azure AD necesitan acceso a Tableau Online. Para asignar estos usuarios o grupos a Tableau Online, siga las instrucciones de [asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Sugerencias importantes para asignar usuarios a Tableau Online

*   Se recomienda asignar un único usuario de Azure AD con Tableau Online para probar la configuración del aprovisionamiento automático de usuarios. Puede asignar usuarios o grupos adicionales más adelante.

*   Al asignar un usuario a Tableau Online, seleccione un rol válido específico de la aplicación, si está disponible, en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-tableau-online"></a>Configurar el aprovisionamiento automático de usuarios a Tableau Online

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Usar para crear, actualizar y deshabilitar usuarios o grupos en Tableau Online basándose en las asignaciones de grupo o usuario de Azure AD.

> [!TIP]
> También puede habilitar basado en SAML único inicio de sesión en Tableau Online. Siga las instrucciones de la [Tableau Online tutorial de inicio de sesión único](tableauonline-tutorial.md). Inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características complementan entre sí.

### <a name="configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Configurar el aprovisionamiento automático de usuarios de Tableau Online en Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales** > **todas las aplicaciones** > **Tableau Online**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Tableau Online**.

    ![El vínculo de Tableau Online en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Modo de aprovisionamiento de tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. En el **las credenciales de administrador** sección, escriba el dominio, nombre de usuario administrador, contraseña de administrador y dirección URL del contenido de la cuenta de Tableau Online:

   * En el **dominio** cuadro, rellene el subdominio según el paso 6.

   * En el **Admin Username** cuadro, rellene el nombre de usuario de la cuenta de administrador en el inquilino de Clarizen. Un ejemplo es admin@contoso.com.

   * En el **contraseña de administrador** cuadro, rellene la contraseña de la cuenta de administrador que se corresponde con el nombre de usuario de administrador.

   * En el **dirección URL del contenido** cuadro, rellene el subdominio según el paso 6.

6. Después de iniciar sesión su cuenta administrativa de Tableau Online, puede obtener los valores de **dominio** y **dirección URL del contenido** desde la dirección URL de la página de administración.

    * El valor de **Dominio** de la cuenta de Tableau Online se puede copiar de esta parte de la dirección URL:

        ![Dominio de tableau Online](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    * El **dirección URL del contenido** de Tableau Online se puede copiar la cuenta de esta sección. Es un valor que se define durante la configuración de la cuenta. En este ejemplo, el valor es "contoso":

        ![Dirección URL del contenido en línea tableau](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > Su **dominio** podría ser diferente del que se muestra aquí.

7. Una vez que rellene los cuadros que se muestra en el paso 5, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a Tableau Online. Si se produce un error en la conexión, asegúrese de que la cuenta de Tableau Online tiene permisos de administrador y vuelva a intentarlo.

    ![Tableau Online Probar conexión](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. En el **correo electrónico de notificación** cuadro, escriba la dirección de correo electrónico de la persona o grupo para recibir las notificaciones de error de aprovisionamiento. Seleccione el **enviar una notificación por correo electrónico cuando se produce un error** casilla de verificación.

    ![Correo electrónico de notificación en pantalla de tableau](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

9. Seleccione **Guardar**.

10. En la sección **Asignaciones**, seleccione **Sincronizar usuarios de Azure Active Directory con Tableau**.

    ![Sincronización de tableau Online de usuario](./media/tableau-online-provisioning-tutorial/UserMappings.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Tableau Online en el **asignaciones de atributos** sección. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Tableau Online con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de usuario coincidentes en línea tableau](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

12. En la sección **Asignaciones**, seleccione **Sincronizar grupos de Azure Active Directory con Tableau**.

    ![Tableau Online del grupo de sincronización](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD con Tableau Online en el **asignaciones de atributos** sección. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Tableau Online con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de tableau Online coincidentes grupo](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

14. Para configurar filtros de ámbito, siga las instrucciones de la [tutorial de filtro de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar el servicio de Tableau Online, el aprovisionamiento en Azure AD la **configuración** sección, cambie **estado de aprovisionamiento** a **en**.

    ![Estado de aprovisionamiento de tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

16. Definir los usuarios o grupos que desea que se aprovisionen en Tableau Online. En el **configuración** , seleccione los valores que desee en **ámbito**.

    ![Ámbito de tableau Online](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

17. Cuando esté listo para aprovisionar, seleccione **guardar**.

    ![Tableau Online Save](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **ámbito** en el **configuración** sección. La sincronización inicial tardará más tiempo en realizarse que las sincronizaciones posteriores. Se producen aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento. El informe describe todas las acciones realizadas por el servicio en Tableau Online de aprovisionamiento de Azure AD.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
