---
title: 'Tutorial: Integración de Azure Active Directory con Menlo Security | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Menlo Security.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: fb4a82d1ae34c81df535e606a39eaf091f7df0da
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55158067"
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a>Tutorial: Integración de Azure Active Directory con Menlo Security

En este tutorial, aprenderá a integrar Menlo Security con Azure Active Directory (Azure AD).

Integrar Menlo Security con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Menlo Security.
- Puede permitir que los usuarios inicien sesión automáticamente en Menlo Security (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si quiere conocer más detalles sobre la integración de aplicaciones SaaS con Azure AD, vea: [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Menlo Security, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Menlo Security

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Menlo Security desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-menlo-security-from-the-gallery"></a>Adición de Menlo Security desde la galería
Para configurar la integración de Menlo Security en Azure AD, debe agregar Menlo Security desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Menlo Security desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Menlo Security**.

    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/tutorial_menlosecurity_search.png)

1. En el panel de resultados, seleccione **Menlo Security** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Menlo Security con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Menlo Security para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de Menlo Security.

Para establecer esta relación de vínculo, se toma el valor del **nombre de usuario** en Azure AD y se asigna como valor del **nombre de usuario** en Menlo Security.

Para configurar y probar el inicio de sesión único de Azure AD con Menlo Security, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Menlo Security](#creating-a-menlo-security-test-user)**: para tener un homólogo de Britta Simon en Menlo Security que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Menlo Security.

**Para configurar el inicio de sesión único de Azure AD con Menlo Security, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Menlo Security**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

1. En la sección **Dominio y direcciones URL de Menlo Security**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_menlosecurity_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.menlosecurity.com/account/login`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Menlo Security](https://www.menlosecurity.com/menlo-contact) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Menlo Security**, haga clic en **Configurar Menlo Security** para abrir la ventana **Configurar inicio de sesión**. Copie el **Identificador de entidad en SAML** y la **Dirección URL de inicio de sesión único de SAML** de la **sección Referencia rápida.**

    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

1. Para configurar el inicio de sesión único en **Menlo Security**, regístrese en el sitio web de **Menlo Security** como administrador.

1. En **Settings** (Configuración), vaya a **Authentication** (Autenticación) y realice las siguientes acciones:
    
    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/menlo_user_setup.png)

     a. Active la casilla **Enable user authentication using SAML** (Habilitar la autenticación de usuario mediante SAML).

    b. Seleccione **Allow External Access** (Permitir acceso externo) en **Yes** (Sí).

    c. En **SAML Provider** (Proveedor de SAML), seleccione **Azure Active Directory**.

    d. **SAML 2.0 Endpoint** (Punto de conexión de SAML 2.0): pegue la **Dirección URL del servicio de inicio de sesión único de SAML** que copió de Azure Portal.

    e. **Service Identifier (Issuer)** (Identificador del servicio [emisor]): pegue el **Identificador de entidad de SAML** que copió de Azure Portal.

    f. **X.509 Certificate** (Certificado X.509): abra el **Certificado (Base64)** descargado de Azure Portal en el Bloc de notas y péguelo en este cuadro.

    g. Haga clic en **Guardar** para guardar la configuración.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/menlosecurity-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-menlo-security-test-user"></a>Creación de un usuario de prueba de Menlo Security
 
En esta sección, creará un usuario llamado Britta Simon en Menlo Security. Colabore con el  [equipo de soporte al cliente de Menlo Security](https://www.menlosecurity.com/menlo-contact) para agregar los usuarios a la plataforma de Menlo Security. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Menlo Security.

![Asignar usuario][200] 

**Para asignar Britta Simon a Menlo Security, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Menlo Security**.

    ![Configurar inicio de sesión único](./media/menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD.

Abra una ventana del explorador en modo "InPrivate" o "Incognito" para desencadenar una nueva autenticación.  En Internet Explorer, utilice Ctrl + Mayús + P.  En Chrome, utilice Ctrl + Mayús + N.  En la ventana de exploración privada, vaya a un recurso protegido e inicie sesión en Azure AD.  Cuando inicie sesión correctamente, se le llevará al sitio solicitado en una sesión de aislamiento.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/menlosecurity-tutorial/tutorial_general_203.png

