---
title: 'Tutorial: Integración de Azure Active Directory con Mixpanel | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Mixpanel.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4595202aa60a2b8888487d505aa8981c6f45bd8d
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56189028"
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a>Tutorial: Integración de Azure Active Directory con Mixpanel

En este tutorial, obtendrá información sobre cómo integrar Mixpanel con Azure Active Directory (Azure AD).

Integrar Mixpanel con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Mixpanel.
- Puede permitir que los usuarios inicien sesión automáticamente en Mixpanel (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Mixpanel, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Mixpanel

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Mixpanel desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-mixpanel-from-the-gallery"></a>Adición de Mixpanel desde la galería
Para configurar la integración de Mixpanel en Azure AD, deberá agregar Mixpanel desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Mixpanel desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Mixpanel**.

    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/tutorial_mixpanel_search.png)

1. En el panel de resultados, seleccione **Mixpanel** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Mixpanel con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Mixpanel para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Mixpanel.

Para establecer la relación de vínculo, en Mixpanel, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Mixpanel, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Mixpanel](#creating-a-mixpanel-test-user)**: para tener un homólogo de Britta Simon en Mixpanel que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Mixpanel.

**Para configurar el inicio de sesión único de Azure AD con Mixpanel, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Mixpanel**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

1. En la sección **Dominio y direcciones URL de Mixpanel**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_mixpanel_url.png)

     En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `https://mixpanel.com/login/`

    > [!NOTE] 
    > Regístrese en [https://mixpanel.com/register/](https://mixpanel.com/register/) para configurar las credenciales de inicio de sesión y póngase en contacto con el [equipo de soporte técnico de Mixpanel](mailto:support@mixpanel.com) para habilitar la configuración de SSO del inquilino. Si es necesario, también puede obtener el valor de la dirección URL de inicio de sesión del equipo de soporte de Mixpanel. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Mixpanel**, haga clic en **Configurar Mixpanel** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_mixpanel_configure.png) 

1. En una ventana de explorador diferente, inicie sesión en su aplicación de Mixpanel como administrador.

1. En la parte inferior de la página, haga clic en el pequeño icono de **engranaje** de la esquina izquierda. 
   
    ![Inicio de sesión único de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_06.png) 

1. Haga clic en la pestaña **Access security** (Seguridad de acceso) y después en **Change settings** (Cambiar la configuración).
   
    ![Configuración de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_08.png) 

1. En la página de diálogo **Change your certificate** (Cambiar su certificado), haga clic en **Choose file** (Elegir archivo) para cargar el certificado descargado y después haga clic en **Next** (Siguiente).
   
    ![Configuración de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_09.png) 

1.  En el cuadro de texto Authentication URL (Dirección URL de autenticación) de la página de diálogo **Change your authentication URL** (Cambiar la dirección URL de autenticación), pegue el valor de **SAML Single Sign-On Service URL** (Dirección URL del servicio de inicio de sesión único de SAML) que ha copiado desde Azure Portal y haga clic en **NEXT** (Siguiente).
   
   ![Configuración de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_10.png) 

1. Haga clic en **Done**(Listo).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/mixpanel-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-mixpanel-test-user"></a>Creación de un usuario de prueba de Mixpanel

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Mixpanel. 

1. Inicie sesión en su sitio de la compañía de Mixpanel como administrador.

1. En la parte inferior de la página, haga clic en el pequeño icono de engranaje en la esquina izquierda para abrir la ventana **Configuración** .

1. Haga clic en la pestaña **Equipo** .

1. En el cuadro de texto de **miembro del equipo** , escriba la dirección de correo electrónico de Britta en Azure.
   
    ![Configuración de Mixpanel](./media/mixpanel-tutorial/tutorial_mixpanel_11.png) 

1. Haga clic en **Invitar**. 

> [!Note]
> El usuario recibirá un correo electrónico para configurar el perfil.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Mixpanel para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Mixpanel, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Mixpanel**.

    ![Configurar inicio de sesión único](./media/mixpanel-tutorial/tutorial_mixpanel_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Mixpanel en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Mixpanel.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/mixpanel-tutorial/tutorial_general_203.png

