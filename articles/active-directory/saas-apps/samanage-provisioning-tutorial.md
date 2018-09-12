---
title: 'Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Samanage.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2018
ms.author: v-wingf-msft
ms.openlocfilehash: e5a69fa2ee9a8c4baaeb6586627c7a9a3c9ba4a8
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347960"
---
# <a name="tutorial-configure-samanage-for-automatic-user-provisioning"></a>Tutorial: Configuración de Samanage para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Samanage y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Samanage.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En el escenario que se describe en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure AD
*   Un [inquilino de Samanage](https://www.samanage.com/pricing/) con el paquete Professional
*   Una cuenta de usuario de Samanage con permisos de administrador

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en [Samanage REST API](https://www.samanage.com/api/), que está disponible para los desarrolladores de Samanage para las cuentas con el paquete Professional.

## <a name="adding-samanage-from-the-gallery"></a>Adición de Samanage desde la galería
Antes de configurar Samanage para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Samanage desde la galería de aplicaciones de Azure AD hasta la lista de aplicaciones SaaS administradas.

**Para agregar Samanage desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]

3. Para agregar Samanage, haga clic en el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Samanage**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/AppSearch.png)

5. En el panel de resultados, seleccione **Samanage** y, luego, haga clic en el botón **Agregar** para agregar Samanage a la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-samanage"></a>Asignación de usuarios a Samanage

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Samanage. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Samanage:

*   [Asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-samanage"></a>Sugerencias importantes para asignar usuarios a Samanage

*   Se recomienda asignar un único usuario de Azure AD a Samanage para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar un usuario a Samanage, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-samanage"></a>Configuración del aprovisionamiento automático de usuarios en Samanage

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Samanage en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Samanage siguiendo las instrucciones del [tutorial de inicio de sesión único de Samanage](samanage-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-samanage-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Samanage en Azure AD, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione Samanage en la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/AppInstanceSearch.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningCredentials.png)

5. En la sección **Credenciales de administrador**, escriba el **nombre de usuario de administrador** y la **contraseña de administrador** de su cuenta de Samanage. Algunos ejemplos de estos valores son:

    *   En el campo **Nombre de usuario de administrador**, rellene el nombre de usuario de la cuenta de administrador de su inquilino de Samanage. Ejemplo: admin@contoso.com.

    *   En el campo **Contraseña de administrador**, rellene la contraseña de la cuenta correspondiente al nombre de usuario de administrador.

6. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Samanage. Si la conexión no se establece, asegúrese de que la cuenta de Samanage tiene permisos de administrador e inténtelo de nuevo.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/TestConnection.png)

7. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/EmailNotification.png)

8. Haga clic en **Save**(Guardar).

9. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Samanage** (Sincronizar usuarios de Azure Active Directory con Samanage).

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/UserMappings.png)

10. Revise los atributos de usuario que se sincronizan entre Azure AD y Samanage, en la sección **Attribute Mappings** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/UserAttributeMapping.png)

11. Para habilitar las asignaciones de grupos, en la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Samanage** (Sincronizar grupos de Azure Active Directory con Samanage).

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/GroupMappings.png)

12. Establezca **Habilitado** a **Sí** para sincronizar los grupos. Revise los atributos de grupo que se sincronizan entre Azure AD y Samanage en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Samanage con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/GroupAttributeMapping.png)

13. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

14. Para habilitar el servicio de aprovisionamiento de Azure AD para Samanage, cambie el **estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ProvisioningStatus.png)

15. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Samanage.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/ScopeSync.png)

16. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Samanage](./media/samanage-provisioning-tutorial/SaveProvisioning.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Samanage.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/samanage-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/samanage-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/samanage-provisioning-tutorial/tutorial_general_03.png
