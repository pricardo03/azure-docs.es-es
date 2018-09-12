---
title: 'Tutorial: Configuración de Cisco Spark para aprovisionar usuarios automáticamente con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y desaprovisionar automáticamente las cuentas de usuario para Cisco Spark.
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
ms.openlocfilehash: aafbde6907e59be3b0ff1d5807ffe4a7d2fffaa4
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346119"
---
# <a name="tutorial-configure-cisco-spark-for-automatic-user-provisioning"></a>Tutorial: Configuración de Cisco Spark para el aprovisionamiento automático de usuarios


El objetivo de este tutorial es mostrar los pasos que se realizan en Cisco Spark y Azure Active Directory (Azure AD) para configurar Azure AD de modo que aprovisione y desaprovisione automáticamente los usuarios en Cisco Spark.


> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md).

## <a name="prerequisites"></a>Requisitos previos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

*   Un inquilino de Azure AD
*   Un inquilino de Cisco Spark
*   Una cuenta de usuario de Cisco Spark con permisos de administrador


> [!NOTE]
> La integración del aprovisionamiento de Azure AD utiliza el [servicio web de Cisco Spark](https://developer.webex.com/getting-started.html), que está disponible para los equipos de Cisco Spark.

## <a name="adding-cisco-spark-from-the-gallery"></a>Adición de Cisco Spark desde la galería
Antes de configurar Cisco Spark para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Cisco Spark desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Cisco Spark desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Sección Aplicaciones empresariales][2]

3. Para agregar Cisco Spark, haga clic en el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Cisco Spark**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/AppSearch.png)

5. En el panel de resultados, seleccione **Cisco Spark** y, luego, haga clic en el botón **Agregar** para agregar Cisco Spark a la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/AppSearchResults.png)

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/AppCreation.png)

## <a name="assigning-users-to-cisco-spark"></a>Asignación de usuarios a Cisco Spark

Azure Active Directory usa un concepto que se denomina "asignaciones" para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto de aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han "asignado" a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios de Azure AD necesitan acceder a Cisco Spark. Una vez decidido, puede asignar estos usuarios a Cisco Spark siguiendo estas instrucciones:

*   [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-cisco-spark"></a>Sugerencias importantes para asignar usuarios a Cisco Spark

*   Se recomienda asignar un único usuario de Azure AD a Cisco Spark para probar la configuración del aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios.

*   Al asignar un usuario a Cisco Spark, debe seleccionar un rol específico de la aplicación válido (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="configuring-automatic-user-provisioning-to-cisco-spark"></a>Configuración del aprovisionamiento automático de usuarios en Cisco Spark

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de Azure AD para crear, actualizar y deshabilitar usuarios en Cisco Spark en función de las asignaciones de usuarios de Azure AD.


### <a name="to-configure-automatic-user-provisioning-for-cisco-spark-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Cisco Spark en Azure AD:


1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory > Aplicaciones empresariales > Todas las aplicaciones**.

2. Seleccione Cisco Spark en la lista de aplicaciones SaaS.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/Successcenter2.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/ProvisioningTab.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/ProvisioningCredentials.png)

5. En la sección **Credenciales de administrador**, escriba **URL de inquilino** y **Token secreto** de la cuenta de Cisco Spark.

    *   En el campo **URL de inquilino**, complete la dirección URL de la API SCIM de Cisco Spark para su inquilino, que adopta la forma de `https://api.ciscospark.com/v1/scim/[Tenant ID]/`, donde `[Tenant ID]` es una cadena alfanumérica, como se describe en el paso 6.

    *   En el campo **Token secreto**, rellene el token secreto tal y como se describe en el paso 6.

1. Para encontrar el **id. de inquilino** y el **token secreto** para la cuenta de Cisco Spark, inicie sesión en el [sitio para desarrolladores de Cisco Spark](https://developer.webex.com/) con su cuenta de administrador. Una vez que haya iniciado sesión:
    * Vaya a la página [Getting Started](https://developer.webex.com/getting-started.html) (Introducción).
    * Desplácese hasta la sección [Authentication](https://developer.webex.com/getting-started.html#authentication) (Autenticación) 
    ![Cisco Spark Authentication Token](./media/cisco-spark-provisioning-tutorial/SecretToken.png) (Token de autenticación de Cisco Spark).
    * La cadena alfanumérica en el cuadro es el **token secreto**. Copie este token en el Portapapeles.
    * Vaya a la página [Get My Own Details](https://developer.webex.com/endpoint-people-me-get.html) (Obtener mis propios detalles).
        * Asegúrese de que Test Mode (Modo de prueba) esté establecido en ON (Activado).
        * Escriba la palabra "Bearer" seguida de un espacio y, luego, copie el token secreto en el campo Authorization (Autorización). ![Token de autenticación de Cisco Spark](./media/cisco-spark-provisioning-tutorial/GetMyDetails.png)
        * Haga clic en Ejecutar.
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

1. Tras rellenar los campos que se muestran en el paso 5, haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Cisco Spark. Si se produce un error en la conexión, asegúrese de que la cuenta de Cisco Spark tenga permisos de administrador y vuelva a intentarlo.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/TestConnection.png)

8. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/EmailNotification.png)

9. Haga clic en **Save**(Guardar).

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Cisco Spark** (Sincronizar usuarios de Azure Active Directory con Cisco Spark).

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/UserMapping.png)

11. Revise los atributos de usuario que se sincronizan entre Azure AD y Cisco Spark en la sección **Asignación de atributos**. Los atributos seleccionados como propiedades de **Coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Cisco Spark con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/UserMappingAttributes.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para Cisco Spark, cambie el **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/ProvisioningStatus.png)

14. Para definir los usuarios o grupos que quiere que se aprovisionen en Cisco Spark, elija los valores deseados en **Ámbito**, en la sección **Configuración**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/SyncScope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Aprovisionamiento de Cisco Spark](./media/cisco-spark-provisioning-tutorial/Save.png)


Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de la actividad de aprovisionamiento, que describe todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Cisco Spark.

Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte el tutorial de [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../manage-apps/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Limitaciones del conector

* Actualmente, Cisco Spark se encuentra en la fase de pruebas de campo tempranas (EFT) de Cisco. Para más información, póngase en contacto con el [equipo de soporte técnico de Cisco](https://www.webex.co.in/support/support-overview.html). 

## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)


## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../manage-apps/check-status-user-account-provisioning.md)

<!--Image references-->
[1]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_01.png
[2]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_02.png
[3]: ./media/cisco-spark-provisioning-tutorial/tutorial_general_03.png
