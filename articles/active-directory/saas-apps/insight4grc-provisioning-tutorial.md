---
title: 'Tutorial: Configuración de Insight4GRC para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario de Insight4GRC.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 34718201-4f0e-4260-9af0-b3b70a1e8265
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 7e8c6c2277f29fc114033aac24844e9e85816b78
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68950761"
---
# <a name="tutorial-configure-insight4grc--for-automatic-user-provisioning"></a>Tutorial: Configuración de Insight4GRC para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se deben realizar en Insight4GRC y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Insight4GRC.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD.
* [Un inquilino de Insight4GRC](https://www.rsmuk.com/).
* Una cuenta de usuario de Insight4GRC con permisos de administrador.

## <a name="assigning-users-to-insight4grc"></a>Asignación de usuarios a Insight4GRC 

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Insight4GRC. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Insight4GRC:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-insight4grc"></a>Sugerencias importantes para asignar usuarios a Insight4GRC 

* Se recomienda asignar un único usuario de Azure AD a Insight4GRC para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Insight4GRC, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="setup-insight4grc-for-provisioning"></a>Configuración de Insight4GRC para el aprovisionamiento

Antes de configurar Insight4GRC para el aprovisionamiento automático de usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en Insight4GRC.

1. Para obtener el token de portador, el cliente final debe ponerse en contacto con el [equipo de soporte técnico](mailto:support.ss@rsmuk.com).

2. Para obtener la dirección URL del punto de conexión de SCIM, debe tener preparado el nombre de dominio de Insight4GRC, ya que se usará para construir la dirección URL del punto de conexión de SCIM. Puede recuperar el nombre de dominio de Insight4GRC como parte de la compra inicial de software con Insight4GRC.


## <a name="add-insight4grc--from-the-gallery"></a>Adición de Insight4GRC desde la galería

Para configurar Insight4GRC para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Insight4GRC desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Insight4GRC desde la galería de aplicaciones de Azure AD, haga lo siguiente:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Insight4GRC**, seleccione **Insight4GRC** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Insight4GRC en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-insight4grc"></a>Configuración del aprovisionamiento automático de usuarios en Insight4GRC  

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Insight4GRC en función de las asignaciones de grupos y usuarios en Azure AD.

> [!TIP]
> También puede optar por seguir las instrucciones del [tutorial de inicio de sesión único de Insight4GRC](insight4grc-tutorial.md) para habilitar en Insight4GRC el inicio de sesión único basado en SAML. El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-insight4grc--in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Insight4GRC en Azure AD, siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Insight4GRC**.

    ![Vínculo a Insight4GRC en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5.  En la sección de credenciales de administrador, escriba `https://{Insight4GRC Domain Name}.insight4grc.com/public/api/scim/v2` en **URL de inquilino** con el valor {Insight4GRC Domain Name} que se recuperó anteriormente. Escriba el **valor de token** recuperado anteriormente en **Token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Insight4GRC. Si la conexión no se establece, asegúrese de que la cuenta de Insight4GRC tenga permisos de administrador e inténtelo de nuevo.

    ![URL de inquilino + Token](common/provisioning-testconnection-tenanturltoken.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Insight4GRC** (Sincronizar usuarios de Azure Active Directory con Insight4GRC).

    ![Insight4GRC: asignaciones de usuario](media/insight4grc-provisioning-tutorial/usermapping.png)

9. Revise los atributos de usuario que se sincronizan entre Azure AD y Insight4GRC en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Insight4GRC con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Insight4GRC: asignaciones de usuario](media/insight4grc-provisioning-tutorial/userattribute.png)

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Group to Insight4GRC** (Sincronizar grupo de Azure Active Directory con Insight4GRC).

    ![Insight4GRC: asignaciones de grupo](media/insight4grc-provisioning-tutorial/groupmapping.png)

11. Revise los atributos de grupo que se sincronizan entre Azure AD y Insight4GRC en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de grupo de Insight4GRC con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Insight4GRC: asignaciones de grupo](media/insight4grc-provisioning-tutorial/groupattribute.png)

10. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Para habilitar el servicio de aprovisionamiento de Azure AD para Insight4GRC, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

12. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Insight4GRC.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Insight4GRC.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).


## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md).
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md).