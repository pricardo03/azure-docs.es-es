---
title: 'Tutorial: Configurar Zscaler Beta para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario de Zscaler Beta.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 83db6b8d-503b-48f3-b918-f9fba1369d53
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 54cf2924a94dda1d29baf048c866f019b82e1402
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59256845"
---
# <a name="tutorial-configure-zscaler-beta-for-automatic-user-provisioning"></a>Tutorial: Configurar Zscaler Beta para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en Zscaler Beta y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar automáticamente y cancelar el aprovisionamiento de usuarios o grupos en Zscaler Beta.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../active-directory-saas-app-provisioning.md).
>

> Este conector está actualmente en versión preliminar pública. Para obtener más información sobre los términos de Microsoft Azure generales de uso para características de vista previa, consulte [condiciones de uso complementarias para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario que se describe en este tutorial se supone que ya cuenta con los elementos siguientes:

* Un inquilino de Azure AD
* Un inquilino de Zscaler Beta
* Una cuenta de usuario de Zscaler Beta con permisos de administrador

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en la API de SCIM de Zscaler Beta, que está disponible para los desarrolladores de Zscaler Beta para las cuentas con el paquete de la empresa.

## <a name="adding-zscaler-beta-from-the-gallery"></a>Incorporación de Zscaler Beta desde la galería

Antes de configurar Zscaler Beta para aprovisionar automáticamente usuarios con Azure AD, deberá agregar Zscaler Beta desde la Galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Zscaler Beta desde la Galería de aplicaciones de Azure AD, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Zscaler Beta**, seleccione **Zscaler Beta** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Zscaler Beta en la lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-zscaler-beta"></a>Asignación de usuarios a Zscaler Beta

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceso a Zscaler Beta. Una vez decidido, puede asignar dichos usuarios o grupos a Zscaler Beta siguiendo estas instrucciones:

* [Asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-zscaler-beta"></a>Sugerencias importantes para asignar usuarios a Zscaler Beta

* Se recomienda que un único usuario de Azure AD está asignado a Zscaler Beta para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar a un usuario a Zscaler Beta, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-zscaler-beta"></a>Configuración del aprovisionamiento automático de usuarios a Zscaler Beta

Esta sección le guiará a través de los pasos para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar a usuarios o grupos en Zscaler Beta en función de las asignaciones de grupo en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Zscaler Beta, siguiendo las instrucciones en el [tutorial de inicio de sesión único de Zscaler Beta](zscaler-beta-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-zscaler-beta-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios de Zscaler Beta en Azure AD:

1. Inicie sesión en el [portal Azure](https://portal.azure.com) y seleccione **aplicaciones empresariales**, seleccione **todas las aplicaciones**, a continuación, seleccione **Zscaler Beta**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Zscaler Beta**.

    ![El vínculo de Zscaler Beta en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-tab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-credentials.png)

5. En el **las credenciales de administrador** sección, entrada la **dirección URL del inquilino** y **Token secreto** de la cuenta de Zscaler Beta como se describe en el paso 6.

6. Para obtener el **dirección URL del inquilino** y **Token secreto**, vaya a **Administración > configuración de autenticación** en la interfaz de usuario del portal de Zscaler Beta y haga clic en  **SAML** en **tipo de autenticación**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/secret-token-1.png)

    Haga clic en **configurar SAML** para abrir el **configuración SAML** opciones.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/secret-token-2.png)

    Seleccione **Enable SCIM-Based aprovisionamiento** para recuperar **URL Base** y **el Token de portador**, a continuación, guarde la configuración. Copia el **URL Base** a **dirección URL del inquilino**, y **el Token de portador** a **Token secreto** en Azure portal.

7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Zscaler Beta. Si se produce un error en la conexión, asegúrese de que la cuenta de Zscaler Beta tiene permisos de administrador y vuelva a intentarlo.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/test-connection.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/notification.png)

9. Haga clic en **Save**(Guardar).

10. En el **asignaciones** sección, seleccione **sincronizar Azure usuarios Active Directory con Zscaler Beta**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/user-mappings.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Zscaler Beta en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario de Zscaler Beta para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/user-attribute-mappings.png)

12. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a Zscaler Beta**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/group-mappings.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD con Zscaler Beta en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para que coincidan con los grupos en Zscaler Beta para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/group-attribute-mappings.png)

14. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](./../active-directory-saas-scoping-filters.md).

15. Para habilitar el servicio para Zscaler Beta de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/provisioning-status.png)

16. Defina los usuarios o grupos que le gustaría que se aprovisionen en Zscaler Beta eligiendo los valores deseados en **ámbito** en el **configuración** sección.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/scoping.png)

17. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Zscaler Beta](./media/zscaler-beta-provisioning-tutorial/save-provisioning.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad, que describe todas las acciones realizadas por el servicio en Zscaler Beta de aprovisionamiento de Azure AD de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-beta-provisioning-tutorial/tutorial-general-03.png
