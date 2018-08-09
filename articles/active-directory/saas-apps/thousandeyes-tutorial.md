---
title: 'Tutorial: integración de Azure Active Directory con ThousandEyes | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ThousandEyes.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: jeedes
ms.openlocfilehash: b6af7a3322b1a01c1d822df78d827121c19e21e1
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39444409"
---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Tutorial: integración de Azure Active Directory con ThousandEyes

En este tutorial, obtendrá información sobre cómo integrar ThousandEyes con Azure Active Directory (Azure AD).

Integrar ThousandEyes con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a ThousandEyes
- Puede permitir que los usuarios inicien sesión automáticamente en ThousandEyes (Inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ThousandEyes, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en ThousandEyes

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, aquí puede obtener una versión de evaluación de un mes: [Oferta de prueba](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar ThousandEyes desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-thousandeyes-from-the-gallery"></a>Agregar ThousandEyes desde la galería
Para configurar la integración de ThousandEyes en Azure AD, será preciso que agregue ThousandEyes desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ThousandEyes desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqued, escriba **ThousandEyes**.

    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/tutorial_thousandeyes_search.png)

1. En el panel de resultados, seleccione **ThousandEyes** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/tutorial_thousandeyes_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con ThousandEyes con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ThousandEyes para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ThousandEyes.

Para establecer la relación de vínculo, en ThousandEyes, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con ThousandEyes, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de ThousandEyes](#creating-a-thousandeyes-test-user)**: para tener un homólogo de Britta Simon en Trakstar vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación ThousandEyes.

**Para configurar el inicio de sesión único de Azure AD con ThousandEyes, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **ThousandEyes**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_thousandeyes_samlbase.png)

1. En la sección **Dominio y direcciones URL de ThousandEyes**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_thousandeyes_url.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `https://app.thousandeyes.com/login/sso`

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_thousandeyes_certificate.png)

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de ThousandEyes**, haga clic en **Configurar ThousandEyes** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_thousandeyes_configure.png) 

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de **ThousandEyes** como administrador.

1. En el menú de la parte superior, haga clic en **Configuración**.

    ![Configuración](./media/thousandeyes-tutorial/ic790066.png "Configuración")

1. Haga clic en **Cuenta**

    ![Cuenta](./media/thousandeyes-tutorial/ic790067.png "Cuenta")

1. Haga clic en la pestaña **Security & Authentication** (Seguridad y autenticación).

    ![Seguridad y autenticación](./media/thousandeyes-tutorial/ic790068.png "Seguridad y autenticación")

1. En la sección **Configurar inicio de sesión único** siga los pasos siguientes:

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/ic790069.png "Configurar inicio de sesión único")

    a. Seleccione **Enable Single Sign-On**(Habilitar inicio de sesión único).

    b. En el cuadro de texto **URL de la página de inicio de sesión**, pegue la **URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    c. En el cuadro de texto **URL de la página de cierre de sesión**, copie el valor de **URL de cierre de sesión** que ha copiado de Azure Portal.

    d. En el cuadro de texto **Emisor de proveedor de identidades**, pegue el valor de **id. de entidad de SAML** que ha copiado de Azure Portal.

    e. En **Certificado de verificación**, haga clic en **Elegir archivo** y cargue el certificado que ha descargado de Azure Portal.

    f. Haga clic en **Save**(Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.

    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/create_aaduser_03.png)

1. En la página de diálogo **Usuario**, realice los siguientes pasos:

    ![Creación de un usuario de prueba de Azure AD](./media/thousandeyes-tutorial/create_aaduser_04.png)

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="creating-a-thousandeyes-test-user"></a>Creación de un usuario de prueba de ThousandEyes

El objetivo de esta sección es crear un usuario llamado Britta Simon en ThousandEyes. ThousandEyes admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](thousandeyes-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

**Para crear un usuario manualmente, siga los pasos siguientes:**

1. Inicie sesión en su sitio de la compañía de ThousandEyes como administrador.

1. Haga clic en **Configuración**.

    ![Configuración](./media/thousandeyes-tutorial/IC790066.png "Configuración")

1. Haga clic en **Cuenta**.

    ![Cuenta](./media/thousandeyes-tutorial/IC790067.png "Cuenta")

1. Haga clic en la pestaña **Accounts & Users** (Cuentas y usuarios).

    ![Cuentas y usuarios](./media/thousandeyes-tutorial/IC790073.png "Cuentas y usuarios")

1. En la sección **Add Users & Accounts** (Agregar usuarios y cuentas), realice los siguientes pasos:

    ![Agregar cuentas de usuario](./media/thousandeyes-tutorial/IC790074.png "Agregar cuentas de usuario")

    a. En el cuadro de texto **Nombre**, escriba el nombre de un usuario, por ejemplo, **Britta Simon**.

    b. En el cuadro de texto **Correo electrónico**, escriba el correo electrónico del usuario, en el ejemplo **brittasimon@contoso.com**.

    b. Haga clic en **Agregar nuevo usuario a la cuenta**.

    > [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo electrónico con un vínculo para confirmar la cuenta y activarla.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de ThousandEyes ofrecida por ThousandEyes para aprovisionar cuentas de usuario de Azure Active Directory.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a ThousandEyes para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar Britta Simon a ThousandEyes, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **ThousandEyes**.

    ![Configurar inicio de sesión único](./media/thousandeyes-tutorial/tutorial_thousandeyes_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ThousandEyes en el panel de acceso, debe iniciar sesión automáticamente en su aplicación ThousandEyes.

Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del aprovisionamiento de usuarios](thousandeyes-provisioning-tutorial.md)


<!--Image references-->

[1]: ./media/thousandeyes-tutorial/tutorial_general_01.png
[2]: ./media/thousandeyes-tutorial/tutorial_general_02.png
[3]: ./media/thousandeyes-tutorial/tutorial_general_03.png
[4]: ./media/thousandeyes-tutorial/tutorial_general_04.png

[100]: ./media/thousandeyes-tutorial/tutorial_general_100.png

[200]: ./media/thousandeyes-tutorial/tutorial_general_200.png
[201]: ./media/thousandeyes-tutorial/tutorial_general_201.png
[202]: ./media/thousandeyes-tutorial/tutorial_general_202.png
[203]: ./media/thousandeyes-tutorial/tutorial_general_203.png
