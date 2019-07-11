---
title: 'Tutorial: Integración de Azure Active Directory con Promapp | Microsoft Docs'
description: En este tutorial, aprenderá a configurar el inicio de sesión único entre Azure Active Directory y Promapp.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: 2ddb8777a6470c0e739545e71867a694022d1723
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67093597"
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a>Tutorial: Integración de Azure Active Directory con Promapp

En este tutorial, aprenderá a integrar Promapp con Azure Active Directory (Azure AD).
Esta integración ofrece las siguientes ventajas:

* Puede usar Azure AD para controlar quién tiene acceso a Promapp.
* Puede permitir que los usuarios inicien sesión automáticamente en Promapp (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información acerca de la integración de aplicaciones SaaS con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Promapp, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una evaluación gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción de Promapp que tenga habilitado el inicio de sesión único.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial configurará y probará el inicio de sesión único de Azure AD en un entorno de prueba.

* Promapp admite el inicio de sesión único iniciado por SP e IDP.

* Promapp admite el aprovisionamiento de usuarios Just-In-Time.

## <a name="add-promapp-from-the-gallery"></a>Agregar Promapp desde la galería

Para configurar la integración de Promapp en Azure AD, deberá agregar Promapp desde la galería a la lista de aplicaciones SaaS administradas.

1. En [Azure Portal](https://portal.azure.com), en el panel izquierdo, seleccione **Azure Active Directory**:

    ![Seleccione Azure Active Directory.](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**:

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación, seleccione **Nueva aplicación** en la parte superior de la ventana:

    ![Seleccionar Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Promapp**. Seleccione **Promapp** en los resultados de la búsqueda y, después, seleccione **Agregar**.

     ![Search Results](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Promapp con un usuario de prueba llamado Britta Simon.
Para habilitar el inicio de sesión único, tendrá que establecer una relación entre un usuario de Azure AD y el usuario correspondiente de Promapp.

Para configurar y probar el inicio de sesión único de Azure AD con Promapp, siga estos pasos:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** para permitir que los usuarios utilicen esta característica.
2. **[Configuración del inicio de sesión único en Promapp](#configure-promapp-single-sign-on)** en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** para probar el inicio de sesión único de Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** para permitir el inicio de sesión único de Azure AD para el usuario.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)** para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Promapp, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación Promapp, seleccione **Inicio de sesión único**:

    ![Selección de inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Selección de un método de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, seleccione el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**:

    ![Icono Editar](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, si quiere configurar la aplicación en modo iniciado por IdP, haga lo siguiente:

    ![Cuadro de diálogo Configuración básica de SAML](common/idp-intiated.png)

    1. En el cuadro **Identificador**, escriba una dirección URL con este patrón:

       | |
        |--|
        | `https://go.promapp.com/TENANTNAME/`|
        | `https://au.promapp.com/TENANTNAME/`|
        | `https://us.promapp.com/TENANTNAME/`|
        | `https://eu.promapp.com/TENANTNAME/`|
        | `https://ca.promapp.com/TENANTNAME/`|
        |   |

       > [!NOTE]
       > La integración de Azure AD con Promapp está configurada actualmente para la autenticación iniciada por el servicio. (es decir, si va a una dirección URL de Promapp, se inicia el proceso de autenticación). Sin embargo, el campo **Dirección URL de respuesta** es obligatorio.

    1. En el cuadro **Dirección URL de respuesta**, escriba una dirección URL con este patrón:

       `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate.aspx`

5. Seleccione **Establecer direcciones URL adicionales** si desea configurar la aplicación en el modo iniciado por SP. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente formato:

      `https://<DOMAINNAME>.promapp.com/TENANTNAME/saml/authenticate`

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Promapp](common/metadata-upload-additional-signon.png)

   

    > [!NOTE]
    > Estos valores son marcadores de posición. Debe usar los valores reales de identificador, dirección URL de respuesta o dirección URL de inicio de sesión. Para obtener los valores, póngase en contacto con el [equipo de soporte técnico de Promapp](https://www.promapp.com/about-us/contact-us/). También puede consultar los patrones que se muestran en el cuadro de diálogo **Configuración básica de SAML** de Azure Portal.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el vínculo **Descargar** situado junto a **Certificado (Base64)** , según sus requisitos, y guarde el certificado en el equipo:

    ![Vínculo de descarga del certificado](common/certificatebase64.png)

7. En la sección **Set up Promapp** (Configurar Promapp), copie las direcciones URL adecuadas según sus necesidades:

    ![Copia de las direcciones URL de configuración](common/copy-configuration-urls.png)

    1. **Dirección URL de inicio de sesión**

    1. **Identificador de Azure AD**.

    1. **Dirección URL de cierre de sesión**

### <a name="configure-promapp-single-sign-on"></a>Configuración del inicio de sesión único en Promapp

1. Inicie sesión en su sitio de la empresa Promapp como administrador.

2. En el menú de la parte superior de la ventana, seleccione **Admin** (Administrador):
   
    ![Seleccionar Admin (Administrador)][12]

3. Seleccione **Configure** (Configurar):
   
    ![Seleccionar Configure (Configurar)][13]

4. En el cuadro de diálogo **Security** (Seguridad), siga los pasos que se indican a continuación.
   
    ![Cuadro de diálogo Security (Seguridad)][14]
    
    1. Pegue el valor de **Dirección URL de inicio de sesión**  que copió de Azure Portal en el cuadro **SSO-Login URL** (URL de inicio de sesión único).
    
    1. En la lista **SSO - Single Sign-on Mode** (Modo Inicio de sesión único), seleccione **Optional** (Opcional). Seleccione **Guardar**.

       > [!NOTE]
       > El modo opcional es solo para pruebas. Una vez que esté satisfecho con la configuración, seleccione **Required** (Re requiere) en la lista **SSO - Single Sign-on Mode** (Modo Inicio de sesión único) para forzar a todos los usuarios a autenticarse en Azure AD.

    1. En el Bloc de notas, abra el certificado que descargó en la sección anterior. Copie el contenido del certificado sin la primera línea ( **---BEGIN CERTIFICATE---** ) o la última línea ( **---END CERTIFICATE---** ). Pegue el contenido del certificado en el cuadro **SSO-certificado x.509** y, a continuación, seleccione **Save** (Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección creará un usuario de prueba llamado Britta Simon en Azure Portal.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**:

    ![Selección de Todos los usuarios](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Selección de Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga los pasos que se indican a continuación.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.
  
    1. En el cuadro **Nombre de usuario**, escriba **BrittaSimon@\<dominioDeSuEmpresa>.\<extensión>** . (Por ejemplo, BrittaSimon@contoso.com).

    1. Seleccione **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    1. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure, para lo que le concederá acceso a Promapp.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales**, **Todas las aplicaciones** y **Promapp**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Promapp**.

    ![Lista de aplicaciones](common/all-applications.png)

3. En el panel izquierdo, seleccione **Usuarios y grupos**:

    ![Seleccionar Usuarios y grupos](common/users-groups-blade.png)

4. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Selección de Agregar usuario](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. Haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="just-in-time-user-provisioning"></a>Aprovisionamiento de usuarios Just-In-Time

Promapp admite el aprovisionamiento de usuarios Just-In-Time. Esta característica está habilitada de forma predeterminada. Si un usuario deja de existir en Promapp, se crea otro después de la autenticación.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Ahora, debe probar la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Promapp en el Panel de acceso, debería iniciar sesión automáticamente en la instancia de Promapp para la que configuró el inicio de sesión único. Para más información acerca del Panel de acceso, consulte [Access and use apps on the My Apps portal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) (Uso de aplicaciones y acceso a ellas en el portal Aplicaciones).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutoriales para integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[12]: ./media/promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/promapp-tutorial/tutorial_promapp_07.png
