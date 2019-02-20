---
title: 'Tutorial: integración de Azure Active Directory con ON24 Virtual Environment SAML Connection | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ON24 Virtual Environment SAML Connection.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0827895d58b0b7633ee4543495014c62b5394312
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56209496"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Tutorial: integración de Azure Active Directory con ON24 Virtual Environment SAML Connection

En este tutorial, obtendrá información sobre cómo integrar ON24 Virtual Environment SAML Connection con Azure Active Directory (Azure AD).

La integración de ON24 Virtual Environment SAML Connection con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a ON24 Virtual Environment SAML Connection.
- Puede permitir que los usuarios inicien sesión automáticamente en ON24 Virtual Environment SAML Connection (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ON24 Virtual Environment SAML Connection, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de ON24 Virtual Environment SAML Connection

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de ON24 Virtual Environment SAML Connection desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>Adición de ON24 Virtual Environment SAML Connection desde la galería
Para configurar la integración de ON24 Virtual Environment SAML Connection en Azure AD, debe agregar ON24 Virtual Environment SAML Connection desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ON24 Virtual Environment SAML Connection desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/on24-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/on24-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/on24-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **ON24 Virtual Environment SAML Connection**, seleccione **ON24 Virtual Environment SAML Connection** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/on24-tutorial/tutorial_on24_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ON24 Virtual Environment SAML Connection con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ON24 Virtual Environment SAML Connection para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ON24 Virtual Environment SAML Connection.

Para configurar y probar el inicio de sesión único de Azure AD con ON24 Virtual Environment SAML Connection, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de ON24 Virtual Environment SAML Connection](#create-an-on24-virtual-environment-saml-connection-test-user)**: para tener un homólogo de Britta Simon en ON24 Virtual Environment SAML Connection que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación ON24 Virtual Environment SAML Connection.

**Para configurar el inicio de sesión único de Azure AD con ON24 Virtual Environment SAML Connection, realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **ON24 Virtual Environment SAML Connection**, seleccione **Inicio de sesión único**.

    ![imagen](./media/on24-tutorial/B1_B2_Select_SSO.png)

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![imagen](./media/on24-tutorial/b1_b2_saml_ssso.png)

3. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/on24-tutorial/b1_b2_saml_sso.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/on24-tutorial/b1-domains_and_urlsedit.png)

5. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![imagen](./media/on24-tutorial/tutorial_on24_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL:

     **Dirección URL del entorno de producción**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **Dirección URL del entorno de control de calidad**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`
 
    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL:
    
     **Dirección URL del entorno de producción**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **Dirección URL del entorno de control de calidad**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. Haga clic en **Establecer direcciones URL adicionales**. 

    d. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

    e. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://vshow.on24.com/vshow/<INSTANCENAME>`

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el certificado adecuado según sus requisitos y guárdelo en el equipo.

    ![imagen](./media/on24-tutorial/tutorial_on24_certificate.png) 

7. Para configurar el inicio de sesión único en **ON24 Virtual Environment SAML Connection**, es preciso enviar el certificado y los metadatos que descargó desde Azure Portal al [equipo de soporte técnico de ON24 Virtual Environment SAML Connection](https://www.on24.com/about-us/support/). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/on24-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/on24-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/on24-tutorial/d_userproperties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-an-on24-virtual-environment-saml-connection-test-user"></a>Creación de un usuario de prueba de ON24 Virtual Environment SAML Connection

En esta sección, creará un usuario llamado Britta Simon en ON24 Virtual Environment SAML Connection. Póngase en contacto con el  [equipo de soporte técnico de conexión de ON24 Virtual Environment SAML Connection](https://www.on24.com/about-us/support/) para agregar usuarios en la plataforma de ON24 Virtual Environment SAML Connection. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ON24 Virtual Environment SAML Connection.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/on24-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **ON24 Virtual Environment SAML Connection**.

    ![imagen](./media/on24-tutorial/tutorial_on24_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/on24-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/on24-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ON24 Virtual Environment SAML Connection en el Panel de acceso, debe iniciar sesión automáticamente en la aplicación ON24 Virtual Environment SAML Connection.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

