---
title: 'Tutorial: Integración de Azure Active Directory con TextMagic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TextMagic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: a91d6878f509f47f029a3b8aaf7bbbe4e6f35435
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57869802"
---
# <a name="tutorial-azure-active-directory-integration-with-textmagic"></a>Tutorial: Integración de Azure Active Directory con TextMagic

En este tutorial, obtendrá información sobre cómo integrar TextMagic con Azure Active Directory (Azure AD).

La integración de TextMagic con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a TextMagic.
- Puede permitir que los usuarios inicien sesión automáticamente en TextMagic (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con TextMagic, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en TextMagic

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar TextMagic desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-textmagic-from-the-gallery"></a>Agregar TextMagic desde la galería

Para configurar la integración de TextMagic en Azure AD, deberá agregar TextMagic desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar TextMagic desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **TextMagic**, seleccione **TextMagic** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![TextMagic en la lista de resultados](./media/textmagic-tutorial/tutorial_textmagic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con TextMagic con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de TextMagic para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de TextMagic.

Para configurar y probar el inicio de sesión único de Azure AD con TextMagic, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de TextMagic](#creating-a-textmagic-test-user)**: para tener un homólogo de Britta Simon en TextMagic que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación TextMagic.

**Para configurar el inicio de sesión único de Azure AD con TextMagic, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **TextMagic**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de TextMagic](./media/textmagic-tutorial/tutorial_textmagic_url.png)

    En el cuadro de texto **Identificador**, escriba una dirección URL como: `https://my.textmagic.com/saml/metadata`

5. La aplicación TextMagic espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/textmagic-tutorial/i4-attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:

    | NOMBRE  | Atributo de origen  | Espacio de nombres |
    | --------------- | --------------- | --------------- |
    | company | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName               | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName            | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | phone               | user.telephonenumber |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    
     a. Haga clic en el **icono Editar** para editar el campo **Valor de identificador de nombre** de **user.userprinicipalname** a **user.mail**.

    ![Atributo de TextMagic](./media/textmagic-tutorial/tutorial_textmagic_email.png)

    b. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./common/new_save_attribute.png)

    ![imagen](./common/new_attribute_details.png)

    c. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    d. Escriba el valor de **Espacio de nombres**.

    e. Seleccione **Atributo** como origen.

    f. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    g. Haga clic en **Aceptar**.

    h. Haga clic en **Save**(Guardar). 

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/textmagic-tutorial/tutorial_textmagic_certificate.png) 

8. En la sección **Set up TextMagic** (Configurar TextMagic), copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de TextMagic](common/configuresection.png)

9. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de TextMagic como administrador.

10. Seleccione **Configuración de la cuenta** bajo el nombre de usuario.

    ![Configuración de TextMagic](./media/textmagic-tutorial/config1.png)

11. Haga clic en la pestaña **Inicio de sesión único (SSO)** y rellene los campos siguientes:  
    
    ![Configuración de TextMagic](./media/textmagic-tutorial/config2.png)

     a. En el cuadro de texto **Identity provider Entity ID:** (Identificador de entidad del proveedor de identidades), pegue el valor de  **Identificador de Azure AD**, que ha copiado de Azure Portal.

    b. En el cuadro de texto **Identity provider SSO URL:** (Dirección URL de SSO de proveedor de identidades), pegue el valor de  **Dirección URL de inicio de sesión**, que ha copiado de Azure Portal.

    c. En el cuadro de texto **Identity provider SLO URL:** (Dirección URL de SLO de proveedor de identidades), pegue el valor de  **URL de cierre de sesión**, que ha copiado de Azure Portal.

    d. Abra el **certificado codificado en base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado x509 público:** .

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el **nombre de usuario** , escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
  
### <a name="creating-a-textmagic-test-user"></a>Creación de un usuario de prueba de TextMagic

La aplicación admite el **aprovisionamiento de usuarios Just-In-Time** y, tras la autenticación, los usuarios se crearán automáticamente en la aplicación. Debe rellenar la información una vez en el primer inicio de sesión para activar la cuenta secundaria en el sistema.
No hay ningún elemento de acción para usted en esta sección.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a TextMagic.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **TextMagic**.

    ![Configurar inicio de sesión único](./media/textmagic-tutorial/tutorial_textmagic_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TextMagic en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación TextMagic.
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
