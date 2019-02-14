---
title: 'Tutorial: Integración de Azure Active Directory con Namely | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Namely.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f78e978e80e27babcf8d2a2736eec06f35aa5e05
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56177842"
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Tutorial: Integración de Azure Active Directory con Namely

En este tutorial, aprenderá a integrar Namely con Azure Active Directory (Azure AD).

La integración de Namely con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Namely.
- Puede permitir que los usuarios inicien sesión automáticamente en Namely (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Namely, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Namely

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Namely desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-namely-from-the-gallery"></a>Adición de Namely desde la galería
Para configurar la integración de Namely en Azure AD, es preciso agregar Namely desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Namely desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Namely**.

    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/tutorial_namely_search.png)

1. En el panel de resultados, seleccione **Namely** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Namely con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Namely para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Namely.

Para establecer la relación de vínculo, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario** de Namely.

Para configurar y probar el inicio de sesión único de Azure AD con Namely, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Namely](#creating-a-namely-test-user)**: el objetivo es tener un homólogo de Britta Simon en Namely que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Namely.

**Para configurar el inicio de sesión único de Azure AD con Namely, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Namely** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_samlbase.png)

1. En la sección **Dominio y direcciones URL de Namely**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.namely.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<subdomain>.namely.com/saml/metadata`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de Namely](https://www.namely.com/contact/) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Namely**, haga clic en **Configurar Namely** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_configure.png) 

1. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Namely como administrador.

1. En la barra de herramientas de la parte superior, haga clic en **Company**(Empresa).
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_06.png) 

1. Haga clic en la pestaña **Configuración** .
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_07.png) 

1. Haga clic en **SAML**.
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_08.png) 

1. En la página **SAML Settings** (Configuración de SAML), realice los pasos siguientes:
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_09.png)
 
     a. Haga clic en **Enable SAML**(Habilitar SAML). 

    b. En el cuadro de texto **Identity provider SSO url** (Dirección URL de inicio de sesión único del proveedor de identidades), pegue el valor de **dirección URL del servicio de inicio de sesión único de SAML**, que ha copiado de Azure Portal.
    
    c. Abra el certificado descargado en el Bloc de notas, copie el contenido y péguelo en el cuadro de texto **Certificado de proveedor de identidades** .
     
    d. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/namely-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-namely-test-user"></a>Creación de un usuario de prueba en Namely

El objetivo de esta sección es crear un usuario llamado Britta Simon en Namely.

**Para crear un usuario llamado Britta Simon en Namely, realice los pasos siguientes:**

1. Inicie sesión en su sitio de la compañía de Namely como administrador.

1. En la barra de herramientas de la parte superior, haga clic en **People**(Gente).
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_10.png) 

1. Haga clic en la pestaña **Directory** (Directorio).
   
    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_11.png) 

1. Haga clic en **Agregar nueva persona**.

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_12.png)

1. En el cuadro de diálogo **Agregar nueva persona** , realice los pasos siguientes:

     a. En el cuadro de texto **Nombre**, escriba **Britta**.

    b. En el cuadro de texto **Apellidos**, escriba **Simon**.

    c. En el cuadro de texto **Correo electrónico**, escriba la **dirección de correo electrónico** de Britta Simon.

    d. Haga clic en **Save**(Guardar).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Namely.

![Asignar usuario][200] 

**Para asignar Britta Simon a Namely, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Namely**.

    ![Configurar inicio de sesión único](./media/namely-tutorial/tutorial_namely_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Namely en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Namely.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/namely-tutorial/tutorial_general_01.png
[2]: ./media/namely-tutorial/tutorial_general_02.png
[3]: ./media/namely-tutorial/tutorial_general_03.png
[4]: ./media/namely-tutorial/tutorial_general_04.png

[100]: ./media/namely-tutorial/tutorial_general_100.png

[200]: ./media/namely-tutorial/tutorial_general_200.png
[201]: ./media/namely-tutorial/tutorial_general_201.png
[202]: ./media/namely-tutorial/tutorial_general_202.png
[203]: ./media/namely-tutorial/tutorial_general_203.png

