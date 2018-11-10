---
title: 'Tutorial: integración de Azure Active Directory con BlueJeans | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y BlueJeans.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: dfc634fd-1b55-4ba8-94a8-b8288429b6a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: jeedes
ms.openlocfilehash: cff1512c56dba9907adbf1bb4452f11d47d0787d
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50095235"
---
# <a name="tutorial-azure-active-directory-integration-with-bluejeans"></a>Tutorial: Integración de Azure Active Directory con BlueJeans

En este tutorial, obtendrá información sobre cómo integrar BlueJeans con Azure Active Directory (Azure AD).

La integración de BlueJeans con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a BlueJeans.
- Puede permitir que los usuarios inicien sesión automáticamente en BlueJeans (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con BlueJeans, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en BlueJeans

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de BlueJeans desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-bluejeans-from-the-gallery"></a>Incorporación de BlueJeans desde la galería

Para configurar la integración de BlueJeans en Azure AD, será preciso que agregue BlueJeans desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar BlueJeans desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **BlueJeans**, seleccione **BlueJeans** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![BlueJeans en la lista de resultados](./media/bluejeans-tutorial/tutorial_bluejeans_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con BlueJeans con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de BlueJeans para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de BlueJeans.

Para configurar y probar el inicio de sesión único de Azure AD con BlueJeans, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de BlueJeans](#creating-a-bluejeans-test-user)**: para tener un homólogo de Britta Simon en BlueJeans que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación BlueJeans.

**Para configurar el inicio de sesión único de Azure AD con BlueJeans, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **BlueJeans**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/bluejeans-tutorial/tutorial_general_301.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/bluejeans-tutorial/tutorial_bluejeans_editurl.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de BlueJeans](./media/bluejeans-tutorial/tutorial_bluejeans_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.BlueJeans.com`.

    > [!NOTE]
    > El valor del inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de BlueJeans](https://support.bluejeans.com/contact) para obtener el valor.

6. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/bluejeans-tutorial/tutorial_bluejeans_certficate.png) 

7. En la sección **Configurar BlueJeans**, copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de BlueJeans](./media/bluejeans-tutorial/tutorial_bluejeans_configure.png) 

8. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la empresa de **BlueJeans**.

9. Vaya a **ADMIN \> CONFIGURACIÓN DE GRUPO \> SEGURIDAD**.

    ![Administración](./media/bluejeans-tutorial/IC785868.png "Administración")

10. En la sección **SEGURIDAD**, realice estos pasos:

    ![Inicio de sesión único de SAML](./media/bluejeans-tutorial/IC785869.png "Inicio de sesión único de SAML")

    a. Seleccione **SAML Single Sign On**(Inicio de sesión único de SAML).

    b. Seleccione **Enable automatic provisioning**(Habilitar aprovisionamiento automático).

11. Continúe con los siguiente pasos:

    ![Ruta del certificado](./media/bluejeans-tutorial/IC785870.png "Ruta del certificado")

    a. Haga clic en **Elegir archivo** para cargar el certificado codificado en base 64 que descargó de Azure Portal.

    b. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **Password Change URL** (Dirección URL de cambio de contraseña), pegue el valor de **Cambiar dirección URL de contraseña** que copió de Azure Portal.

    d. En el cuadro de texto **URL de cierre de sesión**, pegue el valor de **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal.

12. Continúe con los siguiente pasos:

    ![Guardar cambios](./media/bluejeans-tutorial/IC785874.png "Guardar cambios")

    a. En el cuadro de texto **Id. de usuario**, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    b. En el cuadro de texto **Correo electrónico**, escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.

    c. Haga clic en **GURDAR CAMBIOS**.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/bluejeans-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/bluejeans-tutorial/create_aaduser_02.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-bluejeans-test-user"></a>Creación de un usuario de prueba de BlueJeans

El objetivo de esta sección es crear un usuario llamado Britta Simon en BlueJeans. BlueJeans admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](bluejeans-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión como administrador en el sitio de la compañía de **BlueJeans** .

2. Vaya a **ADMIN \> ADMINISTRAR USUARIOS \> AGREGAR USUARIO**.

    ![Administración](./media/bluejeans-tutorial/IC785877.png "Administración")

    >[!IMPORTANT]
    >La pestaña **AGREGAR USUARIO** está disponible solo si en la pestaña **SEGURIDAD**, está desactivada la opción **Habilitar aprovisionamiento automático**. 

3. En la sección **AGREGAR USUARIO**, lleve a cabo estos pasos:

    ![Agregar usuario](./media/bluejeans-tutorial/IC785886.png "Agregar usuario")

    a. En el cuadro de texto **First Name** (Nombre), escriba el nombre de usuario, en este caso **Britta**.

    b. En el cuadro de texto **Last Name** (Apellidos), escriba los apellidos del usuario, en este caso **Simon**.

    c. En el cuadro de texto **Elegir un nombre de usuario de BlueJeans**, escriba el nombre de usuario del usuario, como **Brittasimon**

    d. En el cuadro de texto **Crear una contraseña**, escriba su contraseña.

    e. En el cuadro de texto **Compañía**, escriba su compañía.

    f. En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba el correo electrónico del usuario, por ejemplo, **brittasimon@contoso.com**.

    g. En el cuadro de texto **Create a BlueJeans Meeting I.D** (Crear un id. de reunión de BlueJeans), escriba el identificador de la reunión.

    h. En el cuadro de texto **Pick a Moderator Passcode** (Elegir un código de acceso de moderador), escriba el código de acceso.

    i. Haga clic en **CONTINUE** (Continuar).

    ![Agregar usuario](./media/bluejeans-tutorial/IC785887.png "Add User")

    J. Haga clic en **ADD USER** (Agregar usuario).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de BlueJeans que proporcione BlueJeans para aprovisionar cuentas de usuario de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a BlueJeans.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **BlueJeans**.

    ![Configurar inicio de sesión único](./media/bluejeans-tutorial/tutorial_bluejeans_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de BlueJeans en el Panel de acceso, debe iniciar sesión automáticamente en su aplicación de BlueJeans.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/bluejeans-tutorial/tutorial_general_01.png
[2]: ./media/bluejeans-tutorial/tutorial_general_02.png
[3]: ./media/bluejeans-tutorial/tutorial_general_03.png
[4]: ./media/bluejeans-tutorial/tutorial_general_04.png

[100]: ./media/bluejeans-tutorial/tutorial_general_100.png

[200]: ./media/bluejeans-tutorial/tutorial_general_200.png
[201]: ./media/bluejeans-tutorial/tutorial_general_201.png
[202]: ./media/bluejeans-tutorial/tutorial_general_202.png
[203]: ./media/bluejeans-tutorial/tutorial_general_203.png
