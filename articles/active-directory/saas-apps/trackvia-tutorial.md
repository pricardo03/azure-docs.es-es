---
title: 'Tutorial: Integración de Azure Active Directory con TrackVia | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TrackVia.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e7010023-bdda-4a19-a335-19904e75b813
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6e4c90b6f9fd8b968ceb0e241649ddbcf1c2e1cb
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189129"
---
# <a name="tutorial-azure-active-directory-integration-with-trackvia"></a>Tutorial: Integración de Azure Active Directory con TrackVia

En este tutorial, aprenderá a integrar TrackVia con Azure Active Directory (Azure AD).

Integrar TrackVia con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a TrackVia.
- Puede permitir que los usuarios inicien sesión automáticamente en TrackVia (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con TrackVia, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en TrackVia

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de TrackVia desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-trackvia-from-the-gallery"></a>Adición de TrackVia desde la galería
Para configurar la integración de TrackVia en Azure AD, deberá agregar TrackVia desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar TrackVia desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **TrackVia**, seleccione **TrackVia** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![TrackVia en la lista de resultados](./media/trackvia-tutorial/tutorial_trackvia_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con TrackVia con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de TrackVia para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de TrackVia.

Para establecer la relación de vínculo, en TrackVia, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con TrackVia, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de TrackVia](#create-a-trackvia-test-user)**: para tener un homólogo de Britta Simon en TrackVia que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación TrackVia.

**Para configurar el inicio de sesión único de Azure AD con TrackVia, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **TrackVia**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/trackvia-tutorial/tutorial_trackvia_samlbase.png)

1. En la sección **Dominio y direcciones URL de TrackVia**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de TrackVia](./media/trackvia-tutorial/tutorial_trackvia_url.png)

    En el cuadro de texto **Identificador**, escriba el valor `TrackVia`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de TrackVia](./media/trackvia-tutorial/tutorial_trackvia_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://companyname.trackvia.com`.
     
    > [!NOTE] 
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico al cliente de TrackVia](mailto:support@trackvia.com) para obtener este valor.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/trackvia-tutorial/tutorial_trackvia_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/trackvia-tutorial/tutorial_general_400.png)

1. En la sección **TrackVia Configuration** (Configuración de TrackVia), haga clic en **Configure TrackVia** (Configurar TrackVia) para abrir la ventana **Configurar inicio de sesión**. Copie el valor de **Identificador de entidad de SAML** de la **sección Referencia rápida**

    ![Configuración de TrackVia](./media/trackvia-tutorial/tutorial_trackvia_configure.png)
    
1. En otra ventana del explorador, inicie sesión en el sitio de la compañía de TrackVia como administrador.

1. Haga clic en la configuración **My Account** (Mi cuenta) de Trackvia y seleccione la pestaña **Single Sign On** (Inicio de sesión único), después, realice los pasos siguientes:

    ![Configuración de TrackVia](./media/trackvia-tutorial/configure1.png)

     a. En el cuadro de texto **Identity Provider Entity ID** (Identificador de entidad del proveedor de identidad), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal.

    b. Seleccione **Choose File** (Elegir archivo) para cargar el archivo de metadatos que descargó de Azure Portal.

    c. Haga clic en **Guardar**

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/trackvia-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/trackvia-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/trackvia-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/trackvia-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-trackvia-test-user"></a>Crear un usuario de prueba de TrackVia

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en TrackVia. TrackVia admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a TrackVia, se crea un nuevo usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de TrackVia](mailto:support@trackvia.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a TrackVia.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon en TrackVia, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **TrackVia**.

    ![Vínculo a TrackVia en la lista de aplicaciones](./media/trackvia-tutorial/tutorial_trackvia_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TrackVia en el Panel de acceso, se debería iniciar sesión automáticamente en su aplicación TrackVia.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/trackvia-tutorial/tutorial_general_01.png
[2]: ./media/trackvia-tutorial/tutorial_general_02.png
[3]: ./media/trackvia-tutorial/tutorial_general_03.png
[4]: ./media/trackvia-tutorial/tutorial_general_04.png

[100]: ./media/trackvia-tutorial/tutorial_general_100.png

[200]: ./media/trackvia-tutorial/tutorial_general_200.png
[201]: ./media/trackvia-tutorial/tutorial_general_201.png
[202]: ./media/trackvia-tutorial/tutorial_general_202.png
[203]: ./media/trackvia-tutorial/tutorial_general_203.png
