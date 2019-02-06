---
title: 'Tutorial: Integración de Azure Active Directory con Settling music | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Settling music.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6f86a8a2-4bd0-40cc-b1b4-752fce123328
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: jeedes
ms.openlocfilehash: 900254e42410aafa0d8e58048c7d88efa94dfed5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55178552"
---
# <a name="tutorial-azure-active-directory-integration-with-settling-music"></a>Tutorial: Integración de Azure Active Directory con Settling music

En este tutorial, aprenderá a integrar Settling music con Azure Active Directory (Azure AD).

La integración de Settling music con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Settling music.
- Puede permitir que los usuarios inicien sesión automáticamente en Settling music (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Settling music, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Settling music

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Settling music desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-settling-music-from-the-gallery"></a>Adición de Settling music desde la galería
Para configurar la integración de Settling music en Azure AD, deberá agregar Settling music desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Settling music desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Settling music**, seleccione **Settling music** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Settling music en la lista de resultados](./media/settlingmusic-tutorial/tutorial_settlingmusic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Settling music con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Settling music para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Settling music.

Para configurar y probar el inicio de sesión único de Azure AD con Settling music, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba en Settling music](#create-a-settling-music-test-user)**: para tener un homólogo de Britta Simon en Settling music que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Settling music.

**Para configurar el inicio de sesión único de Azure AD con Settling music, siga estos pasos:**

1. En la página de integración de la aplicación **Settling music** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/settlingmusic-tutorial/tutorial_settlingmusic_samlbase.png)

1. En la sección **Settling music Domain and URLs** (Dominio y direcciones URL de Settling music), lleve a cabo los pasos siguientes:

    ![Información de inicio de sesión único de dominio y direcciones URL de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte al cliente de Settling music](https://rakurakuseisan.jp/) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/settlingmusic-tutorial/tutorial_settlingmusic_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/settlingmusic-tutorial/tutorial_general_400.png)

1. En la sección **Settling music Configuration** (Configuración de Settling music), haga clic en **Configure Settling music** (Configurar Settling music) para abrir la ventana **Configure sign-on** (Configurar inicio de sesión). Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_configure.png) 

1. En otra ventana del explorador web, inicie sesión en Settling music como administrador de seguridad.

1. En la parte superior de la página, haga clic en la pestaña **Administración**.

    ![Paso 1 de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_step1.png)

1. Haga clic en la pestaña **Configuración del sistema**.

    ![Paso 2 de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_step2.png)

1. Cambie la pestaña **Seguridad**.

    ![Paso 3 de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_step3.png)

1. En la sección **Single Sign-on Settings** (Configuración del inicio de sesión único), siga estos pasos:

    ![Paso 5 de Settling music](./media/settlingmusic-tutorial/tutorial_settlingmusic_step4.png)

     a. Haga clic **To enable** (Para habilitar).

    b. En el cuadro de texto **ID Provider Login URL**  (Dirección URL de inicio de sesión del proveedor de identidades), pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que copió de Azure Portal.

    c. En el cuadro de texto **ID Provider Logout URL** (Dirección URL de cierre de sesión del proveedor de identidades), pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal.

    d. Haga clic en **Choose File** (Elegir archivo) para cargar el **Certificado (Base64)** que ha descargado de Azure Portal.

    e. Haga clic en el botón **Save** (Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/settlingmusic-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/settlingmusic-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/settlingmusic-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/settlingmusic-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-settling-music-test-user"></a>Creación de un usuario de prueba de Settling music

En esta sección, creará un usuario llamado Britta Simon en Settling music. Trabaje con el  [equipo de soporte técnico al cliente de Settling music](https://rakurakuseisan.jp/) para agregar los usuarios a la plataforma de Settling music. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Settling music para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Settling music, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Settling music**.

    ![Vínculo a Settling music en la lista de aplicaciones](./media/settlingmusic-tutorial/tutorial_settlingmusic_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Settling music en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Settling music.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/settlingmusic-tutorial/tutorial_general_01.png
[2]: ./media/settlingmusic-tutorial/tutorial_general_02.png
[3]: ./media/settlingmusic-tutorial/tutorial_general_03.png
[4]: ./media/settlingmusic-tutorial/tutorial_general_04.png

[100]: ./media/settlingmusic-tutorial/tutorial_general_100.png

[200]: ./media/settlingmusic-tutorial/tutorial_general_200.png
[201]: ./media/settlingmusic-tutorial/tutorial_general_201.png
[202]: ./media/settlingmusic-tutorial/tutorial_general_202.png
[203]: ./media/settlingmusic-tutorial/tutorial_general_203.png

