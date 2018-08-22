---
title: 'Tutorial: Configuración de Tableau Online para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario de Tableau Online.
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
ms.date: 07/30/2018
ms.author: v-wingf-msft
ms.openlocfilehash: bbee7f0e6022e2945563c102a851819734d52e77
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2018
ms.locfileid: "40107118"
---
# <a name="tutorial-configure-tableau-online-for-automatic-user-provisioning"></a>Tutorial: Configuración de Tableau Online para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Tableau Online y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Tableau Online.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../active-directory-saas-app-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En el escenario que se describe en este tutorial se supone que ya cuenta con los elementos siguientes:

*   Un inquilino de Azure AD
*   Un [inquilino de Tableau Online](https://www.tableau.com/)
*   Una cuenta de usuario de Tableau Online con permisos de administrador

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en la [API REST de Tableau Online](https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm), que está disponible para los desarrolladores de Tableau Online.

## <a name="adding-tableau-online-from-the-gallery"></a>Incorporación de Tableau Online desde la galería
Antes de configurar Tableau Online para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Tableau Online desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Tableau Online desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]

3. Para agregar Tableau Online, haga clic en el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Tableau Online**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/AppSearch.png)

5. En el panel de resultados, seleccione **Tableau Online** y, a continuación, haga clic en el botón **Agregar** para agregar Tableau Online a la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-tableau-online"></a>Asignación de usuarios a Tableau Online

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Tableau Online. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Tableau Online:

*   [Asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-tableau-online"></a>Sugerencias importantes para asignar usuarios a Tableau Online

*   Se recomienda asignar un único usuario de Azure AD a Tableau Online para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar un usuario a Tableau Online, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-tableau-online"></a>Configuración del aprovisionamiento automático de usuarios en Tableau Online

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Tableau Online en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede optar por habilitar el inicio de sesión único basado en SAML para Tableau Online siguiendo las instrucciones del [Tutorial de inicio de sesión único de Tableau Online](tableauonline-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-tableau-online-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Tableau Online en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione Tableau Online en la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/AppInstanceSearch.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningCredentials.png)

5. En la sección **Credenciales de administrador**, escriba los valores de **Dominio**, **Nombre de usuario administrador**, **Contraseña de administrador** y **Dirección URL del contenido** de la cuenta de Tableau Online:

    *   En el campo **Dominio**, rellene el subdominio según el paso 6.

    *   En el campo **Nombre de usuario de administrador**, rellene el nombre de usuario de la cuenta de administrador de su inquilino de Clarizen. Ejemplo: admin@contoso.com.

    *   En el campo **Contraseña de administrador**, rellene la contraseña de la cuenta correspondiente al nombre de usuario de administrador.

    *   En el campo **Dirección URL del contenido**, rellene el subdominio según el paso 6.

6. Después de iniciar sesión en la cuenta administrativa de Tableau Online, los valores de **Dominio** y **Dirección URL del contenido** se pueden extraer de la dirección URL de la página de administración.

    *   El valor de **Dominio** de la cuenta de Tableau Online se puede copiar de esta parte de la dirección URL: ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/DomainUrlPart.png)

    *   El valor de **Dirección URL del contenido** de la cuenta de Tableau Online se puede copiar de esta sección y es un valor definido durante la configuración de la cuenta. En este ejemplo, el valor es "contoso": ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ContentUrlPart.png)

        > [!NOTE]
        > El valor de **Dominio** puede ser diferente del que se muestra aquí. 


7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD se puede conectar a Tableau Online. Si la conexión no se establece, asegúrese de que la cuenta de Tableau Online tiene permisos de administrador e inténtelo de nuevo.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/TestConnection.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/EmailNotification.png)

10. Haga clic en **Save**(Guardar).

11. En la sección **Asignaciones**, seleccione **Sincronizar usuarios de Azure Active Directory con Tableau**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/UserMappings.png)

12. Revise los atributos de usuario que se sincronizan entre Azure AD y Tableau Online en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Tableau Online con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/UserAttributeMapping.png)

13. En la sección **Asignaciones**, seleccione **Sincronizar grupos de Azure Active Directory con Tableau**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/GroupMappings.png)

14. Revise los atributos de grupo que se sincronizan entre Azure AD y Tableau Online en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Tableau Online con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/GroupAttributeMapping.png)

15. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../active-directory-saas-scoping-filters.md).

16. Para habilitar el servicio de aprovisionamiento de Azure AD para Tableau Online, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ProvisioningStatus.png)

17. Defina los usuarios o grupos que le gustaría aprovisionar en Tableau Online eligiendo los valores deseados en **Ámbito** en la sección **Configuración**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/ScopeSync.png)

18. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Tableau Online](./media/tableau-online-provisioning-tutorial/SaveProvisioning.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Tableau Online.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/tableau-online-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/tableau-online-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/tableau-online-provisioning-tutorial/tutorial_general_03.png
