---
title: 'Tutorial: Configuración de Cisco para aprovisionar usuarios automáticamente con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario para Cisco Webex.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: v-wingf
ms.openlocfilehash: 8f5af3cba01e925591c9d90ea0e96ed78b2823e2
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44348385"
---
# <a name="tutorial-configure-cisco-webex-for-automatic-user-provisioning"></a>Tutorial: Configuración de Cisco Webex para el aprovisionamiento automático de usuarios


El objetivo de este tutorial es mostrar los pasos que se dan en Cisco Webex y Azure Active Directory (Azure AD) para configurar Azure AD de modo que aprovisione y desaprovisione automáticamente los usuarios en Cisco Webex.


> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

*   Un inquilino de Azure AD
*   Un inquilino de Cisco Webex
*   Una cuenta de usuario de Cisco Webex con permisos de administrador


> [!NOTE]
> La integración del aprovisionamiento de Azure AD utiliza el [servicio web de Cisco Webex](https://developer.webex.com/getting-started.html), que está disponible para los equipos de Cisco Webex.

## <a name="adding-cisco-webex-from-the-gallery"></a>Adición de Cisco Webex desde la galería
Antes de configurar Cisco Webex para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Cisco Webex desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Cisco Webex desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]

3. Para agregar Cisco Webex, haga clic en el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Cisco Webex**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/AppSearch.png)

5. En el panel de resultados, seleccione **Cisco Webex** y, luego, haga clic en el botón **Agregar** para agregar Cisco Webex a la lista de aplicaciones SaaS.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-webex"></a>Asignación de usuarios a Cisco Webex

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios de Azure AD necesitan acceder a Cisco Webex. Una vez decidido, puede asignar estos usuarios a Cisco Webex siguiendo estas instrucciones:

*   [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-webex"></a>Sugerencias importantes para asignar usuarios a Cisco Webex

*   Se recomienda asignar un único usuario de Azure AD a Cisco Webex para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios.

*   Al asignar un usuario a Cisco Webex, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-cisco-webex"></a>Configuración del aprovisionamiento automático de usuarios en Cisco Webex

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios en Cisco Webex en función de las asignaciones de usuarios de Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cisco-webex-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Cisco Webex en Azure AD:


1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione Cisco Webex en la lista de aplicaciones SaaS.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/Successcenter2.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/ProvisioningCredentials.png)

5. En la sección **Admin Credentials** (Credenciales de administrador), escriba **Tenant URL** (Dirección URL de inquilino) y **Secret Token** (Token secreto) de la cuenta de Cisco Webex.

    *   En el campo **Tenant URL**, complete la dirección URL de la API SCIM de Cisco Webex para su inquilino, que adopta la forma de `https://api.webex.com/v1/scim/[Tenant ID]/`, donde `[Tenant ID]` es una cadena alfanumérica, como se describe en el paso 6.

    *   En el campo **Secret Token**, rellene el token secreto tal y como se describe en el paso 6.

1. Para encontrar el **id. de inquilino** y el **token secreto** para la cuenta de Cisco Webex, inicie sesión en el [sitio para desarrolladores de Cisco Webex](https://developer.webex.com/) con su cuenta de administrador. Una vez que haya iniciado sesión:
    * Vaya a la página [Getting Started](https://developer.webex.com/getting-started.html) (Introducción).
    * Desplácese hasta la sección [Authentication](https://developer.webex.com/getting-started.html#authentication) (Autenticación) 
    ![Cisco Webex Authentication Token](./media/cisco-webex-provisioning-tutorial/SecretToken.png) (Token de autenticación de Cisco Webex).
    * La cadena alfanumérica en el cuadro es el **token secreto**. Copie este token en el Portapapeles.
    * Vaya a la página [Get My Own Details](https://developer.webex.com/endpoint-people-me-get.html) (Obtener mis propios detalles).
        * Asegúrese de que Test Mode (Modo de prueba) esté activado (ON).
        * Escriba la palabra "Bearer" seguida de un espacio, y luego copia el token secreto en el campo Authorization (Autorización). ![Token de autenticación de Cisco Webex](./media/cisco-webex-provisioning-tutorial/GetMyDetails.png)
        * Haga clic en Run (Ejecutar).
    * En el texto de la respuesta a la derecha, el **id. de inquilino** aparece como "orgId":

    ```json
    {
        "id": "(...)",
        "emails": [
            "admin.user@contoso.com"
        ],
        "displayName": "John Smith",
        "nickName": "John",
        "orgId": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
        (...)
    }
    ```

1. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Cisco Webex. Si la conexión no se establece, asegúrese de que la cuenta de Cisco Webex tenga permisos de administrador y pruebe otra vez.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/TestConnection.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/EmailNotification.png)

9. Haga clic en **Save**(Guardar).

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Cisco Webex** (Sincronizar usuarios de Azure Active Directory con Cisco Webex).

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/UserMapping.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD y Cisco Webex en la sección **Attribute Mapping** (Asignaciones de atributos). Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Cisco Webex con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/UserMappingAttributes.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el aprovisionamiento del servicio de aprovisionamiento de Azure AD para Cisco Webex, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/ProvisioningStatus.png)

14. Para definir los usuarios o grupos que desea que se aprovisionen en Cisco Webex, elija los valores deseados en **Ámbito**, en la sección **Configuración**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/SyncScope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento en Cisco Webex](./media/cisco-webex-provisioning-tutorial/Save.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y hacer clic en los vínculos al informe de la actividad de aprovisionamiento, que describe todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Cisco Webex.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Actualmente, Cisco Webex se encuentra en la fase de pruebas de campo tempranas (EFT) de Cisco. Para más información, póngase en contacto con el [equipo de soporte técnico de Cisco](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-provisioning-tutorial/tutorial_general_03.png
