---
title: 'Tutorial: Integración de Azure Active Directory con Informes de gastos de N2F | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Informes de gastos de N2F.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: f56d53d7-5a08-490a-bfb9-78fefc2751ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 27fb299bc3bbbbf75bdf40ae02eac627763ce6d4
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40007599"
---
# <a name="tutorial-azure-active-directory-integration-with-n2f---expense-reports"></a>Tutorial: Integración de Azure Active Directory con Informes de gastos de N2F

En este tutorial, aprenderá a integrar Informes de gastos de N2F con Azure Active Directory (Azure AD).

La integración de Informes de gastos de N2F con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Informes de gastos de N2F.
- Puede permitir que los usuarios inicien sesión automáticamente en Informes de gastos de N2F (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con los informes de gastos de N2F, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en los informes de gastos de N2F.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de los informes de gastos de N2F desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-n2f---expense-reports-from-the-gallery"></a>Incorporación de los informes de gastos de N2F desde la galería

Para configurar la integración de los informes de gastos de N2F con Azure AD, es preciso agregar dichos informes desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar los informes de gastos de N2F desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Informes de gastos de N2F**, seleccione **Informes de gastos de N2F** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Informes de gastos de N2F en la lista de resultados](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con los informes de gastos de N2F con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita saber cuál es el usuario homólogo de Informes de gastos de N2F para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Informes de gastos de N2F.

Para configurar y probar el inicio de sesión único de Azure AD con Informes de gastos de N2F, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Informes de gastos de N2F](#create-a-n2f---expense-reports-test-use)**: para tener un homólogo de Britta Simon en Informes de gastos de N2F que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Informes de gastos de N2F.

**Para configurar el inicio de sesión único de Azure AD con Informes de gastos de N2F, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Informes de gastos de N2F**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_samlbase.png)

3. En la sección **N2F - Expense reports Domain and URLs** (Dominio y direcciones URL de Informes de gastos de N2F), si quiere configurar la aplicación en modo iniciado por **IDP**, el usuario no tiene que realizar ningún paso ya que la aplicación ya está integrada previamente con Azure.

    ![Información de dominio y direcciones URL de inicio de sesión único de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url1.png)

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_url2.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://www.n2f.com/app/`

5. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_certificate.png)

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/n2f-expensereports-tutorial/tutorial_general_400.png)

7. En la sección **Configuración de Informes de gastos de N2F**, haga clic en **Configurar Informes de gastos de N2F** para abrir la ventana **Configurar inicio de sesión único**. Copie el valor de **Identificador de entidad de SAML** de la **sección Referencia rápida**

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_configure.png)

8. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de Informes de gastos de N2F.

9. Haga clic en **Settings** (Configuración) y seleccione **Advance Settings** (Configuración avanzada) en la lista desplegable.

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/configure1.png)

10. Seleccione la pestaña **Account settings** (Configuración de la cuenta).

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/configure2.png)

11. Seleccione **Authentication** (Autenticación) y luego seleccione la pestaña **+ Add an authentication method** (+Agregar un método de autenticación).

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/configure3.png)

12. Seleccione **SAML Microsoft Office 365** como método de autenticación.

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/configure4.png)

13. En la sección **Authentication method** (Método deautenticación), realice los pasos siguientes:

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/configure5.png)

    a. En el cuadro de texto **Entity ID** (Id. de entidad), pegue el valor de **SAML Entity ID** (Id. de entidad de SAML) que copió de Azure Portal.

    b. En el cuadro de texto **Metadata URL** (Dirección URL de metadatos), pegue el valor de **App Federation Metadata Url** (Dirección URL de metadatos de federación de aplicaciones) que copió de Azure Portal.

    c. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/n2f-expensereports-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/n2f-expensereports-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/n2f-expensereports-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/n2f-expensereports-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-n2f---expense-reports-test-user"></a>Creación de un usuario de prueba de Informes de gastos de N2F

Para permitir que los usuarios de Azure AD inicien sesión en Informes de gastos de N2F, deben aprovisionarse en esta aplicación. En el caso de Informes de gastos de N2F, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión como administrador en el sitio de la compañía de Informes de gastos de N2F.

2. Haga clic en **Settings** (Configuración) y seleccione **Advance Settings** (Configuración avanzada) en la lista desplegable.

   ![Informes de gastos de N2F: agregar usuario](./media/n2f-expensereports-tutorial/configure1.png)

3. Seleccione la pestaña **Users** (Usuarios) en el panel de navegación izquierdo.

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/user1.png)

4. Seleccione la pestaña **+ New user** (+Nuevo usuario).

   ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/user2.png)

5. En la sección **User** (Usuario), realice los siguientes pasos:

    ![Configuración de Informes de gastos de N2F](./media/n2f-expensereports-tutorial/user3.png)

    a. En el cuadro de texto **Email address** (Dirección de correo electrónico), escriba la dirección de correo electrónico del usuario, como **brittasimon@contoso.com**.

    b. En el cuadro de texto **Nombre**, escriba el nombre de usuario, en este caso **Britta**.

    c. En el cuadro de texto **Name** (Nombre), escriba el nombre de usuario, por ejemplo, **BrittaSimon**.

    d. Elija **Role, Direct manager (N+1)** (Role, responsable directo [N+1]) y **Division** (División) según los requisitos de su organización.

    e. Haga clic en **Validate and send invitation** (Validar y enviar invitación).

    > [!NOTE]
    > Si tiene algún problema al agregar el usuario, póngase en contacto con el [equipo de soporte técnico de Informes de gastos de N2F](mailto:support@n2f.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, permitirá que Britta Simon use el inicio de sesión único de Azure concediéndole acceso a Informes de gastos de N2F.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Informes de gastos de N2F, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Informes de gastos de N2F**.

    ![El vínculo Informes de gastos de N2F en la lista de aplicaciones](./media/n2f-expensereports-tutorial/tutorial_n2f-expensereports_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Informes de gastos de N2F, iniciará sesión automáticamente en su aplicación Informes de gastos de N2F.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/n2f-expensereports-tutorial/tutorial_general_01.png
[2]: ./media/n2f-expensereports-tutorial/tutorial_general_02.png
[3]: ./media/n2f-expensereports-tutorial/tutorial_general_03.png
[4]: ./media/n2f-expensereports-tutorial/tutorial_general_04.png

[100]: ./media/n2f-expensereports-tutorial/tutorial_general_100.png

[200]: ./media/n2f-expensereports-tutorial/tutorial_general_200.png
[201]: ./media/n2f-expensereports-tutorial/tutorial_general_201.png
[202]: ./media/n2f-expensereports-tutorial/tutorial_general_202.png
[203]: ./media/n2f-expensereports-tutorial/tutorial_general_203.png

