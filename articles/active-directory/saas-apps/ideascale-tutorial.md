---
title: 'Tutorial: Integración de Azure Active Directory con IdeaScale | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e IdeaScale.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ecb73e4b520936b573254f2cf209d4a02c0fdd32
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848755"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Tutorial: Integración de Azure Active Directory con IdeaScale

En este tutorial, aprenderá a integrar IdeaScale con Azure Active Directory (Azure AD).

La integración de IdeaScale con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a IdeaScale
- Puede permitir que los usuarios inicien sesión automáticamente en IdeaScale (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con IdeaScale, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en IdeaScale

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de IdeaScale desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-ideascale-from-the-gallery"></a>Adición de IdeaScale desde la galería
Para configurar la integración de IdeaScale en Azure AD, deberá agregar IdeaScale desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar IdeaScale desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **IdeaScale**.

    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/tutorial_ideascale_search.png)

1. En el panel de resultados, seleccione **IdeaScale** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con IdeaScale con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de IdeaScale para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de IdeaScale.

Para establecer la relación de vínculo en IdeaScale, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con IdeaScale, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de IdeaScale](#creating-an-ideascale-test-user)**: para tener un homólogo de Britta Simon en IdeaScale vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación IdeaScale.

**Para configurar el inicio de sesión único de Azure AD con IdeaScale, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **IdeaScale**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

1. En la sección de **Dominio y direcciones URL de IdeaScale**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_ideascale_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.ideascale.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de clientes de IdeaScale](https://support.ideascale.com/). 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de IdeaScale**, haga clic en **Configurar IdeaScale** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **Sign-Out URL, and SAML Entity ID** (Dirección URL de cierre de sesión e identificador de entidad de SAML) de la **sección Referencia rápida**.

    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de IdeaScale como administrador.

1. Vaya a **Configuración de la comunidad**.
   
    ![Configuración de la Comunidad](./media/ideascale-tutorial/ic790847.png "Configuración de la Comunidad")

1. Vaya a **Security \> Single Signon Settings** (Seguridad > Configuración de inicio de sesión único).
   
    ![Configuración de inicio de sesión único](./media/ideascale-tutorial/ic790848.png "Configuración de inicio de sesión único")

1. En **Single-Signon Type** (Tipo de inicio de sesión único), seleccione **SAML 2.0**.
   
    ![Tipo de inicio de sesión único](./media/ideascale-tutorial/ic790849.png "Tipo de inicio de sesión único")

1. En el cuadro de diálogo **Configuración de inicio de sesión único** , siga estos pasos:
   
    ![Configuración de inicio de sesión único](./media/ideascale-tutorial/ic790850.png "Configuración de inicio de sesión único")
   
     a. En el cuadro de texto **SAML IdP Entity ID** (Identificador de entidad del IdP de SAML), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que copió de Azure Portal.

    b. Copie el contenido del archivo de metadatos descargado de Azure Portal y péguelo en el cuadro de texto **SAML IdP Metadata** (Metadatos del IdP de SAML).

    c. En el cuadro de texto **Logout Success URL** (Dirección URL de cierre de sesión correcto), pegue el valor de **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal.

    d. Haga clic en **Guardar cambios**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/ideascale-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-an-ideascale-test-user"></a>Creación de un usuario de prueba de IdeaScale

Para permitir que los usuarios de Azure AD inicien sesión en IdeaScale, deben aprovisionarse en IdeaScale. En el caso de IdeaScale, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía de **IdeaScale** como administrador.

1. Vaya a **Configuración de la comunidad**.
   
    ![Configuración de la Comunidad](./media/ideascale-tutorial/ic790847.png "Configuración de la Comunidad")

1. Vaya a **Basic Settings \> Member Management** (Configuración básica > Administración de miembros).

1. Haga clic en **Agregar miembro**.
   
    ![Administración de miembros](./media/ideascale-tutorial/ic790852.png "Administración de miembros")

1. En la sección Add New Member (Agregar nuevo miembro), lleve a cabo estos pasos:
   
    ![Agregar nuevo miembro](./media/ideascale-tutorial/ic790853.png "Agregar nuevo miembro")
   
     a. En el cuadro de texto **Direcciones de correo electrónico** , escriba la dirección de correo electrónico de la cuenta de AAD válida que quiera aprovisionar.
   
    b. Haga clic en **Guardar cambios**. 
   
    >[!NOTE]
    >El titular de la cuenta de Azure Active Directory recibe un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active.
      
>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de IdeaScale que proporcione IdeaScale para aprovisionar cuentas de usuario de AAD.
 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a IdeaScale.

![Asignar usuario][200] 

**Para asignar Britta Simon a IdeaScale, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **IdeaScale**.

    ![Configurar inicio de sesión único](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 


El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de IdeaScale en el panel de acceso, debería iniciar sesión automáticamente en su aplicación IdeaScale.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

