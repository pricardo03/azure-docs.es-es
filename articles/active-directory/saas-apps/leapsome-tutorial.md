---
title: 'Tutorial: Integración de Azure Active Directory con Leapsome | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Leapsome.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cb523e97-add8-4289-b106-927bf1a02188
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: jeedes
ms.openlocfilehash: 898d7cf6cdded08cd09c4b1f1f845473af1650a3
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824016"
---
# <a name="tutorial-azure-active-directory-integration-with-leapsome"></a>Tutorial: Integración de Azure Active Directory con Leapsome

En este tutorial, obtendrá información sobre cómo integrar Leapsome con Azure Active Directory (Azure AD).

La integración de Leapsome con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Leapsome.
- Puede permitir que los usuarios inicien sesión automáticamente en Leapsome (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Leapsome, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de Leapsome habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Leapsome desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-leapsome-from-the-gallery"></a>Incorporación de Leapsome desde la galería
Para configurar la integración de Leapsome en Azure AD, deberá agregar Leapsome desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Leapsome desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Leapsome**, seleccione **Leapsome** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Leapsome en la lista de resultados](./media/leapsome-tutorial/tutorial_leapsome_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Leapsome con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Leapsome para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Leapsome.

Para configurar y probar el inicio de sesión único de Azure AD con Leapsome, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Leapsome](#create-a-leapsome-test-user)**: para tener un homólogo de Britta Simon en Leapsome que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Leapsome.

**Para configurar el inicio de sesión único de Azure AD con Leapsome, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Leapsome**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/leapsome-tutorial/tutorial_leapsome_samlbase.png)

1. En la sección **Dominio y direcciones URL de Leapsome**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL como: `https://www.leapsome.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/assert`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://www.leapsome.com/api/users/auth/saml/<CLIENTID>/login`.
     
    > [!NOTE] 
    > El valor de URL de respuesta y URL de inicio de sesión no es un valor real. Estos se actualizarán con los valores reales, lo que se explica más adelante en el tutorial.

1. La aplicación Leapsome espera las instrucciones de aserción de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla muestra un ejemplo.
    
    ![Configurar inicio de sesión único](./media/leapsome-tutorial/tutorial_Leapsome_attribute.png)

1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo | Espacio de nombres |
    | ---------------| --------------- | --------- |   
    | firstname | user.givenname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastname | user.surname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | título | user.jobtitle | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | picture | URL de una fotografía del empleado | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

    > [!Note]
    > El valor del atributo picture no es real. Actualice este valor con la URL de la imagen real. Para obtener este valor, póngase en contacto con el  [equipo de soporte técnico de Fuse](mailto:support@leapsome.com).
    
     a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/leapsome-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/leapsome-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. En el cuadro de texto **Espacio de nombres**, escriba el URI de espacio de nombres para esa fila.
    
    e. Haga clic en **Aceptar**.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/leapsome-tutorial/tutorial_leapsome_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/leapsome-tutorial/tutorial_general_400.png)
    
1. En la sección **Configuración de Leapsome**, haga clic en **Configurar Leapsome** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configuración de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_configure.png)

1. En otra ventana del explorador web, inicie sesión en Leapsome como administrador de seguridad.

1. En la parte superior derecha, haga clic en el logotipo de configuración y luego en **Configuración de administración**. 

    ![Conjunto de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_admin.png)

1. En la barra de menús izquierda, haga clic en **Inicio de sesión único (SSO)** y en la página **Inicio de sesión único basado en SAML (SSO)** siga estos pasos:
    
    ![SAML de Leapsome](./media/leapsome-tutorial/tutorial_leapsome_samlsettings.png)

     a. Seleccione **Enable SAML-based single sign-on** (Habilitar inicio de sesión único basado en SAML).

    b. Copie el valor **Login URL (point your users here to start login)** [URL de inicio de sesión (apuntar a los usuarios aquí para iniciar el inicio de sesión)] y péguelo en el cuadro de texto **URL de inicio de sesión** en la sección **Dominio y direcciones URL de Leapsome**  de Azure Portal.

    c. Copie el valor **Reply URL (receives response from your identity provider)** [URL de respuesta (recibe respuesta desde el proveedor de identidades)] y péguelo en el cuadro de texto **URL de respuesta** en la sección **Dominio y direcciones URL de Leapsome** de Azure Portal.

    d. En el cuadro de texto **SSO Login URL (provided by identity provider)** [Dirección URL de inicio de sesión único (proporcionada por el proveedor de identidades)], pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML), que ha copiado de Azure Portal.

    e. Copie el certificado que ha descargado desde Azure Portal sin los comentarios --BEGIN CERTIFICATE y END CERTIFICATE-- y péguelo en el cuadro de texto **Certificate (provided by identity provider)** [Certificado (proporcionado por el proveedor de identidades)].

    f. Haga clic en **UPDATE SSO SETTINGS** (ACTUALIZAR CONFIGURACIÓN DE SSO).
    
### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/leapsome-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/leapsome-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/leapsome-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/leapsome-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-leapsome-test-user"></a>Creación de un usuario de prueba de Leapsome

En esta sección, creará un usuario llamado Britta Simon en Leapsome. Colabore con el  [equipo de soporte técnico de Leapsome](mailto:support@leapsome.com)  para agregar los usuarios o el dominio que debe incluirse en la lista blanca en la plataforma de Leapsome. Si el dominio lo agrega el equipo, los usuarios se aprovisionarán automáticamente en la plataforma de Leapsome. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único. 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure mediante la concesión de acceso a Leapsome.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Leapsome, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Leapsome**.

    ![Vínculo a Leapsome en la lista de aplicaciones](./media/leapsome-tutorial/tutorial_leapsome_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Leapsome en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Leapsome.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/leapsome-tutorial/tutorial_general_01.png
[2]: ./media/leapsome-tutorial/tutorial_general_02.png
[3]: ./media/leapsome-tutorial/tutorial_general_03.png
[4]: ./media/leapsome-tutorial/tutorial_general_04.png

[100]: ./media/leapsome-tutorial/tutorial_general_100.png

[200]: ./media/leapsome-tutorial/tutorial_general_200.png
[201]: ./media/leapsome-tutorial/tutorial_general_201.png
[202]: ./media/leapsome-tutorial/tutorial_general_202.png
[203]: ./media/leapsome-tutorial/tutorial_general_203.png

