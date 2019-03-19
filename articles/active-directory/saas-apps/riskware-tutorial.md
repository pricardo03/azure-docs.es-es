---
title: 'Tutorial: Integración de Azure Active Directory con Riskware | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Riskware.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81866167-b163-4695-8978-fd29a25dac7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3de8422efdbb7802e0532503e2733518e8050f6d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57842539"
---
# <a name="tutorial-azure-active-directory-integration-with-riskware"></a>Tutorial: Integración de Azure Active Directory con Riskware

En este tutorial, aprenderá a integrar Riskware con Azure Active Directory (Azure AD).

La integración de Riskware con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Riskware.
- Puede habilitar que los usuarios inicien sesión automáticamente en Riskware (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Riskware, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Riskware

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Riskware desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-riskware-from-the-gallery"></a>Agregar Riskware desde la galería
Para configurar la integración de Riskware en Azure AD, será preciso que agregue Riskware desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Riskware desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Riskware**, seleccione **Riskware** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Riskware en la lista de resultados](./media/riskware-tutorial/tutorial_riskware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Riskware con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Riskware para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Riskware.

Para configurar y probar el inicio de sesión único de Azure AD con Riskware, es preciso completar los siguientes bloques de compilación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Riskware](#create-a-riskware-test-user)**: para tener un homólogo de Britta Simon en Riskware que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Riskware.

**Para configurar el inicio de sesión único de Azure AD con Riskware, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Riskware**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/riskware-tutorial/tutorial_riskware_samlbase.png)

1. En la sección **Riskware Domain and URLs** (Dominio y direcciones URL de Riskware), lleve a cabo los pasos siguientes:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Riskware](./media/riskware-tutorial/tutorial_riskware_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL con el siguiente patrón:

    | Entorno| Patrón de dirección URL|
    |--|--|
    | UAT|  `https://riskcloud.net/uat?ccode=<COMPANYCODE>` |
    | PROD| `https://riskcloud.net/prod?ccode=<COMPANYCODE>` |
    | DEMO| `https://riskcloud.net/demo?ccode=<COMPANYCODE>` |
    |||

    b. En el cuadro de texto **Identificador (Id. de entidad)**, escriba la dirección URL:
    
    | Entorno| Patrón de dirección URL|
    |--|--|
    | UAT| `https://riskcloud.net/uat` |
    | PROD| `https://riskcloud.net/prod` |
    | DEMO| `https://riskcloud.net/demo` |
    |||

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Riskware](mailto:support@pansoftware.com.au) para obtener este valor.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/riskware-tutorial/tutorial_riskware_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/riskware-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Riskware**, haga clic en **Configurar Riskware** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_configure.png)

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Riskware.

