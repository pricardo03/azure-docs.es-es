---
title: 'Tutorial: Integración de Azure Active Directory con 8x8 Virtual Office | Microsoft Azure'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y 8x8 Virtual Office.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/27/2018
ms.author: jeedes
ms.openlocfilehash: 11e316d2dc0811faaf3a9daab219386441fb3bb8
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54823647"
---
# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Tutorial: Integración de Azure Active Directory con 8x8 Virtual Office

En este tutorial, obtendrá información sobre cómo integrar 8x8 Virtual Office con Azure Active Directory (Azure AD).
La integración de 8x8 Virtual Office con Azure AD proporciona las siguientes ventajas:

* En Azure AD puede controlar quién tiene acceso a 8x8 Virtual Office.
* Puede permitir que los usuarios inicien sesión automáticamente en 8x8 Virtual Office (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con 8x8 Virtual Office, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción habilitada para el inicio de sesión único en 8x8 Virtual Office

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.


* 8 x 8 Virtual Office admite inicio de sesión único iniciado por **IDP**.

* 8x8 Virtual Office admite aprovisionamiento de usuarios **Just In Time**.

## <a name="adding-8x8-virtual-office-from-the-gallery"></a>Incorporación de 8x8 Virtual Office desde la galería

Para configurar la integración de 8x8 Virtual Office en Azure AD, es preciso agregar 8x8 Virtual Office desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar 8x8 Virtual Office desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **8x8 Virtual Office**, seleccione **8x8 Virtual Office** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

     ![8x8 Virtual Office en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con 8x8 Virtual Office con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de 8x8 Virtual Office.

Para configurar y probar el inicio de sesión único de Azure AD con 8x8 Virtual Office, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de 8x8 Virtual Office](#configure-8x8-virtual-office-single-sign-on)**: para configurar el inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de 8x8 Virtual Office](#create-8x8-virtual-office-test-user)**: para tener un homólogo de Britta Simon en 8x8 Virtual Office que esté vinculado a la representación de ella en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con 8x8 Virtual Office, realice los pasos siguientes:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **8x8 Virtual Office**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, realice los siguientes pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de 8x8 Virtual Office](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://sso.8x8.com/saml2`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://sso.8x8.com/saml2`

4. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/certificateraw.png)

6. En la sección **Set up 8x8 Virtual Office** (Configurar 8x8 Virtual Office), copie la dirección URL adecuada según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-8x8-virtual-office-single-sign-on"></a>Configuración del inicio de sesión único de 8 x 8 Virtual Office

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

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a 8x8 Virtual Office.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y, luego, **8x8 Virtual Office**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **8x8 Virtual Office**.

    ![El vínculo de 8x8 Virtual Office en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-8x8-virtual-office-test-user"></a>Creación de un usuario de prueba de 8x8 Virtual Office

En esta sección, se crea un usuario llamado a Britta Simon en 8x8 Virtual Office. 8x8 Virtual Office admite el **aprovisionamiento de usuarios Just-In-Time**, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario deja de existir en 8x8 Virtual Office, se crea uno después de la autenticación.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico de 8x8 Virtual Office](https://www.8x8.com/about-us/contact-us).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de 8x8 Virtual Office en el Panel de acceso, debería iniciar sesión automáticamente en la solución 8x8 Virtual Office para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

