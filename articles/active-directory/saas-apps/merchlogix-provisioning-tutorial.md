---
title: 'Tutorial: Configuración de MerchLogix para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario a MerchLogix.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe36969661ae1b729601681c02f79e777b2f8cab
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344574"
---
# <a name="tutorial-configure-merchlogix-for-automatic-user-provisioning"></a>Tutorial: Configurar MerchLogix para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en MerchLogix y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar automáticamente y cancelar el aprovisionamiento de usuarios o grupos a MerchLogix. 

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

*   Un inquilino de Azure AD
*   Un inquilino de MerchLogix
*   Un contacto técnico en MerchLogix que puede proporcionar la dirección URL del punto de conexión SCIM y token secreto necesario para el aprovisionamiento de usuarios

## <a name="adding-merchlogix-from-the-gallery"></a>Adición de MerchLogix desde la Galería
Antes de configurar MerchLogix para aprovisionar automáticamente usuarios con Azure AD, deberá agregar MerchLogix desde la Galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar MerchLogix desde la Galería de aplicaciones de Azure AD, realice los pasos siguientes:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]
    
3. Para agregar MerchLogix, haga clic en el **nueva aplicación** botón en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **MerchLogix**.

5. En el panel de resultados, seleccione **MerchLogix**y, a continuación, haga clic en el **agregar** para agregar MerchLogix a la lista de aplicaciones SaaS.

    ![Aprovisionamiento de MerchLogix][4]

## <a name="assigning-users-to-merchlogix"></a>Asignación de usuarios a MerchLogix

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD. 

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceso a MerchLogix. Una vez decidido, puede asignar dichos usuarios o grupos a MerchLogix siguiendo estas instrucciones:

*   [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-merchlogix"></a>Sugerencias importantes para asignar usuarios a MerchLogix

*    Se recomienda que un único usuario de Azure AD se asigna a MerchLogix para probar el configuración del aprovisionamiento de usuarios automático inicial. Más tarde, se pueden asignar otros usuarios o grupos una vez realizadas las pruebas correctamente.

*   Al asignar a un usuario a MerchLogix, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-merchlogix"></a>Configuración del aprovisionamiento automático de usuarios a MerchLogix 

Esta sección le guiará a través de los pasos para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar a usuarios o grupos en MerchLogix en función de las asignaciones de grupos en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de MerchLogix, siguiendo las instrucciones en el [tutorial de inicio de sesión único de MerchLogix](merchlogix-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-merchlogix-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios de MerchLogix en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione MerchLogix desde la lista de aplicaciones de SaaS.

3. Seleccione la pestaña **Aprovisionamiento**.

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de MerchLogix](./media/merchlogix-provisioning-tutorial/Merchlogix1.png)

5. En el **las credenciales de administrador** sección:

    *   En el **dirección URL del inquilino** , escriba la dirección URL de punto de conexión SCIM proporcionada por el contacto técnico de MerchLogix.

    *   En el **Token secreto** , introduzca el token secreto proporcionado por el contacto técnico de MerchLogix.

6. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a MerchLogix. Si se produce un error en la conexión, asegúrese de que la cuenta de MerchLogix tiene permisos de administrador y vuelva a intentarlo.

    
7. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

8. Haga clic en **Save**(Guardar).

9. En el **asignaciones** sección, seleccione **sincronizar Active Directory usuarios de Azure a MerchLogix**.

10. Revise los atributos de usuario que se sincronizan entre Azure AD con MerchLogix en la **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario en MerchLogix para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

11. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a MerchLogix**.

12. Revise los atributos de grupo que se sincronizan entre Azure AD con MerchLogix en la **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para que coincidan con los grupos en MerchLogix para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

13. Para habilitar el servicio de MerchLogix de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección.

14. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad, que describe todas las acciones realizadas por el servicio en MerchLogix de aprovisionamiento de Azure AD de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: common/select-azuread.png
[2]: common/enterprise-applications.png
[3]: common/add-new-app.png
[4]: common/search-new-app.png
