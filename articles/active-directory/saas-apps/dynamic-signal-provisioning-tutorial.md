---
title: 'Tutorial: Configurar Dynamic Signal para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario a Dynamic Signal.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: zchia
ms.openlocfilehash: fec6a7e3433eb5d657deac8c1b2ceb327f8d32e4
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65159411"
---
# <a name="tutorial-configure-dynamic-signal-for-automatic-user-provisioning"></a>Tutorial: Configurar Dynamic Signal para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en Dynamic Signal y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y desaprovisionar automáticamente usuarios o grupos a Dynamic Signal.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para obtener más información sobre los términos de Microsoft Azure generales de uso para características de vista previa, consulte [condiciones de uso complementarias para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de Dynamic Signal](https://dynamicsignal.com/)
* Una cuenta de usuario de Dynamic Signal con permisos de administrador.

## <a name="add-dynamic-signal-from-the-gallery"></a>Agregar Dynamic Signal desde la Galería

Antes de configurar Dynamic Signal para aprovisionar automáticamente usuarios con Azure AD, deberá agregar Dynamic Signal desde la Galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Dynamic Signal desde la Galería de aplicaciones de Azure AD, realice los pasos siguientes:**

1. En el  **[portal Azure](https://portal.azure.com)**, en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione la **nueva aplicación** situado en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Dynamic Signal**, seleccione **Dynamic Signal** en el panel de resultados y, a continuación, haga clic en el **agregar** para agregar la aplicación.

    ![Dynamic Signal en la lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-dynamic-signal"></a>Asignación de usuarios a Dynamic Signal

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceso a Dynamic Signal. Una vez decidido, puede asignar dichos usuarios o grupos a Dynamic Signal, siga estas instrucciones:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-dynamic-signal"></a>Sugerencias importantes para asignar usuarios a Dynamic Signal

* Se recomienda que un único usuario de Azure AD se asigna a Dynamic Signal para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar a un usuario a Dynamic Signal, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-dynamic-signal"></a>Configuración del aprovisionamiento automático de usuarios a Dynamic Signal 

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos de Dynamic Signal en función de las asignaciones de grupo en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Dynamic Signal, siga las instrucciones proporcionadas en el [tutorial de inicio de sesión único de Dynamic Signal](dynamicsignal-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-dynamic-signal-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios de Dynamic Signal en Azure AD:

1. Inicie sesión en el [portal Azure](https://portal.azure.com) y seleccione **aplicaciones empresariales**, seleccione **todas las aplicaciones**, a continuación, seleccione **Dynamic Signal**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Dynamic Signal**.

    ![El vínculo de Dynamic Signal en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña de aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento](common/provisioning-automatic.png)

5. En el **las credenciales de administrador** sección, entrada la **dirección URL del inquilino** y **Token secreto** de cuenta de la dinámica de la señal como se describe en el paso 6.

6. En la consola de administración de Dynamic Signal, vaya a **Admin > avanzadas > API**.

    ![Aprovisionamiento de Dynamic Signal](./media/dynamicsignal-provisioning-tutorial/secret-token-1.png)

    Copia el **SCIM API URL** a **dirección URL del inquilino**. Haga clic en **generar nuevo Token** para generar un **el Token de portador** y copie el valor a **Token secreto**.

    ![Aprovisionamiento de Dynamic Signal](./media/dynamicsignal-provisioning-tutorial/secret-token-2.png)

7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Dynamic Signal. Si se produce un error en la conexión, asegúrese de que la cuenta de Dynamic Signal tiene permisos de administrador y vuelva a intentarlo.

    ![Dirección URL del inquilino + el Token](common/provisioning-testconnection-tenanturltoken.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

9. Haga clic en **Save**(Guardar).

10. En el **asignaciones** sección, seleccione **sincronizar Active Directory usuarios de Azure con Dynamic Signal**.

    ![Asignaciones de usuario de Dynamic Signal](media/dynamicsignal-provisioning-tutorial/user-mappings.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Dynamic Signal en la **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario en Dynamic Signal para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Keeper](media/dynamicsignal-provisioning-tutorial/user-mapping-attributes.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de Dynamic Signal de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección.

    ![Alterne el estado de aprovisionamiento en](common/provisioning-toggle-on.png)

14. Defina los usuarios o grupos que le gustaría que se aprovisionen en Dynamic Signal eligiendo los valores deseados en **ámbito** en el **configuración** sección.

    ![Ámbito de aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardando la configuración del aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad, que describe todas las acciones realizadas por el servicio en Dynamic Signal de aprovisionamiento de Azure AD de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Dynamic Signal no admite eliminaciones permanentes de usuario de Azure AD. Para eliminar un usuario permanentemente en Dynamic Signal, la operación debe realizarse a través de la consola de administración de Dynamic Signal la interfaz de usuario. 
* Dynamic Signal no admite actualmente los grupos.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
