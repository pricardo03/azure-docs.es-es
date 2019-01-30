---
title: 'Tutorial: Integración de Azure Active Directory con Igloo Software | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Igloo Software.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: daa00db39ac55cd14b7720b534313407a98dde2c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817748"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Tutorial: Integración de Azure Active Directory con Igloo Software

En este tutorial, obtendrá información sobre cómo integrar Igloo Software con Azure Active Directory (Azure AD).

La integración de Igloo Software con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Igloo Software.
- Puede permitir que los usuarios inicien sesión automáticamente en Igloo Software (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Igloo Software, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Igloo Software

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Igloo Software desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-igloo-software-from-the-gallery"></a>Agregar Igloo Software desde la galería
Para configurar la integración de Igloo Software en Azure AD, deberá agregar Igloo Software desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Igloo Software desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Igloo Software**.

    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/tutorial_igloosoftware_search.png)

1. En el panel de resultados, seleccione **Igloo Software** y haga clic en **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Igloo Software con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, es preciso que Azure AD sepa cuál es el usuario homólogo en Igloo Software de un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Igloo Software.

En Igloo Software, asigne el valor del **nombre de usuario** en Azure AD como valor de **nombre de usuario** para establecer la relación de vínculo.

Para configurar y probar el inicio de sesión único de Azure AD con Igloo Software, debe completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Igloo Software](#creating-an-igloo-software-test-user)**: para tener un homólogo de Britta Simon en Igloo Software que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación de software Igloo HR.

**Para configurar el inicio de sesión único de Azure AD con Igloo Software, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Igloo Software**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

1. En la sección **Dominio y direcciones URL de Igloo Software**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<company name>.igloocommmunities.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<company name>.igloocommmunities.com/saml.digest`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<company name>.igloocommmunities.com/saml.digest`.

    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Igloo Software](https://www.igloosoftware.com/services/support) para obtener estos valores. 

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_general_400.png)
    
1. En la sección **Configuración de Igloo Software**, haga clic en **Configurar Igloo Software** para abrir la ventana **Configurar inicio de sesión**. Copie las **direcciones URL del servicio de inicio de sesión único de SAML y de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

1. En otra ventana del explorador web, inicie sesión como administrador en el sitio de Igloo Software de la compañía.

1. Vaya a **Control Panel**(Panel de control).
   
     ![Panel de control](./media/igloo-software-tutorial/ic799949.png "Panel de control")

1. En la pestaña **Membership** (Pertenencia), haga clic en **Sign In Settings** (Configuración de inicio de sesión).
   
    ![Configuración de inicio de sesión](./media/igloo-software-tutorial/ic783968.png "Configuración de inicio de sesión")

1. En la sección SAML Configuration (Configuración de SAML), haga clic en **Configure SAML Authentication**(Configurar la autenticación de SAML).
   
    ![Configuración de SAML](./media/igloo-software-tutorial/ic783969.png "Configuración de SAML")
   
1. En la sección de **General Configuration** (Configuración general), realice los pasos siguientes:
   
    ![Configuración general](./media/igloo-software-tutorial/ic783970.png "Configuración general")

     a. En el cuadro de texto **Connection Name** (Nombre de conexión), escriba el nombre personalizado de la configuración.
   
    b. En el cuadro de texto **IdP Login URL** (Dirección URL de inicio de sesión de IdP), pegue el valor de la **dirección URL de inicio de sesión único de SAML** que ha copiado de Azure Portal.
   
    c. En el cuadro de texto **IdP Logout URL** (Dirección URL de cierre de sesión de IdP), pegue el valor de la **dirección URL de cierre de sesión** que copió de Azure Portal.
    
    d. Seleccione **Logout Response and Request HTTP Type** (Respuesta de cierre de sesión y tipo de solicitud HTTP) como **POST**.
   
    e. Abra el certificado codificado en **base 64** descargado de Azure Portal en el Bloc de notas, copie su contenido en el Portapapeles y luego péguelo en el cuadro de texto **Certificado público**.
    
1. En **Response and Authentication Configuration**(Configuración de autenticación y respuesta), realice los pasos siguientes:
    
    ![Configuración de autenticación y respuesta](./media/igloo-software-tutorial/IC783971.png "Configuración de autenticación y respuesta")
  
       a. En **Identity Provider** (Proveedor de identidades), seleccione **Microsoft ADFS**.
      
      b. En **Identifier Type** (Tipo de identificador), seleccione **Email Address** (Dirección de correo electrónico). 

      c. En el cuadro de texto **Atributo de correo electrónico**, escriba **emailaddress**.

      d. En el cuadro de texto **First Name Attribute** (Atributo de nombre), escriba **givenname**.

      e. En el cuadro de texto **Last Name Attribute** (Atributo de apellido), escriba **surname**.

1. Realice los pasos siguientes para completar la configuración:
    
    ![Creación de usuario de inicio de sesión](./media/igloo-software-tutorial/IC783972.png "Creación de usuario de inicio de sesión") 

      a. En **User creation on Sign in** (Creación de usuario al inicio de sesión), seleccione **Create a new user in your site when they sign in** (Crear un nuevo usuario en el sitio cuando se inicia sesión).

     b. En **Sign in Settings** (Configuración de inicio de sesión), seleccione **Use SAML button on "Sign in" screen** (Usar botón SAML en la pantalla "Iniciar sesión").

     c. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/igloo-software-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-an-igloo-software-test-user"></a>Creación de un usuario de prueba de Igloo Software

No hay ningún elemento de acción para que configure el aprovisionamiento de usuarios para Igloo Software.  

Cuando un usuario asignado intenta iniciar sesión en Igloo Software desde el panel de acceso, Igloo Software comprueba si el usuario existe.  Si no hay cuentas de usuario disponibles, Igloo Software crea una automáticamente.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Igloo Software.

![Asignar usuario][200] 

**Para asignar un usuario llamado Britta Simon a Igloo Software, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Igloo Software**.

    ![Configurar inicio de sesión único](./media/igloo-software-tutorial/tutorial_igloosoftware_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Igloo Software en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de Igloo Software.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/igloo-software-tutorial/tutorial_general_203.png

