---
title: 'Tutorial: integración de Azure Active Directory con Egnyte | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Egnyte.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c2101d4-1779-4b36-8464-5c1ff780da18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2018
ms.author: jeedes
ms.openlocfilehash: 33989d04cba06c789089dfc0db62d177d258292a
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976118"
---
# <a name="tutorial-azure-active-directory-integration-with-egnyte"></a>Tutorial: integración de Azure Active Directory con Egnyte

En este tutorial, aprenderá a integrar Egnyte con Azure Active Directory (Azure AD).

Integrar Egnyte con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Egnyte.
- Puede permitir que los usuarios inicien sesión automáticamente en Egnyte (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Egnyte, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Egnyte

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Egnyte desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-egnyte-from-the-gallery"></a>Adición de Egnyte desde la galería

Para configurar la integración de Egnyte en Azure AD, deberá agregar Egnyte desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Egnyte desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Egnyte**, seleccione **Egnyte** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Egnyte en la lista de resultados](./media/egnyte-tutorial/tutorial_egnyte_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Egnyte con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Egnyte para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Egnyte.

Para configurar y probar el inicio de sesión único de Azure AD con Egnyte, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Egnyte](#creating-an-egnyte-test-user)**: para tener un homólogo de Britta Simon en Egnyte que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Egnyte.

**Para configurar el inicio de sesión único de Azure AD con Egnyte, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Egnyte**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Egnyte](./media/egnyte-tutorial/tutorial_egnyte_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.egnyte.com`.

    > [!NOTE] 
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Egnyte](https://www.egnyte.com/corp/contact_egnyte.html) para obtener este valor. 

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/egnyte-tutorial/tutorial_egnyte_certificate.png) 

6. En la sección **Configurar Egnyte**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Egnyte](common/configuresection.png)

7. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Egnyte como administrador.

8. Haga clic en **Configuración**.
   
    ![Configuración](./media/egnyte-tutorial/ic787819.png "Configuración")

9. En el menú, haga clic en **Configuración**.

    ![Configuración](./media/egnyte-tutorial/ic787820.png "Configuración")

10. Haga clic en la pestaña **Configuración** y luego haga clic en **Seguridad**.

    ![Seguridad](./media/egnyte-tutorial/ic787821.png "Seguridad")

11. En la sección **Autenticación de inicio de sesión único** , siga estos pasos:

    ![Autenticación de inicio de sesión único](./media/egnyte-tutorial/ic787822.png "Autenticación de inicio de sesión único")   
    
     a. En **Autenticación de inicio de sesión único**, seleccione **SAML 2.0**.
   
    b. En **Proveedor de identidades**, seleccione **AzureAD**.
   
    c. Pegue la **URL de inicio de sesión** que copió de Azure Portal en el cuadro de texto **URL de inicio de sesión del proveedor de identidades**.
   
    d. Pegue el **Identificador de Azure AD** que copió de Azure Portal en el cuadro de texto **Identity Provider Entity ID** (Id. de entidad del proveedor de identidades).
      
    e. Abra el certificado codificado en base 64 descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado de proveedor de identidades**.
   
    f. En **Asignación de usuario predeterminada**, seleccione **Dirección de correo electrónico**.
   
    g. En **Usar valor de emisor específico del dominio**, seleccione **Deshabilitado**.
   
    h. Haga clic en **Save**(Guardar).

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

### <a name="creating-an-egnyte-test-user"></a>Creación de un usuario de prueba de Egnyte

Para permitir que los usuarios de Azure AD inicien sesión en Egnyte, deben aprovisionarse en Egnyte. En el caso de Egnyte, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la compañía de **Egnyte** como administrador.

2. Vaya a **Configuración \> Usuarios y grupos**.

3. Haga clic en **Agregar nuevo usuario**y luego seleccione el tipo de usuario que quiera agregar.
   
    ![Usuarios](./media/egnyte-tutorial/ic787824.png "Usuarios")

4. En la sección **Nuevo usuario avanzado**, lleve a cabo estos pasos:
    
    ![Nuevo usuario estándar](./media/egnyte-tutorial/ic787825.png "Nuevo usuario estándar")   

     a. En el cuadro de texto **E-mail** (Correo electrónico), escriba el correo electrónico del usuario; por ejemplo, **Brittasimon@contoso.com**.

    b. En el cuadro de texto **Nombre de usuario**, escriba el nombre de un usuario; por ejemplo, **Britta Simon**.

    c. Seleccione **Inicio de sesión único** como **Tipo de autenticación**.
   
    d. Haga clic en **Save**(Guardar).
    
    >[!NOTE]
    >El titular de la cuenta de Azure Active Directory recibirá una notificación por correo electrónico.
    >

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Egnyte que proporcione Egnyte para aprovisionar cuentas de usuario de AAD.
> 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Egnyte.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Egnyte**.

    ![Configurar inicio de sesión único](./media/egnyte-tutorial/tutorial_egnyte_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Egnyte en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Egnyte.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

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
