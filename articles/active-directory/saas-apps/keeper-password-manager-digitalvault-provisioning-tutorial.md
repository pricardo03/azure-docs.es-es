---
title: 'Tutorial: Configurar Keeper Password Manager & Digital Vault para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario a Keeper Password Manager & Digital Vault.
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
ms.date: 05/07/2019
ms.author: zchia
ms.openlocfilehash: 445579eb780b49f536ef1a9e13e5ca43db6f98f6
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65508540"
---
# <a name="tutorial-configure-keeper-password-manager--digital-vault-for-automatic-user-provisioning"></a>Tutorial: Configurar Keeper Password Manager & Digital Vault para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Keeper Password Manager & Digital Vault y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y desaprovisionar automáticamente usuarios o grupos en Keeper Password Manager & Digital Vault.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para obtener más información sobre los términos de Microsoft Azure generales de uso para características de vista previa, consulte [condiciones de uso complementarias para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD
* [Un inquilino de Keeper Password Manager & Digital Vault](https://keepersecurity.com/pricing.html?t=e)
* Una cuenta de usuario en Keeper Password Manager & Digital Vault con permisos de administrador.

## <a name="add-keeper-password-manager--digital-vault-from-the-gallery"></a>Agregar Keeper Password Manager & Digital Vault desde la Galería

Antes de configurar Keeper Password Manager & Digital Vault para el aprovisionamiento automático de usuarios con Azure AD, deberá agregar Keeper Password Manager & Digital Vault desde la Galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Keeper Password Manager & Digital Vault desde la Galería de aplicaciones de Azure AD, realice los pasos siguientes:**

1. En el  **[portal Azure](https://portal.azure.com)**, en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione la **nueva aplicación** situado en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Keeper Password Manager & Digital Vault**, seleccione **Keeper Password Manager & Digital Vault** en el panel de resultados y, a continuación, haga clic en el **agregar**para agregar la aplicación.

    ![Keeper Password Manager & Digital Vault en la lista de resultados](common/search-new-app.png)

## <a name="assigning-users-to-keeper-password-manager--digital-vault"></a>Asignación de usuarios a Keeper Password Manager & Digital Vault

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceso a Keeper Password Manager & Digital Vault. Una vez decidido, puede asignar dichos usuarios o grupos a Keeper Password Manager & Digital Vault siguiendo estas instrucciones:

* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-keeper-password-manager--digital-vault"></a>Sugerencias importantes para asignar usuarios a Keeper Password Manager & Digital Vault

* Se recomienda que un único usuario de Azure AD se asigna a Keeper Password Manager & Digital Vault para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar a un usuario a Keeper Password Manager & Digital Vault, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-keeper-password-manager--digital-vault"></a>Configuración del aprovisionamiento automático de usuarios a Keeper Password Manager & Digital Vault 

Esta sección le guiará a través de los pasos para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Keeper Password Manager & Digital Vault basándose en las asignaciones de grupo de Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Keeper Password Manager & Digital Vault, siga las instrucciones proporcionadas en el [Keeper Password Manager & Digital Vault único inicio de sesión tutorial](keeperpasswordmanager-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-keeper-password-manager--digital-vault-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios de Keeper Password Manager & Digital Vault en Azure AD:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales**, a continuación, seleccione **todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Keeper Password Manager & Digital Vault**.

    ![Vínculo a Keeper Password Manager & Digital Vault en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña de aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña de aprovisionamiento](common/provisioning-automatic.png)

5. En el **las credenciales de administrador** sección, entrada la **dirección URL del inquilino** y **Token secreto** de su Keeper Password Manager & cuenta del almacén Digital tal como se describe en el paso 6.

6. Inicie sesión en su [consola de administración de Keeper](https://keepersecurity.com/console/#login). Haga clic en **Admin** y seleccione un nodo existente o cree uno nuevo. Navegue hasta la **aprovisionamiento** pestaña y seleccione **Agregar método**.

    ![Consola de administración de Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-admin-console.png)

    Seleccione **SCIM (System for Cross-Domain Identity Management**.

    ![Keeper agregar SCIM](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-add-scim.png)

    Haga clic en **crear un Token de aprovisionamiento**.

    ![Keeper crear punto de conexión](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-endpoint.png)

    Copie los valores de **URL** y **Token** y péguelas en **dirección URL del inquilino** y **Token secreto** en Azure AD. Haga clic en **guardar** para completar la configuración de aprovisionamiento en Keeper.

    ![Crear Token Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-create-token.png)

7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Keeper Password Manager & Digital Vault. Si se produce un error en la conexión, asegúrese de que su cuenta de Keeper Password Manager & Digital Vault tiene permisos de administrador y vuelva a intentarlo.

    ![Dirección URL del inquilino + el Token](common/provisioning-testconnection-tenanturltoken.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

9. Haga clic en **Save**(Guardar).

10. En el **asignaciones** sección, seleccione **sincronizar Active Directory usuarios de Azure a Keeper Password Manager & Digital Vault**.

    ![Asignaciones de usuario de Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-mappings.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Keeper Password Manager & Digital Vault en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario en Keeper Password Manager & Digital Vault para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-user-attributes.png)

12. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a Keeper Password Manager & Digital Vault**.

    ![Asignaciones de grupos de Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-mappings.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD con Keeper Password Manager & Digital Vault en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para que coincidan con los grupos en Keeper Password Manager & Digital Vault para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de grupo de Keeper](media/keeper-password-manager-digitalvault-provisioning-tutorial/keeper-group-attributes.png)

14. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar el servicio de Keeper Password Manager & Digital Vault de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección.

    ![Alterne el estado de aprovisionamiento en](common/provisioning-toggle-on.png)

16. Defina los usuarios o grupos que le gustaría que se aprovisionen en Keeper Password Manager & Digital Vault eligiendo los valores deseados en **ámbito** en el **configuración** sección.

    ![Ámbito de aprovisionamiento](common/provisioning-scope.png)

17. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardando la configuración del aprovisionamiento](common/provisioning-configuration-save.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad, que describe todas las acciones realizadas por el servicio Keeper Password Manager de aprovisionamiento de Azure AD aprovisionamiento & Almacén digital.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Keeper Password Manager & Digital Vault requiere **correos electrónicos** y **userName** tener el mismo valor de origen, como las actualizaciones de cualquiera de los atributos modificará el otro valor.
* Keeper Password Manager & Digital Vault no admite eliminaciones de usuario, solo se deshabilita. Usuarios deshabilitados se mostrará como bloqueado en la interfaz de usuario de consola de administrador de Keeper.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

