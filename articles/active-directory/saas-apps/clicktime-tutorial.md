---
title: 'Tutorial: Integración de Azure Active Directory con ClickTime | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y ClickTime.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: joflore
ms.assetid: d437b5ab-4d71-4c13-96d0-79018cebbbd4
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeedes
ms.openlocfilehash: f1c7c3cf850ed48412c8a232e364f927248ed9bf
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811135"
---
# <a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Tutorial: Integración de Azure Active Directory con ClickTime

En este tutorial, aprenderá a integrar ClickTime con Azure Active Directory (Azure AD).

La integración de ClickTime con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a ClickTime.
- Puede habilitar que los usuarios inicien sesión automáticamente en ClickTime (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ClickTime, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en ClickTime

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar ClickTime desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-clicktime-from-the-gallery"></a>Agregar ClickTime desde la galería
Para configurar la integración de ClickTime en Azure AD, debe agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ClickTime desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **ClickTime**, seleccione **ClickTime** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![ClickTime en la lista de resultados](./media/clicktime-tutorial/tutorial_clicktime_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con ClickTime con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ClickTime para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ClickTime.

Para establecer la relación de vínculo, en ClickTime, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con ADP ClickTime, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de ClickTime](#create-a-clicktime-test-user)**: para tener un homólogo de Britta Simon en ClickTime que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación ClickTime.

**Para configurar el inicio de sesión único de Azure AD con ClickTime, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **ClickTime**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/clicktime-tutorial/tutorial_clicktime_samlbase.png)

1. En la sección **Dominio y direcciones URL de ClickTime**, lleve a cabo los pasos siguientes:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de ClickTime](./media/clicktime-tutorial/tutorial_clicktime_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL como: `https://app.clicktime.com/sp/`
    
    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con los siguientes patrones: 

    | |
    |--|
    | `https://app.clicktime.com/Login/` |
    | `https://app.clicktime.com/App/Login/Consume.aspx` |

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/clicktime-tutorial/tutorial_clicktime_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/clicktime-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de ClickTime**, haga clic en **Configurar ClickTime** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configuración de ClickTime](./media/clicktime-tutorial/tutorial_clicktime_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de ClickTime como administrador.

1. En la barra de herramientas de la parte superior, haga clic en**Preferencias** y luego haga clic en **Configuración de seguridad**.

1. En la sección de configuración **Preferencias de inicio de sesión único** , siga estos pasos:
   
    ![Configuración de seguridad](./media/clicktime-tutorial/tic777280.png "Configuración de seguridad")
   
     a.  Seleccione **Allow** sign-in using Single Sign-On (SSO) with **Azure AD** [Permitir inicio de sesión mediante inicio de sesión único (SSO) con Azure AD].
   
    b. En el cuadro de texto **Identity Provider Endpoint** (Punto de conexión del proveedor de identidades), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que copió desde Azure Portal.
   
    c.  Abra el **certificado codificado en Base 64** que descargó de Azure Portal en el **Bloc de notas**, copie el contenido y, luego, péguelo en el cuadro de texto **X.509 Certificate** (Certificado X.509).
   
    d.  Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/clicktime-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.
    
    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/clicktime-tutorial/create_aaduser_02.png) 

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.
 
    ![Botón Agregar](./media/clicktime-tutorial/create_aaduser_03.png) 

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:
 
    ![Cuadro de diálogo Usuario](./media/clicktime-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-clicktime-test-user"></a>Creación de un usuario de prueba de ClickTime

Para permitir que los usuarios de Azure AD inicien sesión en ClickTime, deben aprovisionarse en ClickTime.  
En el caso de ClickTime, el aprovisionamiento es una tarea manual.

> [!NOTE]
> Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de ClickTime ofrecida por ClickTime para aprovisionar cuentas de usuario de Azure AD.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**
1. Inicie sesión en su inquilino de **ClickTime** .
1. En la barra de herramientas de la parte superior, haga clic en **Compañía** y, luego, en **Contactos**.
   
    ![Personas](./media/clicktime-tutorial/tic777282.png "Personas")
1. Haga clic en **Add Person**(Agregar persona).
   
    ![Agregar persona](./media/clicktime-tutorial/tic777283.png "Agregar persona")
1. En la sección New Person (Nueva persona), lleve a cabo estos pasos:
   
    ![Personas](./media/clicktime-tutorial/tic777284.png "Personas")
   
     a.  En el cuadro de texto de **nombre completo**, escriba el nombre completo de un usuario, por ejemplo, **Britta Simon**. 
  
    b.  En el cuadro de texto de **correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, **brittasimon@contoso.com**.
       
    > [!NOTE]
    > Si lo desea, puede establecer propiedades adicionales del nuevo objeto persona.
   
    c.  Haga clic en **Save**(Guardar).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ClickTime.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a ClickTime, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **ClickTime**.

    ![Vínculo a ClickTime en la lista de aplicaciones](./media/clicktime-tutorial/tutorial_clicktime_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ClickTime en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación ClickTime.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/clicktime-tutorial/tutorial_general_01.png
[2]: ./media/clicktime-tutorial/tutorial_general_02.png
[3]: ./media/clicktime-tutorial/tutorial_general_03.png
[4]: ./media/clicktime-tutorial/tutorial_general_04.png

[100]: ./media/clicktime-tutorial/tutorial_general_100.png

[200]: ./media/clicktime-tutorial/tutorial_general_200.png
[201]: ./media/clicktime-tutorial/tutorial_general_201.png
[202]: ./media/clicktime-tutorial/tutorial_general_202.png
[203]: ./media/clicktime-tutorial/tutorial_general_203.png

