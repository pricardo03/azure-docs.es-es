---
title: 'Tutorial: Configuración de Zendesk para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar las cuentas de usuario a Zendesk automáticamente.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: 01d5e4d5-d856-42c4-a504-96fa554baf66
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant
ms.collection: M365-identity-device-management
ms.openlocfilehash: f559d2c2398998ba590419758de559f21d9b65f5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62114672"
---
# <a name="tutorial-configure-zendesk-for-automatic-user-provisioning"></a>Tutorial: Configuración de Zendesk para el aprovisionamiento automático de usuarios

Este tutorial muestra los pasos necesarios para realizar en Zendesk y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y cancelar el aprovisionamiento de usuarios y grupos a Zendesk automáticamente.

> [!NOTE]
> Este tutorial describe un conector que se basa en el servicio de aprovisionamiento de usuario de Azure AD. Para obtener información sobre lo que hace este servicio, cómo funciona y las preguntas más frecuentes, consulte [automatización del aprovisionamiento y Desaprovisionamiento de usuarios para aplicaciones de software-como-servicio (SaaS) con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

El escenario descrito en este tutorial se supone que tiene:

* Un inquilino de Azure AD.
* Un inquilino de Zendesk con el [Enterprise](https://www.zendesk.com/product/pricing/) planear o uno superior habilitado.
* Una cuenta de usuario de Zendesk con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en el [API de Rest de Zendesk](https://developer.zendesk.com/rest_api/docs/core/introduction). Esta API está disponible para los equipos de Zendesk en el plan Enterprise o superior.

## <a name="add-zendesk-from-the-azure-marketplace"></a>Agregar Zendesk desde Azure Marketplace

Antes de configurar Zendesk para aprovisionar automáticamente usuarios con Azure AD, debe agregar Zendesk desde Azure Marketplace a la lista de aplicaciones SaaS administradas.

Para agregar Zendesk desde Marketplace, siga estos pasos.

1. En el [portal Azure](https://portal.azure.com), en el panel de navegación de la izquierda, seleccione **Azure Active Directory**.

    ![El icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Zendesk** y seleccione **Zendesk** desde el panel de resultados. Para agregar la aplicación, seleccione **agregar**.

    ![Zendesk en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-zendesk"></a>Asignar a usuarios a Zendesk

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se asignaron a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decidir qué usuarios o grupos en Azure AD necesitan acceder a Zendesk. Para asignar estos usuarios o grupos a Zendesk, siga las instrucciones de [asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md).

### <a name="important-tips-for-assigning-users-to-zendesk"></a>Sugerencias importantes para asignar usuarios a Zendesk

* Hoy en día, los roles de Zendesk automática y dinámicamente rellenan en la interfaz de usuario del portal de Azure. Antes de asignar roles de Zendesk para los usuarios, asegúrese de que se ha completado una sincronización inicial con Zendesk para recuperar las funciones más recientes en el inquilino de Zendesk.

* Se recomienda asignar un único usuario de Azure AD a Zendesk para probar el configuración del aprovisionamiento de usuarios automático inicial. Puede asignar usuarios o grupos adicionales más adelante después de las pruebas son correctas.

* Al asignar un usuario a Zendesk, seleccione un rol válido específico de la aplicación, si está disponible, en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-zendesk"></a>Configurar el aprovisionamiento automático de usuarios a Zendesk 

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Usar para crear, actualizar y deshabilitar usuarios o grupos en Zendesk en función de las asignaciones de usuario o grupo en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único para Zendesk. Siga las instrucciones de la [tutorial de inicio de sesión único de Zendesk](zendesk-tutorial.md). Inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características complementan entre sí.

### <a name="configure-automatic-user-provisioning-for-zendesk-in-azure-ad"></a>Configurar el aprovisionamiento automático de usuarios para Zendesk en Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales** > **todas las aplicaciones** > **Zendesk**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Zendesk**.

    ![El vínculo de Zendesk en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk16.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Modo de aprovisionamiento de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk1.png)

5. En el **las credenciales de administrador** sección, escriba el nombre de usuario administrador, el token secreto y el dominio de su cuenta de Zendesk. Algunos ejemplos de estos valores son:

   * En el **Admin Username** cuadro, rellene el nombre de usuario de la cuenta de administrador en el inquilino de Zendesk. Un ejemplo es admin@contoso.com.

   * En el **Token secreto** cuadro, rellene el token secreto tal como se describe en el paso 6.

   * En el **dominio** cuadro, rellene el subdominio del inquilino de Zendesk. Por ejemplo, para una cuenta con una dirección URL del inquilino `https://my-tenant.zendesk.com`, el subdominio es **mi inquilino**.

6. El token secreto para la cuenta de Zendesk está ubicado en **Admin** > **API** > **configuración**. Asegúrese de que **Access Token** está establecido en **habilitado**.

    ![Configuración de administrador de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk4.png)

    ![Token de secreto de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk2.png)

7. Una vez que rellene los cuadros que se muestra en el paso 5, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a Zendesk. Si se produce un error en la conexión, asegúrese de que su cuenta de Zendesk tiene permisos de administrador y vuelva a intentarlo.

    ![Conexión de prueba de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk19.png)

8. En el **correo electrónico de notificación** cuadro, escriba la dirección de correo electrónico de la persona o grupo para recibir las notificaciones de error de aprovisionamiento. Seleccione el **enviar una notificación por correo electrónico cuando se produce un error** casilla de verificación.

    ![Correo electrónico de notificación de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk9.png)

9. Seleccione **Guardar**.

10. En la sección **Mappings** (Asignaciones), seleccione **Synchronize Azure Active Directory Users to Zendesk** (Sincronizar usuarios de Azure Active Directory con Zendesk).

    ![Sincronización de usuario de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk10.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD y Zendesk en la **asignaciones de atributos** sección. Los atributos seleccionados como propiedades de **Matching** (Coincidencia) se usan para buscar coincidencias con las cuentas de usuario de Zendesk para las operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de usuario coincidentes de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk11.png)

12. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a Zendesk**.

    ![Sincronización de grupos de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk12.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD y Zendesk en la **asignaciones de atributos** sección. Los atributos seleccionados como propiedades de **Matching** (Coincidencia) se usan para buscar coincidencias con los grupos de Zendesk para las operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de grupo coincidentes de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk13.png)

14. Para configurar filtros de ámbito, siga las instrucciones de la [tutorial de filtro de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar el servicio para Zendesk, el aprovisionamiento en Azure AD la **configuración** sección, cambie **estado de aprovisionamiento** a **en**.

    ![Estado de aprovisionamiento de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk14.png)

16. Definir los usuarios o grupos que desea que se aprovisionen en Zendesk. En el **configuración** , seleccione los valores que desee en **ámbito**.

    ![Ámbito de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk15.png)

17. Cuando esté listo para aprovisionar, seleccione **guardar**.

    ![Guardar de Zendesk](./media/zendesk-provisioning-tutorial/ZenDesk18.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **ámbito** en el **configuración** sección. La sincronización inicial tardará más tiempo en realizarse que las sincronizaciones posteriores. Se producen aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento. El informe describe todas las acciones realizadas por el servicio de Zendesk de aprovisionamiento de Azure AD.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Zendesk admite el uso de grupos para los usuarios con **agente** solo roles. Para obtener más información, consulte el [Zendesk documentación](https://support.zendesk.com/hc/en-us/articles/203661966-Creating-managing-and-using-groups).

* Cuando un rol personalizado se asigna a un usuario o grupo, el usuario de Azure AD automático el servicio de aprovisionamiento también asigna el rol predeterminado **agente**. Solo los agentes se pueden asignar un rol personalizado. Para obtener más información, consulte el [documentación de la API de Zendesk](https://developer.zendesk.com/rest_api/docs/support/users#json-format-for-agent-or-admin-requests). 

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/zendesk-tutorial/tutorial_general_01.png
[2]: ./media/zendesk-tutorial/tutorial_general_02.png
[3]: ./media/zendesk-tutorial/tutorial_general_03.png
