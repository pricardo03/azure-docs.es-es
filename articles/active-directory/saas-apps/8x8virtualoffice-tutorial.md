---
title: 'Tutorial: integración de Azure Active Directory con 8x8 Virtual Office | Microsoft Azure'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y 8x8 Virtual Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 53db637bf7ad47896747b491fcbe31123fdb104e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741817"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Tutorial: Integración de Azure Active Directory con 8x8 Virtual Office

En este tutorial, obtendrá información sobre cómo integrar 8x8 Virtual Office con Azure Active Directory (Azure AD).

La integración de 8x8 Virtual Office con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a 8x8 Virtual Office.
- Puede permitir que los usuarios inicien sesión automáticamente en 8x8 Virtual Office (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con 8x8 Virtual Office, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en 8x8 Virtual Office

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de 8x8 Virtual Office desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Incorporación de 8x8 Virtual Office desde la galería

Para configurar la integración de 8x8 Virtual Office en Azure AD, es preciso agregar 8x8 Virtual Office desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar 8x8 Virtual Office desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **8x8 Virtual Office**, seleccione **8x8 Virtual Office** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![8x8 Virtual Office en la lista de resultados](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con 8x8 Virtual Office con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de 8x8 Virtual Office para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de 8x8 Virtual Office.

Para configurar y probar el inicio de sesión único de Azure AD con 8x8 Virtual Office, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de 8x8 Virtual Office](#creating-a-8x8-virtual-office-test-user)**: para tener un homólogo de Britta Simon en 8x8 Virtual Office que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación 8x8 Virtual Office.

**Para configurar el inicio de sesión único de Azure AD con 8x8 Virtual Office, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **8x8 Virtual Office**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de 8x8 Virtual Office](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL como: `https://sso.8x8.com/saml2`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL:`https://sso.8x8.com/saml2`

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_certificate.png) 

6. En la sección **Set up 8x8 Virtual Office** (Configurar 8x8 Virtual Office), copie la dirección URL adecuada según sus necesidades.

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de 8x8 Virtual Office](common/configuresection.png)

7. Inicie la sesión en el inquilino de 8x8 Virtual Office como administrador.

8. Seleccione el **administrador de cuentas de Virtual Office** en el panel de la aplicación.

    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

9. Seleccione la cuenta **empresarial** que se va a administrar y haga clic en el botón **Iniciar sesión**.

    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

10. Haga clic en la pestaña **CUENTAS** en la lista de menús.

    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

11. Haga clic en **Inicio de sesión único** en la lista de cuentas.
  
    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

12. Seleccione **Inicio de sesión único** en los método de autenticación y haga clic en **SAML**.

    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

13. En la sección **Inicio de sesión único de SAML**, siga estos pasos:

    ![Configurar en la aplicación](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. En el cuadro de texto **URL de inicio de sesión**, pegue el valor de la **URL de inicio de sesión** que ha copiado de Azure Portal.

    b. En el cuadro de texto **Sign Out URL** (URL de cierre de sesión), pegue el valor de la **URL de cierre de sesión** que ha copiado de Azure Portal.

    c. En el cuadro de texto **URL del emisor**, pegue el valor del **identificador de Azure AD** que ha copiado de Azure Portal.

    d. Haga clic en el botón **Examinar** para cargar el certificado que descargó de Azure Portal.

    e. Haga clic en el botón **Save** (Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
  
### <a name="creating-a-8x8-virtual-office-test-user"></a>Creación de un usuario de prueba de 8x8 Virtual Office

El objetivo de esta sección es crear un usuario llamado Britta Simon en 8x8 Virtual Office. 8x8 Virtual Office admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada.

No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a 8x8 Virtual Office se crea un nuevo usuario, en caso de que no exista.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de 8x8 Virtual Office](https://www.8x8.com/about-us/contact-us).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a 8x8 Virtual Office.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **8x8 Virtual Office**.

    ![Configurar inicio de sesión único](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de 8x8 Virtual Office en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación 8x8 Virtual Office.
Para más información sobre el Panel de acceso, vea la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
