---
title: 'Tutorial: Integración de Azure Active Directory con Zoho One | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Zoho One.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bbc3038c-0d8b-45dd-9645-368bd3d01a0f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f17a297d3099d51b3a58a6654560a67f9a4192a
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56208323"
---
# <a name="tutorial-azure-active-directory-integration-with-zoho-one"></a>Tutorial: Integración de Azure Active Directory con Zoho One

En este tutorial se aprende a integrar Zoho One con Azure Active Directory (Azure AD).

La integración de Zoho One con Azure AD proporciona las siguientes ventajas:

- En Azure AD, puede controlar quién tiene acceso a Zoho One.
- Puede permitir que los usuarios inicien sesión automáticamente en Zoho One (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Zoho One, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Zoho One

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Zoho One desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-zoho-one-from-the-gallery"></a>Incorporación de Zoho One desde la galería
Para configurar la integración de Zoho One en Azure AD, deberá agregar Zoho One desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Zoho One desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Zoho One**, seleccione **Zoho One** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Zoho One en la lista de resultados](./media/zohoone-tutorial/tutorial_zohoone_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con Zoho One con un usuario de prueba de nombre "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Zoho One para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Zoho One.

Para configurar y probar el inicio de sesión único de Azure AD con Zoho One, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Zoho One](#create-a-zoho-one-test-user)**, para tener un homólogo de Britta Simon en Zoho One que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección se habilita el inicio de sesión único de Azure AD en Azure Portal y se configura en la aplicación Zoho One.

**Para configurar el inicio de sesión único de Azure AD con Zoho One, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Zoho One**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/zohoone-tutorial/tutorial_zohoone_samlbase.png)

1. En la sección **Dominio y direcciones URL de Zoho One**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de inicio de sesión único de Dominio y direcciones URL de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_url.png)

     a. En el cuadro de texto **Identificador (Id. de entidad)**, escriba una dirección URL: `one.zoho.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://accounts.zoho.com/samlresponse/<saml-identifier>`.

    c. Active **Mostrar configuración avanzada de URL**.

    d. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `https://one.zoho.com`

1. Si quiere configurar la aplicación en modo iniciado por **SP**, realice el siguiente paso:

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://accounts.zoho.com/samlauthrequest/<domain_name>?serviceurl=https://one.zoho.com`.
     
    > [!NOTE] 
    > El valor de **URL de respuesta** y **URL de inicio de sesión** no es real. El valor se actualiza con la dirección URL de inicio de sesión y de respuesta reales, lo que se explica más adelante en el tutorial. 

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/zohoone-tutorial/tutorial_zohoone_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/zohoone-tutorial/tutorial_general_400.png)
    
1. En la sección **Configuración de Zoho One**, haga clic en **Configurar Zoho One** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de Zoho One como administrador.

1. En la pestaña **Organización**, haga clic en **Configuración** en **Autenticación SAML**.

    ![Org. Zoho One](./media/zohoone-tutorial/tutorial_zohoone_setup.png)

1. En la página emergente, realice los pasos siguientes:

    ![Ini. Zoho One](./media/zohoone-tutorial/tutorial_zohoone_save.png)

     a. Copie el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal en el cuadro de texto **URL de inicio de sesión**.

    b. En el cuadro de texto **Dirección URL de cierre de sesión**, pegue el valor de **Dirección URL de cierre de sesión** que ha copiado de Azure Portal.

    c. Haga clic en **Examinar** para cargar el **Certificado (Base64)** que ha descargado de Azure Portal.

    d. Haga clic en **Save**(Guardar).

1. Después de guardar la configuración de autenticación SAML, copie el valor **Identificador SAML** y use este valor en **URL de respuesta** en Azure Portal, en la sección **Dominio y direcciones URL de Zoho One**.

    ![SAML de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_samlidenti.png)

1. Vaya a la pestaña **Dominios** y haga clic en **Agregar dominio**.

    ![Dominio de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_domain.png)

1. En la página **Agregar dominio**, siga estos pasos:

    ![Agregar dominio de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_adddomain.png)

     a. En el cuadro de texto **Nombre de dominio**, escriba el dominio, por ejemplo **contoso.com**.

    b. Haga clic en **Agregar**.

    >[!Note]
    >Después de agregar el dominio, siga [estos pasos](https://www.zoho.com/one/help/admin-guide/domain-verification.html) para comprobar el dominio. Una vez verificado el dominio, use el nombre de dominio en **URL de inicio de sesión**, en la sección **Dominio y direcciones URL de Zoho One** de Azure Portal.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/zohoone-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/zohoone-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/zohoone-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/zohoone-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-zoho-one-test-user"></a>Creación de un usuario de prueba de Zoho One

Para permitir que los usuarios de Azure AD inicien sesión en Zoho One, deben aprovisionarse en Zoho One. En Zoho One, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en Zoho One como administrador de seguridad.

1. En la pestaña **Usuarios**, haga clic en **Logotipo de usuario**.

    ![Usuario de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_users.png)

1. En la página **Add User** (Agregar usuario), siga estos pasos:

    ![Agregar usuario de Zoho One](./media/zohoone-tutorial/tutorial_zohoone_adduser.png)
    
     a. En el cuadro de texto **Nombre**, escriba el nombre de un usuario, por ejemplo, **Britta Simon**.
    
    b. En el cuadro de texto **Email Address** (Dirección de correo electrónico), escriba el correo electrónico del usuario, por ejemplo, **brittasimon@contoso.com**.

    >[!Note]
    >Seleccione el dominio verificado en la lista de dominios.

    c. Haga clic en **Agregar**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección se habilita a Britta Simon para que use el inicio de sesión único de Azure al concederle acceso a Zoho One.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Zoho One, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Zoho One**.

    ![Vínculo de Zoho One en la lista de aplicaciones](./media/zohoone-tutorial/tutorial_zohoone_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Zoho One en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Zoho One.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/zohoone-tutorial/tutorial_general_01.png
[2]: ./media/zohoone-tutorial/tutorial_general_02.png
[3]: ./media/zohoone-tutorial/tutorial_general_03.png
[4]: ./media/zohoone-tutorial/tutorial_general_04.png

[100]: ./media/zohoone-tutorial/tutorial_general_100.png

[200]: ./media/zohoone-tutorial/tutorial_general_200.png
[201]: ./media/zohoone-tutorial/tutorial_general_201.png
[202]: ./media/zohoone-tutorial/tutorial_general_202.png
[203]: ./media/zohoone-tutorial/tutorial_general_203.png

