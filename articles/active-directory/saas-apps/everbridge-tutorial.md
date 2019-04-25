---
title: 'Tutorial: Integración de Azure Active Directory con EverBridge | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y EverBridge.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 58d7cd22-98c0-4606-9ce5-8bdb22ee8b3e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1bb62c9a11971f72a6c96c4652b136c19812cb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60279509"
---
# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Tutorial: Integración de Azure Active Directory con EverBridge

En este tutorial, aprenderá a integrar EverBridge con Azure Active Directory (Azure AD).

La integración de EverBridge con Azure AD proporciona las siguientes ventajas:

- Le permite controlar en Azure AD quién tiene acceso a EverBridge.
- Puede habilitar a los usuarios para que inicien sesión automáticamente en EverBridge (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con EverBridge, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en EverBridge

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de EverBridge desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-everbridge-from-the-gallery"></a>Adición de EverBridge desde la galería

Para configurar la integración de EverBridge en Azure AD, es preciso agregar EverBridge desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar EverBridge desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **EverBridge**, seleccione **EverBridge** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![EverBridge en la lista de resultados](./media/everbridge-tutorial/tutorial_everbridge_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con EverBridge con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD tiene que saber cuál es el usuario homólogo de EverBridge para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de EverBridge.

Para configurar y probar el inicio de sesión único de Azure AD con EverBridge, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba para EverBridge](#creating-an-everbridge-test-user)**: para tener un homólogo de Britta Simon en EverBridge que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación EverBridge.

**Para configurar el inicio de sesión único de Azure AD con EverBridge, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **EverBridge**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

    >[!NOTE]
    >Tiene que realizar las configuraciones de la aplicación COMO portal administrador O BIEN como portal miembro en ambos extremos, es decir, en Azure Portal y en el portal de EverBridge.

4. Para configurar la aplicación **EverBridge** como **portal administrador de EverBridge**, en la sección **Configuración básica de SAML** realice estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://sso.everbridge.net/<API_Name>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://manager.everbridge.net/saml/SSO/<API_Name>/alias/defaultAlias`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de EverBridge](mailto:support@everbridge.com) para obtener estos valores.

5. Para configurar la aplicación **EverBridge** como **portal miembro de EverBridge**, en la sección **Configuración básica de SAML** realice estos pasos:

   * Si quiere configurar la aplicación en modo iniciado por **IDP**:

       ![Información sobre dominio y direcciones URL de inicio de sesión único de EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url1.png)

       * En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://sso.everbridge.net/<API_Name>/<Organization_ID>`

       * En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://member.everbridge.net/saml/SSO/<API_Name>/<Organization_ID>/alias/defaultAlias`.

   * Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

       ![Información sobre dominio y direcciones URL de inicio de sesión único de EverBridge](./media/everbridge-tutorial/tutorial_everbridge_url2.png)

       * En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://member.everbridge.net/saml/login/<API_Name>/<Organization_ID>/alias/defaultAlias?disco=true`.

     > [!NOTE]
     > Estos valores no son reales. Actualice estos valores con el identificador y las direcciones URL de inicio de sesión y de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de EverBridge](mailto:support@everbridge.com) para obtener estos valores.

6. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **archivo XML de metadatos de federación** y, luego, guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/everbridge-tutorial/tutorial_everbridge_certificate.png) 

7. En la sección **Configurar EverBridge**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de EverBridge](common/configuresection.png)

8. Para configurar el inicio de sesión único de la aplicación **EverBridge** como **portal administrador de EverBridge**, realice estos pasos: 
 
9. En otra ventana del explorador web, inicie sesión en EverBridge como administrador.

9. En el menú de la parte superior, haga clic en la pestaña **Settings** (Configuración) y seleccione **Single Sign-On** (Inicio de sesión único) en **Security** (Seguridad).
   
     ![Configurar inicio de sesión único](./media/everbridge-tutorial/tutorial_everbridge_002.png)
   
      a. En el cuadro de texto **Nombre**, escriba el nombre del proveedor de identificadores (por ejemplo, el nombre de la compañía).
   
     b. En el cuadro de texto **API Name** (nombre de API), escriba el nombre de la API.
   
     c. Haga clic en el botón **Elegir archivo** para cargar el archivo de metadatos que descargó en Azure Portal.
   
     d. Como SAML Identity Location (Ubicación de identidad de SAML), seleccione **Identity is in the NameIdentifier element of the Subject statement** (La identidad está en el elemento NameIdentifier de la instrucción de sujeto).
   
     e. En el cuadro de texto **Identity Provider Login URL** (Dirección URL del proveedor de identidades), pegue el valor de la **dirección URL de inicio de sesión** que ha copiado de Azure Portal.
   
     f. Para Service Provider Initiated Request Binding (Vinculación de solicitud iniciada por el proveedor de servicios), seleccione **Redirección HTTP**.

     g. Haga clic en **Guardar**

10. Para configurar el inicio de sesión único en la aplicación **EverBridge** como **portal miembro de EverBridge**, tiene que enviar el **archivo XML de metadatos de federación** descargados al [equipo de soporte técnico de EverBridge](mailto:support@everbridge.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon\@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
  
### <a name="creating-an-everbridge-test-user"></a>Creación de un usuario de prueba para EverBridge

En esta sección, creará una usuaria llamada Britta Simon en Everbridge. Colabore con el [equipo de soporte técnico de EverBridge](mailto:support@everbridge.com) mediante para agregar los usuarios a la plataforma EverBridge.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a EverBridge.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **EverBridge**.

    ![Configurar inicio de sesión único](./media/everbridge-tutorial/tutorial_everbridge_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de EverBridge en el panel de acceso, debería iniciar sesión automáticamente en su aplicación EverBridge.
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
