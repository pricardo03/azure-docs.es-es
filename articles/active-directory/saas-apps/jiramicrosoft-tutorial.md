---
title: 'Tutorial: Integración de Azure Active Directory con JIRA SAML SSO by Microsoft | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y JIRA SAML SSO by Microsoft.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4b663047-7f88-443b-97bd-54224b232815
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: cc87985404ef8c9ee625f32b359e6ac1a29e73ae
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "42143187"
---
# <a name="tutorial-azure-active-directory-integration-with-jira-saml-sso-by-microsoft"></a>Tutorial: Integración de Azure Active Directory con JIRA SAML SSO by Microsoft

En este tutorial, obtendrá información sobre cómo integrar JIRA SAML SSO by Microsoft con Azure Active Directory (Azure AD).

La integración de JIRA SAML SSO by Microsoft con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a JIRA SAML SSO by Microsoft.
- Puede permitir que los usuarios inicien sesión automáticamente en JIRA SAML SSO by Microsoft (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="description"></a>DESCRIPCIÓN

Use la cuenta de Microsoft Azure Active Directory con el servidor JIRA de Atlassian para habilitar el inicio de sesión único. De esta forma, todos los usuarios de la organización pueden usar las credenciales de Azure AD para iniciar sesión en la aplicación JIRA. Este complemento usa SAML 2.0 para la federación.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con JIRA SAML SSO by Microsoft, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- JIRA Core y Software versiones de 6.0 a 7.8, o JIRA Service Desk versiones de 3.0 a 3.2, deben instalarse y configurarse en una versión de 64 bits de Windows
- El servidor JIRA es compatible con HTTPS
- Tenga en cuenta que las versiones admitidas para el complemento JIRA se mencionan en la sección siguiente.
- El servidor JIRA es accesible en Internet, especialmente a la página de inicio de sesión de Azure AD para la autenticación y debe poder recibir el token de Azure AD
- Las credenciales de administrador se configuran en JIRA
- WebSudo está deshabilitado en JIRA
- Usuario de prueba creado en la aplicación de servidor JIRA

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción de JIRA. Pruebe primero la integración en el entorno de desarrollo o de ensayo de la aplicación y, después, use el entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, aquí puede obtener una versión de prueba de un mes: [oferta de versión de prueba](https://azure.microsoft.com/pricing/free-trial/).

## <a name="supported-versions-of-jira"></a>Versiones compatibles de JIRA

*   JIRA Core y Software: de la versión 6.0 a la 7.8
*   JIRA Service Desk versiones de 3.0 a 3.2
*   JIRA también admite 5.2. Para obtener más detalles, haga clic en [Microsoft Azure Active Directory single sign-on for JIRA 5.2](jira52microsoft-tutorial.md) (Inicio de sesión único de Microsoft Azure Active Directory para JIRA 5.2).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar JIRA SAML SSO by Microsoft desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-jira-saml-sso-by-microsoft-from-the-gallery"></a>Agregar JIRA SAML SSO by Microsoft desde la galería
Para configurar la integración de JIRA SAML SSO by Microsoft en Azure AD, deberá agregar JIRA SAML SSO by Microsoft desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar JIRA SAML SSO by Microsoft desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **JIRA SAML SSO by Microsoft**, seleccione **JIRA SAML SSO by Microsoft** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![JIRA SAML SSO by Microsoft se encuentra en la lista de resultados](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con JIRA SAML SSO by Microsoft con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de JIRA SAML SSO by Microsoft para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de JIRA SAML SSO by Microsoft.

Para configurar y probar el inicio de sesión único de Azure AD con JIRA SAML SSO by Microsoft, es necesario completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de JIRA SAML SSO by Microsoft](#create-a-jira-saml-sso-by-microsoft-test-user)**: para tener un homólogo de Britta Simon en JIRA SAML SSO by Microsoft que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación JIRA SAML SSO by Microsoft.

**Para configurar el inicio de sesión único de Azure AD con JIRA SAML SSO by Microsoft, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **JIRA SAML SSO by Microsoft**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_samlbase.png)

3. En la sección **Dominio y direcciones URL de JIRA SAML SSO by Microsoft**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de JIRA SAML SSO by Microsoft](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<domain:port>/plugins/servlet/saml/auth`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<domain:port>/`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<domain:port>/plugins/servlet/saml/auth`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. En caso de que sea una dirección URL con nombre, el puerto es opcional. Estos valores se reciben durante la configuración del complemento de Jira, que se explica más adelante en el tutorial.

4. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/tutorial_metadataurl.png)

5. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/tutorial_general_400.png)

6. En otra ventana del explorador web, inicie sesión en la instancia de JIRA como administrador.

7. Mantenga el mouse encima del icono de engranaje y haga clic en **Complementos**.

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/addon1.png)

8. Descargue el complemento del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=56506). Cargue manualmente el complemento proporcionado por Microsoft mediante el menú **Cargar complemento**. La descarga del complemento está contemplada en el [Acuerdo de servicio de Microsoft](https://www.microsoft.com/servicesagreement/).

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/addon12.png)

9. Una vez instalado el complemento, aparece en la sección de complementos **Instalados por el usuario** de **Administrar complemento**. Haga clic en **Configurar** para configurar el nuevo complemento.

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/addon13.png)

10. Siga estos pasos en la página de configuración:

    ![Configurar inicio de sesión único](./media/jiramicrosoft-tutorial/addon52.png)

    > [!TIP]
    > Asegúrese de que hay un solo certificado asignado a la aplicación, de forma que no se produzca ningún error en la resolución de los metadatos. Si hay varios certificados, después de resolver los metadatos, el administrador recibe un error.

    a. En el cuadro de texto **URL de metadatos**, pegue la **dirección URL de metadatos de federación de aplicación** que copió en Azure Portal y haga clic en el botón **Resolver**. Se lee la dirección URL de metadatos de IdP y se rellena toda la información de campos.

    b. Copie los valores **Identificador, Dirección URL de respuesta y Dirección URL de inicio de sesión**, y péguelos en los cuadros de texto **Identificador, Dirección URL de respuesta y Dirección URL de inicio de sesión** respectivamente en la sección **Dominio y direcciones URL de JIRA SAML SSO by Microsoft** de Azure Portal.

    c. En **Nombre del botón de inicio de sesión** escriba el nombre del botón que la organización quiere que los usuarios vean en la pantalla de inicio de sesión.

    d. En **SAML User ID Locations** (Ubicaciones de Id. de usuario de SAML) seleccione **User ID is in the NameIdentifier element of the Subject statement** (El Id. de usuario está en el elemento NameIdentifier de la instrucción Subject) o **User ID is in an Attribute element** (El Id. de usuario está en un elemento Attribute).  Este identificador debe ser el identificador de usuario de JIRA. Si el identificador de usuario no coincide, el sistema no permitirá que los usuarios inicien sesión.

    > [!Note]
    > La ubicación del Id. de usuario de SAML predeterminada es el identificador de nombre. Puede cambiarlo a una opción de atributo y escribir el nombre de atributo adecuado.

    e. Si selecciona la opción **User ID is in an Attribute element** (El Id. de usuario está en un elemento Attribute), escriba el nombre del atributo cuando se espera el Id. de usuario en el cuadro de texto **Nombre del atributo**.

    f. Si se usa el dominio federado (por ejemplo, ADFS, etc.) con Azure AD, haga clic en la opción **Habilitar detección de dominio principal** y configure el **nombre de dominio**.

    g. En **Nombre de dominio**, escriba el nombre del dominio en el caso de inicios de sesión basados en ADFS.

    h. Active **Habilitar cierre de sesión único** si quiere que se cierre la sesión de Azure AD cuando un usuario cierra la sesión de JIRA.

    i. Haga clic en el botón **Save** (Guardar) para guardar la configuración.

    > [!NOTE]
    > Para más información sobre la instalación y la solución de problemas, visite [MS JIRA SSO Connector Admin Guide](../ms-confluence-jira-plugin-adminguide.md) (Guía de administración del conector SSO de MS JIRA), que contiene una sección de [preguntas frecuentes](../ms-confluence-jira-plugin-faq.md) para ayudarle.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/jiramicrosoft-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/jiramicrosoft-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/jiramicrosoft-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/jiramicrosoft-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-jira-saml-sso-by-microsoft-test-user"></a>Creación de un usuario de prueba de JIRA SAML SSO by Microsoft

Para permitir que los usuarios de Azure AD inicien sesión en el servidor local de JIRA, se deben aprovisionar en JIRA SAML SSO by Microsoft. Para JIRA SAML SSO by Microsoft, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el servidor local de JIRA.

2. Mantenga el mouse encima del icono de engranaje y haga clic en **Administración de usuarios**.

    ![Agregar empleado](./media/jiramicrosoft-tutorial/user1.png)

3. Se le redirigirá a la página de acceso de administrador para especificar la **contraseña** y haga clic en el botón **Confirmar**.

    ![Agregar empleado](./media/jiramicrosoft-tutorial/user2.png)

4. En la sección de la pestaña **Administración de usuarios**, haga clic en **Crear usuario**.

    ![Agregar empleado](./media/jiramicrosoft-tutorial/user3.png) 

5. En la página del cuadro de diálogo **"Create New User"** (Crear nuevo usuario), realice los pasos siguientes:

    ![Agregar empleado](./media/jiramicrosoft-tutorial/user4.png) 

    a. En el cuadro de texto **Dirección de correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    b. En el cuadro de texto **Nombre completo**, escriba el nombre completo de un usuario, por ejemplo, Britta Simon.

    c. En el cuadro de texto **Nombre de usuario**, escriba el correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    d. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.

    e. Haga clic en **Crear usuario**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a JIRA SAML SSO by Microsoft.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a JIRA SAML SSO by Microsoft, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **JIRA SAML SSO by Microsoft**.

    ![El enlace de JIRA SAML SSO by Microsoft se encuentra en la lista de aplicaciones](./media/jiramicrosoft-tutorial/tutorial_singlesign-onforjira_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de JIRA SAML SSO by Microsoft en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación JIRA SAML SSO by Microsoft.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/msaadssojira-tutorial/tutorial_general_01.png
[2]: ./media/msaadssojira-tutorial/tutorial_general_02.png
[3]: ./media/msaadssojira-tutorial/tutorial_general_03.png
[4]: ./media/msaadssojira-tutorial/tutorial_general_04.png

[100]: ./media/msaadssojira-tutorial/tutorial_general_100.png

[200]: ./media/msaadssojira-tutorial\tutorial_general_200.png
[201]: ./media/msaadssojira-tutorial\tutorial_general_201.png
[202]: ./media/msaadssojira-tutorial\tutorial_general_202.png
[203]: ./media/msaadssojira-tutorial\tutorial_general_203.png