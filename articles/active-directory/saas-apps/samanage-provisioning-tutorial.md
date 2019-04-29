---
title: 'Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar las cuentas de usuario a Samanage automáticamente.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 62d0392f-37d4-436e-9aff-22f4e5b83623
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: v-wingf-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: d474d9bfd6016885eaa21afcea5d44d39c624084
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104636"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios

Este tutorial muestra los pasos necesarios para realizar en Samanage y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y cancelar el aprovisionamiento de usuarios y grupos a Samanage automáticamente.

> [!NOTE]
> Este tutorial describe un conector que se basa en el servicio de aprovisionamiento de usuario de Azure AD. Para obtener información sobre lo que hace este servicio, cómo funciona y las preguntas más frecuentes, consulte [automatización del aprovisionamiento y Desaprovisionamiento de usuarios para aplicaciones de software-como-servicio (SaaS) con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

El escenario descrito en este tutorial se supone que tiene:

* Un inquilino de Azure AD.
* Un [inquilino de Samanage](https://www.samanage.com/pricing/) con el paquete profesional.
* Una cuenta de usuario de Samanage con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en el [API de Rest de Samanage](https://www.samanage.com/api/). Esta API está disponible para los desarrolladores de Samanage para las cuentas con el paquete profesional.

## <a name="add-samanage-from-the-azure-marketplace"></a>Agregar Samanage desde Azure Marketplace

Antes de configurar Samanage para aprovisionar automáticamente usuarios con Azure AD, debe agregar Samanage desde Azure Marketplace a la lista de aplicaciones SaaS administradas.

Para agregar Samanage desde Marketplace, siga estos pasos.

1. En el [portal Azure](https://portal.azure.com), en el panel de navegación de la izquierda, seleccione **Azure Active Directory**.

    ![El icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Samanage** y seleccione **Samanage** desde el panel de resultados. Para agregar la aplicación, seleccione **agregar**.

    ![Samanage en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-samanage"></a>Asignar a usuarios a Samanage

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se asignaron a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decidir qué usuarios o grupos en Azure AD necesitan acceder a Samanage. Para asignar estos usuarios o grupos a Samanage, siga las instrucciones de [asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-samanage"></a>Sugerencias importantes para asignar usuarios a Samanage

*    Hoy en día, los roles de Samanage automática y dinámicamente rellenan en la interfaz de usuario del portal de Azure. Antes de asignar roles de Samanage a los usuarios, asegúrese de que se ha completado una sincronización inicial con Samanage para recuperar las funciones más recientes en el inquilino de Samanage.

*    Se recomienda asignar un único usuario de Azure AD a Samanage para probar el configuración del aprovisionamiento de usuarios automático inicial. Puede asignar usuarios y grupos adicionales más adelante después de las pruebas son correctas.

*    Al asignar un usuario a Samanage, seleccione un rol válido específico de la aplicación, si está disponible, en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-samanage"></a>Configurar el aprovisionamiento automático de usuarios a Samanage

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Usar para crear, actualizar y deshabilitar usuarios o grupos en Samanage en función de las asignaciones de usuario o grupo en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Samanage. Siga las instrucciones de la [tutorial de inicio de sesión único de Samanage](samanage-tutorial.md). Inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características complementan entre sí.

### <a name="configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Configurar el aprovisionamiento automático de usuarios para Samanage en Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales** > **todas las aplicaciones** > **Samanage**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Samanage**.

    ![El vínculo de Samanage en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Modo de aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. En el **las credenciales de administrador** sección, escriba el nombre de usuario administrador y la contraseña de administrador de la cuenta de Samanage. Algunos ejemplos de estos valores son:

   * En el **Admin Username** cuadro, rellene el nombre de usuario de la cuenta de administrador en el inquilino de Samanage. Un ejemplo es admin@contoso.com.

   * En el **contraseña de administrador** cuadro, rellene la contraseña de la cuenta de administrador que se corresponde con el nombre de usuario de administrador.

6. Una vez que rellene los cuadros que se muestra en el paso 5, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a Samanage. Si se produce un error en la conexión, asegúrese de que la cuenta de Samanage tiene permisos de administrador y vuelva a intentarlo.

    ![Conexión de prueba de Samanage](./media/samanage-provisioning-tutorial/TestConnection.png)

7. En el **correo electrónico de notificación** cuadro, escriba la dirección de correo electrónico de la persona o grupo para recibir las notificaciones de error de aprovisionamiento. Seleccione el **enviar una notificación por correo electrónico cuando se produce un error** casilla de verificación.

    ![Correo electrónico de notificación de Samanage](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Seleccione **Guardar**.

9. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Samanage** (Sincronizar usuarios de Azure Active Directory con Samanage).

    ![Sincronización de usuario de Samanage](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Revise los atributos de usuario que se sincronizan entre Azure AD y Samanage, en la sección **Attribute Mappings** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de usuario coincidentes de Samanage](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Para habilitar las asignaciones de grupos, en la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Samanage** (Sincronizar grupos de Azure Active Directory con Samanage).

    ![Sincronización de grupos de Samanage](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Establezca **Habilitado** a **Sí** para sincronizar los grupos. Revise los atributos de grupo que se sincronizan entre Azure AD y Samanage en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de grupo coincidentes de Samanage](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Para configurar filtros de ámbito, siga las instrucciones de la [tutorial de filtro de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Para habilitar el servicio para Samanage, el aprovisionamiento en Azure AD la **configuración** sección, cambie **estado de aprovisionamiento** a **en**.

    ![Estado de aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. Definir los usuarios o grupos que desea que se aprovisionen en Samanage. En el **configuración** , seleccione los valores que desee en **ámbito**. Cuando se selecciona el **sincronizar todos los usuarios y grupos** opción, tenga en cuenta las limitaciones, como se describe en la siguiente sección "Limitaciones de conector".

    ![Ámbito de Samanage](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Cuando esté listo para aprovisionar, seleccione **guardar**.

    ![Guardar Samanage](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **ámbito** en el **configuración** sección. La sincronización inicial tardará más tiempo en realizarse que las sincronizaciones posteriores. Se producen aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento. El informe describe todas las acciones realizadas por el servicio en Samanage de aprovisionamiento de Azure AD.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

Si selecciona el **sincronizar todos los usuarios y grupos** opción y configura un valor para el Samanage **roles** atributo, el valor en el **valor predeterminado si es null (es opcional)** cuadro se debe expresar en el formato siguiente:

- {"displayName": "rol"}, donde el rol es el valor predeterminado que desee.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
