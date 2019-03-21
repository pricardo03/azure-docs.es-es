---
title: 'Tutorial: Integración de Azure Active Directory con InsideView | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e InsideView.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: c489a7ab-6b1f-4efb-8a66-8bc13bca78c3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92cb4f1093d8450bb2c32d99f4ccedd0a5688253
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58165924"
---
# <a name="tutorial-azure-active-directory-integration-with-insideview"></a>Tutorial: Integración de Azure Active Directory con InsideView

En este tutorial, obtendrá información sobre cómo integrar InsideView con Azure Active Directory (Azure AD).

La integración de InsideView con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a InsideView
- Puede permitir que los usuarios inicien sesión automáticamente en InsideView (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con InsideView, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Suscripción habilitada para un inicio de sesión único en InsideView

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de InsideView desde la Galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-insideview-from-the-gallery"></a>Incorporación de InsideView desde la Galería
Para configurar la integración de InsideView en Azure AD, será preciso que agregue InsideView desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar InsideView desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **InsideView**.

    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/tutorial_insideview_search.png)

1. En el panel de resultados, seleccione **InsideView** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/tutorial_insideview_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con InsideView con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de InsideView para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de InsideView.

Para establecer la relación de vínculo, en InsideView, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con InsideView, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de InsideView](#creating-a-insideview-test-user)**  : para tener un homólogo de Britta Simon en InsideView que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación InsideView.

**Para configurar el inicio de sesión único de Azure AD con InsideView, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **InsideView**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_insideview_samlbase.png)

1. En la sección **Dominio y direcciones URL de InsideView**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_insideview_url.png)
    
    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://my.insideview.com/iv/<STS Name>/login.iv`.

    > [!NOTE] 
    > Este valor no es real. Actualice este valor con la dirección URL de respuesta real. Póngase en contacto con [equipo de soporte técnico de InsideView](mailto:support@insideview.com) para obtener este valor.
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (sin procesar)** y, a continuación, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_insideview_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de InsideView**, haga clic en **Configurar InsideView** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_insideview_configure.png) 

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía de InsideView.

1. En la barra de herramientas en la parte superior, haga clic en**Admin**, **Configuración de inicio de sesión único** y luego haga clic en**Agregar SAML**.
   
   ![Configuración de inicio de sesión único de SAML](./media/insideview-tutorial/ic794135.png "Cnfiguración de inicio de sesión único de SAML")

1. En la sección **Agregar un nuevo SAML** , realice estos pasos:

    ![Adición de un nuevo SAML](./media/insideview-tutorial/ic794136.png "Adición de un nuevo SAML")
   
     a. En el cuadro de texto **Nombre STS** , escriba el nombre de la configuración.

    b. Copie la  **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal en el cuadro de texto de la **SamlP/WS-Fed Unsolicited EndPoint**.
    
    c. Abra el certificado codificado en base 64 descargado de Azure Portal, copie su contenido en el Portapapeles y péguelo en el cuadro de texto **Certificado STS**.

    d. En el cuadro de texto **Crm User Id Mapping** (Asignación de Id de usuario de Crm), escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
        
    e. En el cuadro de texto **Crm Email Mapping** (Asignación de correo electrónico de Crm), escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    f. En el cuadro de texto **Crm First Name Mapping** (Asignación de nombre de Crm), escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`.
    
    g. En el cuadro de texto **Crm lastName Mapping** (Asignación de apellido de Crm), escriba `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`.  

    h. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 
 
### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/insideview-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-an-insideview-test-user"></a>Creación de un usuario de prueba de InsideView

Para permitir que los usuarios de Azure AD inicien sesión en InsideView, tienen que aprovisionarse en InsideView. En el caso de InsideView, el aprovisionamiento es una tarea manual.

Para obtener los usuarios o contactos creados en InsideView, póngase en contacto con el [equipo de soporte técnico de InsideView](mailto:support@insideview.com).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de InsideView suministrada por InsideView para aprovisionar cuentas de usuario de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a InsideView para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar a Britta Simon a InsideView, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **InsideView**.

    ![Configurar inicio de sesión único](./media/insideview-tutorial/tutorial_insideview_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de InsideView en el panel de acceso, debería iniciar sesión automáticamente en la aplicación InsideView.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/insideview-tutorial/tutorial_general_01.png
[2]: ./media/insideview-tutorial/tutorial_general_02.png
[3]: ./media/insideview-tutorial/tutorial_general_03.png
[4]: ./media/insideview-tutorial/tutorial_general_04.png

[100]: ./media/insideview-tutorial/tutorial_general_100.png

[200]: ./media/insideview-tutorial/tutorial_general_200.png
[201]: ./media/insideview-tutorial/tutorial_general_201.png
[202]: ./media/insideview-tutorial/tutorial_general_202.png
[203]: ./media/insideview-tutorial/tutorial_general_203.png

