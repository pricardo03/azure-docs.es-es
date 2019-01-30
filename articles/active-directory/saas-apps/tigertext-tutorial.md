---
title: 'Tutorial: Integración de Azure Active Directory con TigerText Secure Messenger | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y TigerText Secure Messenger.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 03f1e128-5bcb-4e49-b6a3-fe22eedc6d5e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.openlocfilehash: 9bb638f2ff4f94ebb6c75a05fd7de772f8f9cdec
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817884"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>Tutorial: Integración de Azure Active Directory con TigerText Secure Messenger

En este tutorial, obtendrá información sobre cómo integrar TigerText Secure Messenger con Azure Active Directory (Azure AD).

La integración de TigerText Secure Messenger con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a TigerText Secure Messenger.
- Puede permitir que los usuarios inicien sesión automáticamente en TigerText Secure Messenger (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con TigerText Secure Messenger, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en TigerText Secure Messenger

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar TigerText Secure Messenger desde la galería
1. Configuración y prueba del inicio de sesión único en Azure AD

## <a name="add-tigertext-secure-messenger-from-the-gallery"></a>Agregar TigerText Secure Messenger desde la galería
Para configurar la integración de TigerText Secure Messenger en Azure AD, será preciso que agregue TigerText Secure Messenger desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar TigerText Secure Messenger desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **TigerText Secure Messenger**, seleccione **TigerText Secure Messenger** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Incorporación de la aplicación desde la galería](./media/tigertext-tutorial/tutorial_tigertext_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD
En esta sección, puede configurar y probar el inicio de sesión único de Azure AD con TigerText Secure Messenger con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de TigerText Secure Messenger para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de TigerText Secure Messenger.

Para establecer la relación de vínculo, en TigerText Secure Messenger, asigne el valor del **nombre de usuario** de Azure AD como el valor del **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con TigerText Secure Messenger, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de TigerText](#create-a-tigertext-secure-messenger-test-user)**: para tener un homólogo de Britta Simon en TigerText Secure Messenger que esté vinculado a la representación de ella en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación TigerText Secure Messenger.

**Para configurar el inicio de sesión único de Azure AD con TigerText Secure Messenger, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **TigerText Secure Messenger**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Inicio de sesión basado en SAML](./media/tigertext-tutorial/tutorial_tigertext_samlbase.png)

1. En la sección **Dominio y direcciones URL de TigerText Secure Messenger**, lleve a cabo los pasos siguientes:

    ![Sección Dominio y direcciones URL de TigerText Secure Messenger](./media/tigertext-tutorial/tutorial_tigertext_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL como: `https://home.tigertext.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://saml-lb.tigertext.me/v1/organization/<instance Id>`

    > [!NOTE] 
    > Este valor no es real. Actualícelo con el identificador real. Póngase en contacto con el [equipo de soporte técnico de TigerText Secure Messenger](mailTo:prosupport@tigertext.com) para obtener este valor. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Sección Certificado de firma SAML](./media/tigertext-tutorial/tutorial_tigertext_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Guardar](./media/tigertext-tutorial/tutorial_general_400.png)

1. Para configurar SSO en su aplicación, póngase en contacto con el [equipo de soporte técnico de TigerText Secure Messenger](mailTo:prosupport@tigertext.com) y proporciónele los **metadatos descargados**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/tigertext-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Usuarios y grupos -> Todos los usuarios](./media/tigertext-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Botón Agregar](./media/tigertext-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Cuadro de diálogo usuario](./media/tigertext-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-tigertext-secure-messenger-test-user"></a>Creación de un usuario de prueba de TigerText Secure Messenger

En esta sección, creará un usuario llamado Britta Simon en TigerText. Póngase en contacto con el [equipo de soporte técnico de TigerText Secure Messenger](mailTo:prosupport@tigertext.com) para agregar los usuarios a la plataforma de TigerText Secure Messenger.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, puede habilitar a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a TigerText Secure Messenger.

![Asignar usuario][200] 

**Para asignar a Britta Simon a TigerText Secure Messenger, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **TigerText Secure Messenger**.

    ![TigerText Secure Messenger en la lista de aplicaciones](./media/tigertext-tutorial/tutorial_tigertext_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de TigerText en el panel de acceso, debería iniciar sesión automáticamente en la aplicación TigerText. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tigertext-tutorial/tutorial_general_01.png
[2]: ./media/tigertext-tutorial/tutorial_general_02.png
[3]: ./media/tigertext-tutorial/tutorial_general_03.png
[4]: ./media/tigertext-tutorial/tutorial_general_04.png

[100]: ./media/tigertext-tutorial/tutorial_general_100.png

[200]: ./media/tigertext-tutorial/tutorial_general_200.png
[201]: ./media/tigertext-tutorial/tutorial_general_201.png
[202]: ./media/tigertext-tutorial/tutorial_general_202.png
[203]: ./media/tigertext-tutorial/tutorial_general_203.png

