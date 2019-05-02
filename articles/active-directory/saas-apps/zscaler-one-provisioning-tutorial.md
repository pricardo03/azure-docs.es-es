---
title: 'Tutorial: Configurar Zscaler One para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y cancelar el aprovisionamiento de cuentas de usuario de Zscaler One automáticamente.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd-msft
ms.assetid: 72f6ba2b-73ed-420a-863a-aff672f26fa3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: v-ant-msft
ms.openlocfilehash: 5319b0ac06c4ddf1a7627a4e7fe0bfb2694f79f6
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706593"
---
# <a name="tutorial-configure-zscaler-one-for-automatic-user-provisioning"></a>Tutorial: Configurar Zscaler One para aprovisionar usuarios automáticamente

Este tutorial muestra los pasos necesarios para realizar en Zscaler One y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y cancelar el aprovisionamiento de usuarios y grupos a Zscaler One automáticamente.

> [!NOTE]
> Este tutorial describe un conector que se basa en el servicio de aprovisionamiento de usuario de Azure AD. Para obtener información sobre lo que hace este servicio, cómo funciona y las preguntas más frecuentes, consulte [automatización del aprovisionamiento y Desaprovisionamiento de usuarios para aplicaciones de software-como-servicio (SaaS) con Azure Active Directory](../active-directory-saas-app-provisioning.md).
>
> Este conector está disponible actualmente como versión preliminar. Para obtener más información sobre los términos de Microsoft Azure generales de uso para características de vista previa, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

El escenario descrito en este tutorial se supone que tiene:

* Un inquilino de Azure AD.
* Un inquilino de Zscaler One.
* Una cuenta de usuario de Zscaler One con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en la API de Zscaler una SCIM. Esta API está disponible para los desarrolladores de Zscaler One para las cuentas con el paquete de la empresa.

## <a name="add-zscaler-one-from-the-azure-marketplace"></a>Agregar Zscaler One desde Azure Marketplace

Antes de configurar Zscaler One para aprovisionar automáticamente usuarios con Azure AD, agregar Zscaler One desde Azure Marketplace a la lista de aplicaciones SaaS administradas.

Para agregar Zscaler One desde Marketplace, siga estos pasos.

