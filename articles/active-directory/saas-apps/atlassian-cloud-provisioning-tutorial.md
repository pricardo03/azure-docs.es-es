---
title: 'Tutorial: Configuración de Atlassian Cloud para aprovisionar automáticamente usuarios con Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente cuentas de usuario en Atlassian Cloud.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: na
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/03/2019
ms.author: v-ant
ms.openlocfilehash: ff6a111663df5d7e7f4f517446084ef75b009e59
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58092708"
---
# <a name="tutorial-configure-atlassian-cloud-for-automatic-user-provisioning"></a>Tutorial: Configurar Atlassian Cloud para aprovisionar usuarios automáticamente

El objetivo de este tutorial es mostrar los pasos que se realizan en Atlassian Cloud y Azure Active Directory (Azure AD) para configurar Azure AD para aprovisionar y desaprovisionar automáticamente usuarios o grupos en Atlassian Cloud. 

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).
> 
> Este conector está actualmente en versión preliminar pública. Para obtener más información sobre los términos de Microsoft Azure generales de uso para características de vista previa, consulte [condiciones de uso complementarias para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

*   Un inquilino de Azure AD
*   [Un inquilino de Atlassian Cloud](https://www.atlassian.com/licensing/cloud)
*   Una cuenta de usuario en Atlassian Cloud con permisos de administrador.

> [!NOTE]
> La integración del aprovisionamiento de Azure AD se basa en el **API SCIM de Atlassian Cloud**, que está disponible para los equipos de Atlassian Cloud.

## <a name="adding-atlassian-cloud-from-the-gallery"></a>Incorporación de Atlassian Cloud desde la galería
Antes de configurar Atlassian Cloud para aprovisionar automáticamente usuarios con Azure AD, deberá agregar Atlassian Cloud desde la Galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Atlassian Cloud desde la Galería de aplicaciones de Azure AD, realice los pasos siguientes:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]
    
3. Para agregar Atlassian Cloud, haga clic en el **nueva aplicación** botón en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Atlassian Cloud**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/app-search.png)

5. En el panel de resultados, seleccione **Atlassian Cloud**y, a continuación, haga clic en el **agregar** para agregar Atlassian Cloud a la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/app-create.png)

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/app-instance.png)

## <a name="assigning-users-to-atlassian-cloud"></a>Asignación de usuarios a Atlassian Cloud

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD. 

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceso a Atlassian Cloud. Una vez decidido, puede asignar dichos usuarios o grupos a Atlassian Cloud, siga estas instrucciones:

*   [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-atlassian-cloud"></a>Sugerencias importantes para asignar usuarios a Atlassian Cloud

*   Se recomienda que un único usuario de Azure AD se asigna a Atlassian Cloud para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

*   Al asignar a un usuario en Atlassian Cloud, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-atlassian-cloud"></a>Configuración del aprovisionamiento automático de usuarios a Atlassian Cloud 

Esta sección le guiará a través de los pasos para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar a usuarios o grupos en Atlassian Cloud en función de las asignaciones de grupo en Azure AD.

> [!TIP]
> También puede habilitar basado en SAML sesión único de Atlassian Cloud, siga las instrucciones proporcionadas en el [tutorial de inicio de sesión único de Atlassian Cloud](atlassian-cloud-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-atlassian-cloud-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios de Atlassian Cloud en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione Atlassian Cloud desde la lista de aplicaciones de SaaS.
 
    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/application-instance-search.png)

3. Seleccione la pestaña **Aprovisionamiento**.
    
    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-tab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/credentials.png)

5. En el **credenciales de administrador** sección, de entrada el **dirección URL del inquilino** y **Token secreto** de cuenta de Atlassian Cloud. Algunos ejemplos de estos valores son:

   * En el **dirección URL del inquilino** , a continuación, rellene el punto de conexión específico del inquilino recibirá desde el Atlassian, tal como se describe en el paso 6. Por ejemplo: **https://api.atlassian.com/scim/directory/{directoryId}**

   * En el campo **Token secreto**, rellene el token secreto tal y como se describe en el paso 6.

6. Vaya a [Administrador de organización de Atlassian](https://admin.atlassian.com) **> aprovisionamiento de usuarios** y haga clic en **crear un Token**. Copia el **dirección URL base del directorio** y **el Token de portador** a la **dirección URL del inquilino** y **Token secreto** campos respectivamente.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-1.png) ![aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-2.png)
    
    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/secret-token-3.png)

7. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Atlassian Cloud. Si se produce un error en la conexión, asegúrese de que la cuenta de Atlassian Cloud tiene permisos de administrador y vuelva a intentarlo.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/test-connection.png)
    
8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/notification.png)

9. Haga clic en **Save**(Guardar).

10. En el **asignaciones** sección, seleccione **sincronizar Active Directory usuarios de Azure con Atlassian Cloud**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provision-users.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD con Atlassian Cloud en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para coincidir con las cuentas de usuario en Atlassian Cloud para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/user-mapping.png)

12. En el **asignaciones** sección, seleccione **sincronizar Azure grupos de Active Directory a Atlassian Cloud**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provision-groups.png)

13. Revise los atributos de grupo que se sincronizan entre Azure AD con Atlassian Cloud en el **asignación de atributos** sección. Los atributos seleccionados como **coincidencia** propiedades se utilizan para que coincidan con los grupos en Atlassian Cloud para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/group-mapping.png)

14. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

15. Para habilitar el servicio de Atlassian Cloud de aprovisionamiento de Azure AD, cambie el **estado de aprovisionamiento** a **en** en el **configuración** sección.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-on.png)

16. Defina los usuarios o grupos que le gustaría que se aprovisionen en Atlassian Cloud eligiendo los valores deseados en **ámbito** en el **configuración** sección.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/provisioning-options.png)

17. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Atlassian Cloud](./media/atlassian-cloud-provisioning-tutorial/save.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar el **detalles de sincronización** sección para supervisar el progreso y seguir los vínculos al informe de actividad, que describe todas las acciones realizadas por el servicio en Atlassian Cloud de aprovisionamiento de Azure AD de aprovisionamiento.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Atlassian Cloud permite el aprovisionamiento de usuarios solo de [dominios comprobados](https://confluence.atlassian.com/cloud/organization-administration-938859734.html).
* Atlassian Cloud no admite cambios de nombre de grupo hoy en día. Esto significa que los cambios realizados en la propiedad displayName de un grupo en Azure AD no se actualiza y reflejados en Atlassian Cloud.
* El valor de la **correo** atributo de usuario de Azure AD solo se rellena si el usuario tiene un buzón de Exchange de Microsoft. Si el usuario no tiene uno, se recomienda asignar un atributo diferente deseado para el **correos electrónicos** atributo en Atlassian Cloud.

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-01.png
[2]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-02.png
[3]: ./media/atlassian-cloud-provisioning-tutorial/tutorial-general-03.png