1. En la parte superior derecha, haga clic en **Maintenance** (Mantenimiento) para abrir la página de mantenimiento.

    ![Mantener la configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. En la página de mantenimiento, haga clic en **Authentication** (Autenticación).

    ![Autenticar la configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_authen.png)

1. En la página **Authentication Configuration** (Configuración de la autenticación), realice los siguientes pasos:

    ![Autenticar la configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_config.png)

     a. En **Type** (Tipo), seleccione **SAML** para la autenticación.

    b. En el cuadro de texto **Code** (Código), escriba el código, por ejemplo, AZURE_UAT.

    c. En el cuadro de texto **Description** (Descripción), escriba la descripción como configuración de AZURE para SSO.

    d. En el cuadro de texto **Single Sign On Page** (Página de inicio de sesión), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    e. En el cuadro de texto **Sign-out Page** (Página de cierre de sesión), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    f. En el cuadro de texto **Post Form Field** (Campo de formulario Post), escriba el nombre del campo presente en la respuesta de post que contenga SAML como SAMLResponse.

    g. En el cuadro de texto **XML Identity Tag Name** (Nombre de la etiqueta de la identidad XML), escriba el atributo de tipo que contenga el identificador único de la respuesta SAML, como NameID.

    h. Abra  **XML de metadatos**  de Azure Portal en el Bloc de notas, copie el certificado del archivo de metadatos y péguelo en el cuadro de texto **Certificate** (Certificado).

    i. En el cuadro de texto **Consumer URL** (Dirección URL del consumidor), pegue el valor de **Reply URL** (Dirección URL de respuesta), que le proporcionará el equipo de soporte técnico.

    j. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **Identifier** (Identificador), que le proporcionará el equipo de soporte técnico.

    > [!Note]
    > Póngase en contacto con el [equipo de soporte técnico de Riskware](mailto:support@pansoftware.com.au) para obtener estos valores.

    k. Seleccione la casilla **Use POST** (Usar POST).

    l. Seleccione la casilla **Use SAML Request** (Usar solicitud SAML).

    m. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/riskware-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/riskware-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/riskware-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/riskware-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-riskware-test-user"></a>Crear un usuario de prueba en Riskware

Para permitir que los usuarios de Azure AD inicien sesión en Riskware, deben aprovisionarse en dicha solución. En Riskware, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Riskware como administrador de seguridad.

1. En la parte superior derecha, haga clic en **Maintenance** (Mantenimiento) para abrir la página de mantenimiento. 

    ![Mantener la configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_maintain.png)

1. En la página de mantenimiento, haga clic en **People** (Usuarios).

    ![Configuración de personas en Riskware](./media/riskware-tutorial/tutorial_riskware_people.png)

1. Seleccione la pestaña **Details** (Detalles) y lleve a cabo los pasos siguientes:

    ![Detalles de configuración de Riskware](./media/riskware-tutorial/tutorial_riskware_details.png)

     a. Seleccione **Person Type** (Tipo de usuario), por ejemplo, Employee (Empleado).

    b. En el cuadro de texto **First Name** (Nombre), escriba el nombre del usuario, en este caso **Britta**.

    c. En el cuadro de texto **Surname** (Apellido), escriba el apellido del usuario, en este caso **Simon**.

1. En la pestaña **Seguridad** , lleve a cabo estos pasos:

    ![Configuración de la seguridad en Riskware](./media/riskware-tutorial/tutorial_riskware_security.png)

     a. En la sección **Authentication** (Autenticación), seleccione el modo **Authentication** (Autenticación), que ha configurado como Configuración de AZURE para inicio de sesión único.

    b. En **detalles de inicio de sesión** sección la **Id. de usuario** cuadro de texto, escriba el correo electrónico del usuario, como **brittasimon\@contoso.com**.

    c. En el cuadro de texto **Password** (Contraseña), escriba la contraseña del usuario.

1. En la pestaña **Organization** (Organización), realice los pasos siguientes:

    ![Configuración de la organización en Riskware](./media/riskware-tutorial/tutorial_riskware_org.png)

     a. Seleccione la opción como organización **Level1**.

    b. En la sección **Person's Primary Workplace** (Área de trabajo principal del usuario), en el cuadro de texto **Location** (Ubicación), escriba la ubicación.

    c. En la sección **Employee** (Empleado), en **Employee Status** (Estado del empleado) seleccione Casual (Informal).

1. Haga clic en **Save**(Guardar).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Riskware.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Riskware, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Riskware**.

    ![Vínculo a Riskware en la lista de aplicaciones](./media/riskware-tutorial/tutorial_riskware_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Riskware en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Riskware.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/riskware-tutorial/tutorial_general_01.png
[2]: ./media/riskware-tutorial/tutorial_general_02.png
[3]: ./media/riskware-tutorial/tutorial_general_03.png
[4]: ./media/riskware-tutorial/tutorial_general_04.png

[100]: ./media/riskware-tutorial/tutorial_general_100.png

[200]: ./media/riskware-tutorial/tutorial_general_200.png
[201]: ./media/riskware-tutorial/tutorial_general_201.png
[202]: ./media/riskware-tutorial/tutorial_general_202.png
[203]: ./media/riskware-tutorial/tutorial_general_203.png

