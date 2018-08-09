---
title: 'Tutorial: Integración de Azure Active Directory con Vidyard | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Vidyard.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bed7df23-6e13-4e7c-b4cc-53ed4804664d
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2018
ms.author: jeedes
ms.openlocfilehash: 871942db15d6a3cff45584e33b2191e21d2281a0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426462"
---
# <a name="tutorial-azure-active-directory-integration-with-vidyard"></a>Tutorial: Integración de Azure Active Directory con Vidyard

En este tutorial, obtendrá información sobre cómo integrar Vidyard con Azure Active Directory (Azure AD).

La integración de Vidyard con Azure AD proporciona las ventajas siguientes:

- Puede controlar en Azure AD quién tiene acceso a Vidyard.
- Puede permitir que los usuarios inicien sesión automáticamente en Vidyard (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Vidyard, necesita los elementos siguientes:

- Una suscripción de Azure AD
- Una suscripción de Vidyard habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Vidyard desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-vidyard-from-the-gallery"></a>Adición de Vidyard desde la galería
Para configurar la integración de Vidyard en Azure AD, tendrá que agregar Vidyard desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Vidyard desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Vidyard**, seleccione **Vidyard** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Vidyard en la lista de resultados](./media/vidyard-tutorial/tutorial_vidyard_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Vidyard con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Vidyard para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Vidyard.

Para configurar y probar el inicio de sesión único de Azure AD con Vidyard, es preciso completar los bloques de creación siguientes:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Vidyard](#create-a-vidyard-test-user)**: para tener un homólogo de Britta Simon en Vidyard que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Vidyard.

**Para configurar el inicio de sesión único de Azure AD con Vidyard, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Vidyard**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/vidyard-tutorial/tutorial_vidyard_samlbase.png)

1. En la sección **Dominio y direcciones URL de Vidyard**, siga los pasos siguientes si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Vidyard](./media/vidyard-tutorial/tutorial_vidyard_url2.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://secure.vidyard.com/sso/saml/<unique id>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://secure.vidyard.com/sso/saml/<unique id>/consume`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Vidyard](./media/vidyard-tutorial/tutorial_vidyard_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://secure.vidyard.com/sso/saml/<unique id>/login`.

    > [!NOTE]
    > Estos valores no son reales. Los valores se actualizarán con la dirección URL de inicio de sesión, la dirección URL de respuesta y el identificador reales, que se explican más adelante en el tutorial.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/vidyard-tutorial/tutorial_vidyard_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/vidyard-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Vidyard**, haga clic en **Configurar Vidyard** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configuración de Vidyard](./media/vidyard-tutorial/tutorial_vidyard_configure.png)

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de empresa de Vidyard Software.

1. En el panel Vidyard, seleccione **Grupo** > **Seguridad**

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure1.png)

1. Haga clic en la pestaña **Nuevo perfil**.

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure2.png)

1. En la sección **Configuración de SAML**, siga estos pasos:

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure3.png)

    a. Escriba el nombre de perfil general en el cuadro de texto **Nombre de perfil**.

    b. Copie el valor de **SSO User Login Page** (Página de inicio de sesión de usuario de SSO) y péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **Dominio y direcciones URL de Vidyard** de Azure Portal.

    c. Copie el valor de **URL de ACS** y péguelo en el cuadro de texto **URL de respuesta** de la sección **Dominio y direcciones URL de Vidyard** de Azure Portal.

    d. Copie el valor de **URL de metadatos del emisor** y péguelo en el cuadro de texto **Identificador** de la sección **Dominio y direcciones URL de Vidyard** de Azure Portal.

    e. Abra el certificado descargado de Azure Portal en el Bloc de notas y luego péguelo en el cuadro de texto **Certificado X.509**.

    f. En el cuadro de texto **Dirección URL del punto de conexión de SAML**, pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    g. Haga clic en **Confirmar**.

1. En la pestaña Inicio de sesión único, haga clic en **Asignar** junto a un perfil existente.

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure4.png)

    > [!NOTE]
    > Una vez que haya creado un perfil de SSO, asígnelo a los grupos para los que los usuarios necesitarán acceso a través de Azure. Si el usuario no existe en el grupo al que se asignó, Vidyard creará automáticamente una cuenta de usuario y asignará su rol en tiempo real.

1. Seleccione el grupo de la organización, que es visible en **Groups Available to Assign** (Grupos disponibles para asignar).

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure5.png)

1. Puede ver los grupos asignados en **Groups Currently Assigned** (Grupos asignados actualmente). Seleccione un rol para el grupo de acuerdo con la organización y haga clic en **Confirmar**.

    ![Configuración de Vidyard](./media/vidyard-tutorial/configure6.png)

    > [!NOTE]
    > Para obtener más información, consulte [este documento](https://knowledge.vidyard.com/saml-single-sign-on-authentication/saml-based-single-sign-on-sso-in-vidyard).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/vidyard-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/vidyard-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/vidyard-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/vidyard-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-vidyard-test-user"></a>Creación de un usuario de prueba de Vidyard

El objetivo de esta sección es crear un usuario llamado Britta Simon en Vidyard. Vidyard admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Vidyard, se crea un usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Vidyard](mailto:support@vidyard.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Vidyard.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Vidyard, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Vidyard**.

    ![Vínculo a Vidyard en la lista de aplicaciones](./media/vidyard-tutorial/tutorial_vidyard_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Vidyard en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Vidyard.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/vidyard-tutorial/tutorial_general_01.png
[2]: ./media/vidyard-tutorial/tutorial_general_02.png
[3]: ./media/vidyard-tutorial/tutorial_general_03.png
[4]: ./media/vidyard-tutorial/tutorial_general_04.png

[100]: ./media/vidyard-tutorial/tutorial_general_100.png

[200]: ./media/vidyard-tutorial/tutorial_general_200.png
[201]: ./media/vidyard-tutorial/tutorial_general_201.png
[202]: ./media/vidyard-tutorial/tutorial_general_202.png
[203]: ./media/vidyard-tutorial/tutorial_general_203.png