1. En el [portal Azure](https://portal.azure.com), en el panel de navegación de la izquierda, seleccione **Azure Active Directory**.

    ![El icono de Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Zscaler One** y seleccione **Zscaler One** desde el panel de resultados. Para agregar la aplicación, seleccione **agregar**.

    ![Zscaler One en la lista de resultados](common/search-new-app.png)

## <a name="assign-users-to-zscaler-one"></a>Asignar a usuarios a Zscaler One

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, se sincronizan sólo los usuarios o grupos que se asignaron a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, decidir qué usuarios o grupos en Azure AD necesitan acceso a Zscaler One. Para asignar estos usuarios o grupos a Zscaler One, siga las instrucciones de [asignar un usuario o grupo a una aplicación empresarial](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

### <a name="important-tips-for-assigning-users-to-zscaler-one"></a>Sugerencias importantes para asignar usuarios a Zscaler One

* Se recomienda asignar un único usuario de Azure AD a Zscaler One para probar la configuración del aprovisionamiento automático de usuarios. Puede asignar usuarios o grupos adicionales más adelante.

* Al asignar un usuario a Zscaler One, seleccione un rol válido específico de la aplicación, si está disponible, en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configure-automatic-user-provisioning-to-zscaler-one"></a>Configurar el aprovisionamiento automático de usuarios a Zscaler One

En esta sección le guiará por los pasos para configurar el servicio de aprovisionamiento de Azure AD. Usar para crear, actualizar y deshabilitar usuarios o grupos en Zscaler One basándose en las asignaciones de grupo o usuario de Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Zscaler One. Siga las instrucciones de la [Zscaler One tutorial de inicio de sesión único](zscaler-One-tutorial.md). Inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características complementan entre sí.

### <a name="configure-automatic-user-provisioning-for-zscaler-one-in-azure-ad"></a>Configurar el aprovisionamiento automático de usuarios de Zscaler One en Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). Seleccione **aplicaciones empresariales** > **todas las aplicaciones** > **Zscaler One**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Zscaler One**.

    ![En la lista de aplicaciones en el vínculo Zscaler One](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Zscaler One](./media/zscaler-one-provisioning-tutorial/provisioning-tab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Un modo de aprovisionamiento de Zscaler](./media/zscaler-one-provisioning-tutorial/provisioning-credentials.png)

5. En el **las credenciales de administrador** sección, rellene el **dirección URL del inquilino** y **Token secreto** cuadros con la configuración de Zscaler One cuenta como se describe en el paso 6.

6. Para obtener el inquilino de dirección URL y el token de secreto, vaya a **administración** > **configuración de autenticación** en la IU del portal de Zscaler One. En **tipo de autenticación**, seleccione **SAML**.

    ![Configuración de autenticación de Zscaler One](./media/zscaler-one-provisioning-tutorial/secret-token-1.png)

     a. Seleccione **configurar SAML** para abrir el **configurar SAML** opciones.

    ![Configuración de Zscaler One SAML](./media/zscaler-one-provisioning-tutorial/secret-token-2.png)

    b. Seleccione **Enable SCIM-Based aprovisionamiento** para obtener la configuración de **URL Base** y **el Token de portador**. A continuación, guarde la configuración. Copia el **URL Base** si se establece en **dirección URL del inquilino** en Azure portal. Copia el **el Token de portador** si se establece en **Token secreto** en Azure portal.

7. Una vez que rellene los cuadros que se muestra en el paso 5, seleccione **Probar conexión** para asegurarse de que Azure AD puede conectarse a Zscaler One. Si se produce un error en la conexión, asegúrese de que la cuenta de Zscaler One tiene permisos de administrador y vuelva a intentarlo.

    ![Conexión de una prueba de Zscaler](./media/zscaler-one-provisioning-tutorial/test-connection.png)

8. En el **correo electrónico de notificación** cuadro, escriba la dirección de correo electrónico de la persona o grupo para recibir las notificaciones de error de aprovisionamiento. Seleccione el **enviar una notificación por correo electrónico cuando se produce un error** casilla de verificación.

    ![Correo electrónico de notificación de Zscaler One](./media/zscaler-one-provisioning-tutorial/notification.png)

9. Seleccione **Guardar**.

10. En el **asignaciones** sección, seleccione **sincronizar Azure usuarios Active Directory con Zscaler One**.

    ![Sincronización de usuario de Zscaler One](./media/zscaler-one-provisioning-tutorial/user-mappings.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Zscaler One en el **asignaciones de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario de Zscaler One para las operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de usuario coincidentes Zscaler One](./media/zscaler-one-provisioning-tutorial/user-attribute-mappings.png)

12. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a Zscaler One**.

    ![Sincronización de grupos de Zscaler One](./media/zscaler-one-provisioning-tutorial/group-mappings.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD con Zscaler One en el **asignaciones de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para que coincidan con los grupos en Zscaler One para las operaciones de actualización. Para guardar los cambios, seleccione **guardar**.

    ![Atributos de grupo coincidentes de Zscaler One](./media/zscaler-one-provisioning-tutorial/group-attribute-mappings.png)

14. Para configurar filtros de ámbito, siga las instrucciones de la [tutorial de filtro de ámbito](./../active-directory-saas-scoping-filters.md).

15. Para habilitar el servicio para Zscaler One, el aprovisionamiento en Azure AD la **configuración** sección, cambie **estado de aprovisionamiento** a **en**.

    ![Un estado de aprovisionamiento Zscaler](./media/zscaler-one-provisioning-tutorial/provisioning-status.png)

16. Definir los usuarios o grupos que desea que se aprovisionen en Zscaler One. En el **configuración** , seleccione los valores que desee en **ámbito**.

    ![Ámbito de Zscaler One](./media/zscaler-one-provisioning-tutorial/scoping.png)

17. Cuando esté listo para aprovisionar, seleccione **guardar**.

    ![Zscaler One Save](./media/zscaler-one-provisioning-tutorial/save-provisioning.png)

Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **ámbito** en el **configuración** sección. La sincronización inicial tardará más tiempo en realizarse que las sincronizaciones posteriores. Se producen aproximadamente cada 40 minutos mientras se ejecuta el servicio de aprovisionamiento de Azure AD. 

Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento. El informe describe todas las acciones realizadas por el servicio en Zscaler One de aprovisionamiento de Azure AD.

Para obtener información acerca de cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../active-directory-saas-provisioning-reporting.md)

<!--Image references-->
[1]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/zscaler-one-provisioning-tutorial/tutorial-general-03.png
