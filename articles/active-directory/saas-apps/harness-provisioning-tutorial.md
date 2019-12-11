---
title: 'Tutorial: Configuración de Harness para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Harness.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 0cdb970b-440b-4e7c-9118-2f03baab6a20
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2019
ms.author: Zhchia
ms.openlocfilehash: 34d05d6392e00757bf1e5562ffd8341ad04cc9dc
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74807666"
---
# <a name="tutorial-configure-harness-for-automatic-user-provisioning"></a>Tutorial: Configuración de Harness para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Harness y Azure Active Directory (Azure AD) para configurar Azure AD a fin de aprovisionar y cancelar automáticamente el aprovisionamiento de usuarios o grupos en Harness.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de Harness](https://harness.io/pricing/)
* Una cuenta de usuario de Harness con permisos de administrador.

## <a name="assigning-users-to-harness"></a>Asignación de usuarios a Harness

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Harness. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Harness:
* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-harness"></a>Sugerencias importantes para asignar usuarios a Harness

* Se recomienda asignar un único usuario de Azure AD a Harness para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Harness, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="set-up-harness-for-provisioning"></a>Configuración de Harness para el aprovisionamiento

1. Inicie sesión en la [consola de administración de Harness](https://app.harness.io/#/login). Vaya a **Continuous Security > Access Management** (Seguridad continua > Administración de acceso).

    ![Consola de administración de Harness](media/harness-provisioning-tutorial/admin.png)

2.  Haga clic en **API Keys** (Claves de API).

    ![Agregar SCIM en Harness](media/harness-provisioning-tutorial/apikeys.png)

3. Haga clic en **Add New Key** (Agregar nueva clave). En el cuadro de diálogo **Add Api Key** (Agregar clave de API), proporcione un **Name** (Nombre) y seleccione una opción del menú desplegable **Permissions Inherited from** (Permisos heredados de). Haga clic en el botón **Submit** (Enviar).

    ![Agregar nueva clave en Harness](media/harness-provisioning-tutorial/addkey.png)

    ![Cuadro de diálogo Agregar nueva clave en Harness](media/harness-provisioning-tutorial/title.png)

3.  Copie el campo **Key** (Clave). Este valor se escribirá en el campo Token secreto de la pestaña Aprovisionamiento en la aplicación Harness en Azure Portal.

    ![Crear token en Harness](media/harness-provisioning-tutorial/token.png)

## <a name="add-harness-from-the-gallery"></a>Adición de Harness desde la galería

Antes de configurar Harness para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Harness desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Harness desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Harness**, seleccione **Harness** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Harness en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-harness"></a>Configuración del aprovisionamiento automático de usuarios en Harness 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios o grupos en Harness en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Harness siguiendo las instrucciones del [Tutorial de inicio de sesión único de Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial). El inicio de sesión único se puede configurar independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

> [!NOTE]
> Para más información sobre el punto de conexión de SCIM de Harness, consulte [este documento](https://docs.harness.io/article/smloyragsm-api-keys).

### <a name="to-configure-automatic-user-provisioning-for-harness-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Harness en Azure AD:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Harness**.

    ![Vínculo a Harness en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba `https://app.harness.io/gateway/api/scim/account/XCPzWkCIQ46ypIu2DeT7yw` en la **URL de inquilino**. Escriba el valor **SCIM Authentication Token** (Token de autenticación de SCIM) recuperado anteriormente en **Token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Harness. Si la conexión no se establece, asegúrese de que la cuenta de Harness tiene permisos de administrador y pruebe otra vez.

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Sincronizar usuarios de Azure Active Directory con Harness**.

    ![Asignaciones de usuario de Harness](media/harness-provisioning-tutorial/usermappings.png)

9. Examine los atributos de grupo que se sincronizan entre Azure AD y Harness en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Harness con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Harness](media/harness-provisioning-tutorial/userattributes.png)

10. En la sección **Asignaciones**, seleccione **Sincronizar grupos de Azure Active Directory con Harness**.

    ![Asignaciones de grupo de Harness](media/harness-provisioning-tutorial/groupmappings.png)

11. Examine los atributos de grupo que se sincronizan entre Azure AD y Harness en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para establecer coincidencias con los grupos de Harness con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de grupo de Harness](media/harness-provisioning-tutorial/groupattributes.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para Harness, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Harness.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Harness.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)
