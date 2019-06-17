---
title: 'Tutorial: integración de Azure Active Directory con Kanbanize | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Kanbanize.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b436d2f6-bfa5-43fd-a8f9-d2144dc25669
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22c136225e5a8526afd482e5ef8400198947422f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60264237"
---
# <a name="tutorial-azure-active-directory-integration-with-kanbanize"></a>Tutorial: integración de Azure Active Directory con Kanbanize

En este tutorial, aprenderá a integrar Kanbanize con Azure Active Directory (Azure AD).

La integración de Kanbanize con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Kanbanize.
- Puede permitir que los usuarios inicien sesión automáticamente en Kanbanize (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Kanbanize, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción a Kanbanize habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Kanbanize desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-kanbanize-from-the-gallery"></a>Adición de Kanbanize desde la galería
Para configurar la integración de Kanbanize en Azure AD, deberá agregar Kanbanize desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Kanbanize desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Kanbanize**, seleccione **Kanbanize** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Kanbanize en la lista de resultados](./media/kanbanize-tutorial/tutorial_kanbanize_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Kanbanize con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Kanbanize para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Kanbanize.

Para configurar y probar el inicio de sesión único de Azure AD con Kanbanize, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Kanbanize](#create-a-kanbanize-test-user)** : para tener un homólogo de Britta Simon en Kanbanize que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Kanbanize.

**Para configurar el inicio de sesión único de Azure AD con Kanbanize, siga estos pasos:**

1. En Azure Portal, en la página de integración de aplicaciones de **Kanbanize**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/kanbanize-tutorial/tutorial_kanbanize_samlbase.png)

3. En la sección **Kanbanize Domain and URLs** (Dominio y direcciones URL de Kanbanize), realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.kanbanize.com/`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.kanbanize.com/saml/acs`.

    c. Active **Mostrar configuración avanzada de URL**.

    d.  En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `/ctrl_login/saml_login`

    e. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL siguiendo este patrón: `https://<subdomain>.kanbanize.com`
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Kanbanize](mailto:support@ms.kanbanize.com) para obtener estos valores. 

5. La aplicación Kanbanize espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de pantalla le muestra un ejemplo de esto. El valor predeterminado de **Identificador de usuario** es **user.userprincipalname**, pero Kanbanize espera que este valor se asigne a la dirección de correo electrónico del usuario. Para ello, puede usar el atributo **user.mail** de la lista o usar el valor de atributo correspondiente en función de la configuración de su organización.
    
    ![Configurar inicio de sesión único](./media/kanbanize-tutorial/tutorial_Kanbanize_attribute.png)

6. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/kanbanize-tutorial/tutorial_kanbanize_certificate.png) 

7. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/kanbanize-tutorial/tutorial_general_400.png)
    
8. En la sección **Configuración de Kanbanize**, haga clic en **Configurar Kanbanize** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_configure.png)

9. En otra ventana del explorador web, inicie sesión en Kanbanize como administrador de seguridad. 

10. Vaya a la parte superior derecha de la página y haga clic en el logotipo de **configuración**.

    ![Configuración de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_set.png)

11. En la página del panel de administración del lado izquierdo del menú, haga clic en **Integrations** (Integraciones) y, a continuación, habilite **Single Sign-On** (Inicio de sesión único). 

    ![Integraciones de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_admin.png)

12. En la sección de integraciones, haga clic en **CONFIGURE** (CONFIGURAR) para abrir la página **Single Sign-On Integration** (Integración de inicio de sesión único).

    ![Configuración de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_config.png)

13. En la página **Single Sign-On Integration** (Integración de inicio de sesión único), en **Configurations** (Configuraciones), realice los siguientes pasos:

    ![Integraciones de Kanbanize](./media/kanbanize-tutorial/tutorial_kanbanize_save.png)

    a. En el cuadro de texto **IdP Entity ID** (Id. de entidad IdP), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que copió de Azure Portal.

    b. En el cuadro de texto **Idp Login Endpoint** (Punto de conexión de inicio de sesión de IdP), pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que ha copiado de Azure Portal.

    c. En el cuadro de texto **Idp Logout Endpoint** (Punto de conexión de cierre de sesión de IdP), pegue el valor de la **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal.

    d. En **Attribute name for Email** (Nombre de atributo de correo electrónico), escriba este valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. En el cuadro de texto **Attribute name for First Name** (Nombre de atributo para nombre), escriba este valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.

    f. En el cuadro de texto **Attribute name for Last Name** (Nombre de atributo para apellido), escriba este valor `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`. 
    > [!Note]
    > Puede obtener estos valores mediante la combinación de valores de espacio de nombres y nombre del atributo correspondiente de la sección Atributos de usuario de Azure Portal.

    g. En el Bloc de notas, abra el certificado codificado en base 64 que descargó de Azure Portal, copie su contenido (sin los marcadores de inicio y fin) y péguelo en el cuadro de texto  **Idp X.509 Certificate**  (Certificado X.509 de IdP).

    h. Marque **Enable login with both SSO and Kanbanize** (Habilitar el inicio de sesión con SSO y Kanbanize).
    
    i. Haga clic en **Guardar configuración**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/kanbanize-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/kanbanize-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/kanbanize-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/kanbanize-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-kanbanize-test-user"></a>Creación de un usuario de prueba de Kanbanize

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Kanbanize. Kanbanize admite aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Kanbanize, se crea un nuevo usuario, en caso de que no exista.

>[!Note]
>Si necesita crear manualmente un usuario, es preciso que se ponga contacto con el  [equipo de soporte técnico de Kanbanize](mailto:support@ms.kanbanize.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Kanbanize.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Kanbanize, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Kanbanize**.

    ![Vínculo a Kanbanize en la lista de aplicaciones](./media/kanbanize-tutorial/tutorial_kanbanize_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Kanbanize en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Kanbanize.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kanbanize-tutorial/tutorial_general_01.png
[2]: ./media/kanbanize-tutorial/tutorial_general_02.png
[3]: ./media/kanbanize-tutorial/tutorial_general_03.png
[4]: ./media/kanbanize-tutorial/tutorial_general_04.png

[100]: ./media/kanbanize-tutorial/tutorial_general_100.png

[200]: ./media/kanbanize-tutorial/tutorial_general_200.png
[201]: ./media/kanbanize-tutorial/tutorial_general_201.png
[202]: ./media/kanbanize-tutorial/tutorial_general_202.png
[203]: ./media/kanbanize-tutorial/tutorial_general_203.png

