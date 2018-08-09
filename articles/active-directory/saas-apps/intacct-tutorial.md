---
title: 'Tutorial: integración de Azure Active Directory con Intacct | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Intacct.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: d834ca75085878350e257cc1c50e60fc1bf28484
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447346"
---
# <a name="tutorial-azure-active-directory-integration-with-intacct"></a>Tutorial: integración de Azure Active Directory con Intacct

En este tutorial, obtendrá información sobre cómo integrar Intacct con Azure Active Directory (Azure AD).

Integrar Intacct con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Intacct.
- Puede permitir que los usuarios inicien sesión automáticamente en Intacct (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Intacct, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Intacct

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Intacct desde la Galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-intacct-from-the-gallery"></a>Incorporación de Intacct desde la Galería
Para configurar la integración de Intacct en Azure AD, será preciso agregar Intacct desde la Galería a la lista de aplicaciones SaaS administradas.

**Para agregar Intacct desde la Galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Intacct**.

    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/tutorial_intacct_search.png)

1. En el panel de resultados, seleccione **Intacct** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/tutorial_intacct_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Intacct con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita saber cuál es el usuario homólogo de Intacct para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Intacct.

Para establecer la relación de vínculo, en Intacct, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Intacct, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Intacct](#creating-an-intacct-test-user)**: para tener un homólogo de Britta Simon en Intacct que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Intacct.

**Para configurar el inicio de sesión único de Azure AD con Intacct, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Intacct**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_intacct_samlbase.png)

1. En la sección **Dominio y direcciones URL de Intacct**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_intacct_url.png)

    En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<companyname>.intacct.com/ia/acct/sso_response.phtml`|
    | `https://www.intacct.com/ia/acct/sso_response.phtml` |

    > [!NOTE] 
    > Este valor no es real. Actualice este valor con la dirección URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico de Intacct](https://us.intacct.com/support) para obtener este valor.

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_intacct_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Intacct**, haga clic en **Configurar Intacct** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_intacct_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Intacct como administrador.

1. Haga clic en la pestaña **Compañía** y luego haga clic en**Información de la compañía**.

    ![Compañía](./media/intacct-tutorial/ic790037.png "Compañía")

1. Haga clic en la pestaña **Seguridad** y luego haga clic en **Editar**.

    ![Seguridad](./media/intacct-tutorial/ic790038.png "Seguridad")

1. En la sección **Inicio de sesión único (SSO)** , siga estos pasos:

    ![Inicio de sesión único](./media/intacct-tutorial/ic790039.png "inicio de sesión único")

    a. Seleccione **Habilitar inicio de sesión único**.

    b. En **Tipo de proveedor de identidades**, seleccione **SAML 2.0**.

    c. En el cuadro de texto **Dirección URL del emisor**, pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal.
   
    d. En el cuadro de texto **Dirección URL de inicio de sesión**, pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    e. Abra el certificado codificado en **base-64** en el Bloc de notas, copie el contenido del mismo en el Portapapeles y luego péguelo en el cuadro de texto **Certificado**.
   
    f. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/intacct-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-an-intacct-test-user"></a>Creación de un usuario de prueba de Intacct

Para configurar los usuarios de Azure AD para que puedan iniciar sesión en Intacct, tienen que aprovisionarse en Intacct. Para Intacct, el aprovisionamiento es una tarea manual.

**Para aprovisionar cuentas de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **Intacct** .

1. Haga clic en la pestaña **Compañía** y luego haga clic en **Usuarios**.

    ![Usuarios](./media/intacct-tutorial/ic790041.png "Usuarios")
1. Haga clic en la pestaña **Agregar**.

    ![Agregar](./media/intacct-tutorial/ic790042.png "Agregar")
1. En la sección **Información del usuario** , lleve a cabo estos pasos:

    ![Información del usuario](./media/intacct-tutorial/ic790043.png "Información del usuario")

    a. Escriba el **Id. de usuario**, **Apellidos**, **Nombre**, **Dirección de correo electrónico**, **Puesto** y **Teléfono** de una cuenta de Azure AD que quiera aprovisionar en la sección **Información del usuario**.

    b. Seleccione **Privilegios administrativos** de una cuenta de Azure AD que quiera aprovisionar.
   
    c. Haga clic en **Save**(Guardar). El titular de la cuenta de Azure AD recibe un mensaje de correo y sigue un vínculo para confirmar su cuenta antes de que se active.

>[!NOTE]
>Para aprovisionar cuentas de usuario de Azure AD, puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Intacct ofrecida por Intacct.
        
### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Intacct para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar la usuaria Britta Simon a Intacct., siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Intacct**.

    ![Configurar inicio de sesión único](./media/intacct-tutorial/tutorial_intacct_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Intacct. en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de Intacct.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/intacct-tutorial/tutorial_general_01.png
[2]: ./media/intacct-tutorial/tutorial_general_02.png
[3]: ./media/intacct-tutorial/tutorial_general_03.png
[4]: ./media/intacct-tutorial/tutorial_general_04.png

[100]: ./media/intacct-tutorial/tutorial_general_100.png

[200]: ./media/intacct-tutorial/tutorial_general_200.png
[201]: ./media/intacct-tutorial/tutorial_general_201.png
[202]: ./media/intacct-tutorial/tutorial_general_202.png
[203]: ./media/intacct-tutorial/tutorial_general_203.png

