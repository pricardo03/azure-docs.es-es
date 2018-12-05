---
title: 'Tutorial: Integración de Azure Active Directory con Kantega SSO para Bitbucket | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Kantega SSO para Bitbucket.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c41cdaaf-0441-493c-94c7-569615b7b1ab
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 6dee106d688d9f9a6ebc6dc26caa6a46db3f6850
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "51823901"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bitbucket"></a>Tutorial: Integración de Azure Active Directory con Kantega SSO para Bitbucket

En este tutorial obtendrá información sobre cómo integrar Kantega SSO para Bitbucket con Azure Active Directory (Azure AD).

La integración de Kantega SSO para Bitbucket con Azure AD le proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Kantega SSO para Bitbucket.
- Puede permitir que los usuarios inicien sesión automáticamente en Kantega SSO para Bitbucket (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Kantega SSO para Bitbucket, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Kantega SSO para Bitbucket

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Kantega SSO para Bitbucket desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-kantega-sso-for-bitbucket-from-the-gallery"></a>Incorporación de Kantega SSO para Bitbucket desde la galería
Para configurar la integración de Kantega SSO para Bitbucket en Azure AD, tiene que agregar Kantega SSO para Bitbucket desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Kantega SSO para Bitbucket desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Kantega SSO para Bitbucket**.

    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_search.png)

1. En el panel de resultados, seleccione **Kantega SSO para Bitbucket** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Kantega SSO para Bitbucket con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Kantega SSO para Bitbucket para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Kantega SSO para Bitbucket.

Para establecer la relación de vínculo, en Kantega SSO para Bitbucket, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Kantega SSO para Bitbucket, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Kantega SSO para Bitbucket](#creating-a-kantega-sso-for-bitbucket-test-user)**: para tener un homólogo de Britta Simon en Kantega SSO para Bitbucket que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Kantega SSO para Bitbucket.

**Para configurar el inicio de sesión único de Azure AD con Kantega SSO para Bitbucket, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Kantega SSO para Bitbucket**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_samlbase.png)

1. En el modo de inicio de **IDP**, en la sección **Dominio y direcciones URL de Kantega SSO para Bitbucket** realice el paso siguiente:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url1.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`.

1. En el modo de inicio de **SP**, active la casilla **Mostrar configuración avanzada de URL** y realice el siguiente paso:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_url2.png)
    
    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`.

    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Estos valores se reciben durante la configuración del complemento de Bitbucket, que se explica más adelante en el tutorial.

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_general_400.png)

1. En otra ventana del explorador web, inicie sesión como administrador en el portal de administración de Bitbucket.

1. Haga clic en el icono de engranaje y, luego, en **Find new add-ons** (Buscar nuevos complementos).

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon1.png)

1. Busque **Kantega SSO para Bitbucket (SAML & Kerberos)** y haga clic en el botón **Instalar** para instalar el nuevo complemento SAML.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon2.png)

1. Se inicia la instalación del complemento.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon31.png)

1. Una vez completada la instalación. Haga clic en **Cerrar**.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon33.png)

1.  Haga clic en **Administrar**.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon34.png)
    
1. Haga clic en **Configurar** para configurar el nuevo complemento. 

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon35.png)

1. En la sección **SAML**. Seleccione **Azure Active Directory (Azure AD)** en la lista desplegable **Agregar proveedor de identidades**.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon4.png)

1. Seleccione el nivel de suscripción **Básica**.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon5.png)

1. En la sección **Agregar propiedades**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon6.png)

     a. Copie el valor **URI de id. de aplicación** y úselo como **Identificador, Dirección URL de respuesta y Dirección URL de inicio de sesión** en la sección **Dominio y direcciones URL de Kantega SSO para Bitbucket** en Azure Portal.

    b. Haga clic en **Next**.

1. En la sección **Importar metadatos**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon7.png)

     a. Seleccione **Archivo de metadatos en el equipo** y cargue el archivo de metadatos que descargó desde Azure Portal.

    b. Haga clic en **Next**.

1. En la sección**Name and SSO location** (Nombre y ubicación de SSO), siga estos pasos:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon8.png)

     a. Agregue el nombre del proveedor de identidades en el cuadro de texto **Nombre del proveedor de identidades** (por ejemplo, Azure AD).

    b. Haga clic en **Next**.

1. Compruebe el certificado de firma y haga clic en **Siguiente**.   

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon9.png)

1. En la sección **Cuentas de usuario de Bitbucket**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon10.png)

     a. Seleccione **Create users in Bitbucket's internal Directory if needed** (Crear usuarios en el directorio interno de Bitbucket si es necesario) y escriba el nombre adecuado del grupo de usuarios (puede ser un número múltiple de grupos separados por coma).

    b. Haga clic en **Next**.

1. Haga clic en **Finalizar**

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon11.png)

1. En la sección **Known domains for Azure AD** (Dominios conocidos para Azure AD), siga estos pasos:  

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/addon12.png)

     a. Seleccione **Known domains** (Dominios conocidos) en el panel izquierdo de la página.

    b. Escriba el nombre de dominio en el cuadro de texto **Known domains** (Dominios conocidos).

    c. Haga clic en **Save**(Guardar).  

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/kantegassoforbitbucket-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-kantega-sso-for-bitbucket-test-user"></a>Creación de un usuario de prueba de Kantega SSO para Bitbucket

Para permitir que los usuarios de Azure AD inicien sesión en Bitbucket, deben aprovisionarse a Bitbucket. En Kantega SSO para Bitbucket, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su sitio de la compañía de Bitbucket como administrador.

1. Haga clic en el icono de configuración.

    ![Agregar empleado](./media/kantegassoforbitbucket-tutorial/user1.png) 

1. En la sección de la pestaña **Administración**, haga clic en **Usuarios**.

    ![Agregar empleado](./media/kantegassoforbitbucket-tutorial/user2.png)

1. Haga clic en **Crear usuario**.

    ![Agregar empleado](./media/kantegassoforbitbucket-tutorial/user3.png)   

1. En la página del cuadro de diálogo **Crear usuario**, realice los pasos siguientes:

    ![Agregar empleado](./media/kantegassoforbitbucket-tutorial/user4.png) 

     a. En el cuadro de texto **Nombre de usuario**, escriba el correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.
    
    b. En el cuadro de texto **Nombre completo**, escriba el nombre completo de un usuario, por ejemplo, Britta Simon.
    
    c. En el cuadro de texto **Dirección de correo electrónico**, escriba la dirección de correo electrónico de un usuario, por ejemplo, Brittasimon@contoso.com.

    d. En el cuadro de texto **Contraseña**, escriba la contraseña del usuario.  

    e. En el cuadro de texto **Confirmar contraseña**, escriba nuevamente la contraseña de usuario.

    f. Haga clic en **Crear usuario**.   

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Kantega SSO para Bitbucket.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Kantega SSO para Bitbucket, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Kantega SSO para Bitbucket**.

    ![Configurar inicio de sesión único](./media/kantegassoforbitbucket-tutorial/tutorial_kantegassoforbitbucket_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Kantega SSO para Bitbucket en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Kantega SSO para Bitbucket.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforbitbucket-tutorial/tutorial_general_203.png

